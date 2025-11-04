---
title: "自动发布学习笔记"
linkTitle: "自动发布"
weight: 61
description: >
  自动发布现有学习笔记到nginx
---

学习笔记有几十个, 平时更新频繁, 内容更新之后往往没有发布最新内容.

因此还是需要一个自动发布的机制, 定期检查各个学习笔记有没有更新, 如果有则触发发布流程, 将最新的内容发布到 nginx 下.

### 目录结构

学习笔记的父目录一般为 `/home/sky/work/code/learning/` , 子目录为 "learning-" 开头的几十个文件夹, 每个子目录是一个独立的 git 仓库.

如:

```bash
$ pwd
/home/sky/work/code/learning
$ ls -l
total 196
drwxr-xr-x  9 sky sky  4096 Oct  9 20:15 learning-golang
drwxr-xr-x  9 sky sky  4096 Oct 21 10:33 learning-grpc
drwxr-xr-x 10 sky sky  4096 Oct 21 10:26 learning-hugo
drwxr-xr-x  9 sky sky  4096 Oct 21 10:29 learning-inoreader
drwxr-xr-x  9 sky sky  4096 Oct 21 10:26 learning-java-aot
drwxr-xr-x  9 sky sky  4096 Oct 21 10:29 learning-jdk
drwxr-xr-x  9 sky sky  4096 Oct 21 10:24 learning-kuasar
drwxr-xr-x  9 sky sky  4096 Oct 21 10:33 learning-linux-mint
......
```

### 脚本内容(串行版本)

串行版本是一个接一个的处理子目录, 因为子目录比较多,所以速度有点慢:

```bash
vi /home/sky/work/code/learning/auto_publish.sh
```

内容为:

```bash
#!/bin/bash
# 自动检查并发布 learning-* 子目录
# 运行环境: Debian 12/13

BASE_DIR="/home/sky/work/code/learning"
PUBLISH_SCRIPT="$BASE_DIR/publish.sh"

# 遍历所有以 learning- 开头的子目录
for repo in "$BASE_DIR"/learning-*; do
    if [ -d "$repo/.git" ]; then
        cd "$repo" || continue
        echo ">>> 检查仓库: $(basename "$repo")"

        # 获取远程更新
        git fetch origin &>/dev/null

        # 判断是否有更新
        LOCAL=$(git rev-parse @)
        REMOTE=$(git rev-parse @{u} 2>/dev/null)
        BASE=$(git merge-base @ @{u} 2>/dev/null)

        if [ "$LOCAL" = "$REMOTE" ]; then
            echo "    已是最新，无需更新"
        elif [ "$LOCAL" = "$BASE" ]; then
            echo "    检测到远程更新，执行 git pull..."
            git pull --ff-only
            echo "    调用发布脚本..."
            "$PUBLISH_SCRIPT" "$(basename "$repo")"
        elif [ "$REMOTE" = "$BASE" ]; then
            echo "    本地有未推送的提交，跳过发布"
        else
            echo "    本地与远程有分歧，请手动处理"
        fi
    fi
done
```

增加执行属性:

```bash
chmod +x /home/sky/work/code/learning/auto_publish.sh
```

跑起来确实比较慢,因为子目录太多了(30上下). 不过可以清晰的看到哪个仓库有更新.

### 脚本内容(并行版本)

并行版本同时检查所有子目录的更新情况, 好处是执行起来特别快,尤其大部分情况下子目录都是没有内容更新的:

```bash
vi /home/sky/work/code/learning/auto_publish_parallel.sh
```

内容为:

```bash
#!/bin/bash
# 自动检查并发布 learning-* 子目录 (并发版本)
# 运行环境: Debian 12/13

BASE_DIR="/home/sky/work/code/learning"
PUBLISH_SCRIPT="$BASE_DIR/publish.sh"

check_and_publish() {
    repo="$1"
    cd "$repo" || exit 1
    repo_name=$(basename "$repo")
    echo ">>> 检查仓库: $repo_name"

    # 获取远程更新
    git fetch origin &>/dev/null

    LOCAL=$(git rev-parse @)
    REMOTE=$(git rev-parse @{u} 2>/dev/null)
    BASE=$(git merge-base @ @{u} 2>/dev/null)

    if [ "$LOCAL" = "$REMOTE" ]; then
        echo "    [$repo_name] 已是最新"
    elif [ "$LOCAL" = "$BASE" ]; then
        echo "    [$repo_name] 检测到远程更新，执行 git pull..."
        git pull --ff-only
        echo "    [$repo_name] 调用发布脚本..."
        "$PUBLISH_SCRIPT" "$repo_name"
    elif [ "$REMOTE" = "$BASE" ]; then
        echo "    [$repo_name] 本地有未推送的提交，跳过发布"
    else
        echo "    [$repo_name] 本地与远程有分歧，请手动处理"
    fi
}

export -f check_and_publish
export BASE_DIR PUBLISH_SCRIPT

# 遍历所有 learning-* 子目录并发执行
for repo in "$BASE_DIR"/learning-*; do
    if [ -d "$repo/.git" ]; then
        check_and_publish "$repo" &
    fi
done

# 等待所有后台任务完成
wait
echo ">>> 所有仓库检查完成"
```

增加执行属性:

```bash
chmod +x /home/sky/work/code/learning/auto_publish_parallel.sh
```

手工执行:

```bash
/home/sky/work/code/learning/auto_publish_parallel.sh
```

输出为:

```bash
$ ./auto_publish_parallel.sh 

>>> 检查仓库: learning-ai-agent
>>> 检查仓库: learning-durabletask
>>> 检查仓库: learning-golang
>>> 检查仓库: learning-computer-hardware
>>> 检查仓库: learning-debian
>>> 检查仓库: learning-grpc
>>> 检查仓库: learning-cloudstate
>>> 检查仓库: learning-esxi
>>> 检查仓库: learning-distributed-state
>>> 检查仓库: learning-datamesh
>>> 检查仓库: learning-inoreader
>>> 检查仓库: learning-mockito
>>> 检查仓库: learning-macos
>>> 检查仓库: learning-git
>>> 检查仓库: learning-openwrt
>>> 检查仓库: learning-cloud-hypervisor
>>> 检查仓库: learning-kuasar
>>> 检查仓库: learning-distributed-transaction
>>> 检查仓库: learning-linux-mint
>>> 检查仓库: learning-hugo
>>> 检查仓库: learning-python
>>> 检查仓库: learning-jdk
>>> 检查仓库: learning-rust
>>> 检查仓库: learning-java-aot
>>> 检查仓库: learning-tokio
>>> 检查仓库: learning-spring-cloud
>>> 检查仓库: learning-ubuntu-server
>>> 检查仓库: learning-windows-server
>>> 检查仓库: learning-windows11
>>> 检查仓库: learning-photoshop
>>> 检查仓库: learning-pve
>>> 检查仓库: learning-serverless
>>> 检查仓库: learning-vscode
    [learning-jdk] 已是最新
    [learning-grpc] 已是最新
    [learning-kuasar] 已是最新
    [learning-windows-server] 已是最新
    [learning-computer-hardware] 已是最新
    [learning-vscode] 已是最新
    [learning-cloudstate] 已是最新
    [learning-hugo] 已是最新
    [learning-windows11] 已是最新
    [learning-mockito] 已是最新
    [learning-java-aot] 已是最新
    [learning-tokio] 已是最新
    [learning-esxi] 已是最新
    [learning-cloud-hypervisor] 已是最新
    [learning-python] 已是最新
    [learning-rust] 已是最新
    [learning-durabletask] 已是最新
    [learning-golang] 已是最新
    [learning-pve] 已是最新
    [learning-inoreader] 已是最新
    [learning-macos] 已是最新
    [learning-linux-mint] 已是最新
    [learning-spring-cloud] 已是最新
    [learning-openwrt] 已是最新
    [learning-distributed-state] 已是最新
    [learning-debian] 已是最新
    [learning-git] 已是最新
    [learning-ubuntu-server] 已是最新
    [learning-distributed-transaction] 已是最新
    [learning-datamesh] 已是最新
    [learning-serverless] 已是最新
    [learning-photoshop] 已是最新
    [learning-ai-agent] 已是最新
>>> 所有仓库检查完成
```

没有内容更新的情况下, 大概15到20秒就检查完成,速度快多了.

如果多个仓库同时更新, 可能会有并发执行发布人物的性能问题,不过通常我不太会在10分钟内更新多个仓库的内容,所以就不额外处理了(比如设置最大并发数)

### 设置定时任务

设置定时任务, 每 10 分钟执行一次.

编辑 crontab：

```bash
crontab -e
```

添加一行：

```properties
*/10 * * * * /bin/bash /home/sky/work/code/learning/auto_publish_parallel.sh >> /home/sky/work/code/learning/auto_publish_parallel.log 2>&1
```

这样脚本会每 10 分钟运行一次，并把日志写到 auto_publish.log。

如果不想打印日志:

```properties
*/10 * * * * /bin/bash /home/sky/work/code/learning/auto_publish_parallel.sh >/dev/null 2>&1
```

注意: 用 sky 帐号执行 `crontab -e`,不要用 root 帐号或者 sudo 执行 `crontab -e`, 那样编辑的是 root 帐号的 cron,执行命令是用的是 root 用户来执行自动发布版本. 而我的 git 仓库一般都是用 sky 帐号 clone 的. 在新版本的 git 中会报错:

```bash
fatal: detected dubious ownership in repository at '/home/sky/work/code/learning/learning-windows-server' 
To add an exception for this directory, call: 

    git config --global --add safe.directory /home/sky/work/code/learning/learning-windows-server
```

