# :no_entry_sign: Git

## :underage: Git配置SSH登录

> [GitHub文档操作指导](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys "GitHub")

### :no_mobile_phones: 客户端生成SSH密钥

```bash
# 打开git bash
ssh-keygen -t ed25519 -C "csjmjy@outlook.com"
# $ ssh-keygen -t ed25519 -C "csjmjy@outlook.com"
# Generating public/private ed25519 key pair.
# Enter file in which to save the key (/c/Users/材料用途的工人/.ssh/id_ed25519):
# Enter passphrase (empty for no passphrase):
# Enter same passphrase again:
# Your identification has been saved in /c/Users/材料用途的工人/.ssh/id_ed25519
# Your public key has been saved in /c/Users/材料用途的工人/.ssh/id_ed25519.pub
# The key fingerprint is:
# SHA256:ih95nmfxKOEDY+wVBQXHeWUtUJih7StoNMxeEymysvg csjmjy@outlook.com
# The key's randomart image is:
# +--[ED25519 256]--+
# |        o=o.oB+. |
# |         .+++.. .|
# |      . ..+..  . |
# |       =.. o     |
# |    ... S.o .    |
# |   . +==o+.. .   |
# |  . oo+==..+.    |
# |   . ..=+.+..    |
# |    E . o=       |
# +----[SHA256]-----+
```

### :no_mobile_phones: 将 SSH 密钥添加到 ssh-agent

```bash
# 在新的_管理员提升_终端窗口(PowerShell 或 CMD)中,确保 ssh-agent 正在运行
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent

# 在无提升权限的终端窗口中,将SSH私钥添加到ssh-agent.
ssh-add C:\Users\材料用途的工人\.ssh\id_ed25519
# Identity added: C:\Users\鏉愭枡鐢ㄩ€旂殑宸ヤ汉\.ssh\id_ed25519 (csjmjy@outlook.com)
```

### :no_mobile_phones: 使用SSH密钥密码

```bash
# 添加或更改密码:Z!3WyrPTjV9zx31r
$ ssh-keygen -p -f ~/.ssh/id_ed25519
# Key has comment 'csjmjy@outlook.com'
# Enter new passphrase (empty for no passphrase):
# Enter same passphrase again:
# Your identification has been saved with the new passphrase.

# 在 Git for Windows 上自动启动 ssh-agent
# 可以在打开 bash 或 Git shell 时自动运行 ssh-agent. 复制以下行并将其粘贴到 Git shell 中的 ~/.profile 或 ~/.bashrc 文件中:
---------------------------------------------------------------------------------------------------
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi
---------------------------------------------------------------------------------------------------
```

### :no_mobile_phones: 新增SSH密钥到GitHub帐户

![1698169077575](image/linux/1698169077575.png)

### :no_mobile_phones: 测试SSH连接

```bash
$ ssh -T git@github.com
# The authenticity of host 'github.com (20.205.243.166)' can't be established.
# ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
# This key is not known by any other names.
# Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
# Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
# Hi mawanxiangone! You've successfully authenticated, but GitHub does not provide shell access.
```

### :no_mobile_phones: 检查现有SSH密钥

```bash
$ ls -al ~/.ssh
# total 38
# drwxr-xr-x 1 材料用途的工人 197121    0 10月 25 01:40 ./
# drwxr-xr-x 1 材料用途的工人 197121    0 10月 25 02:15 ../
# -rw-r--r-- 1 材料用途的工人 197121  464 10月 25 02:00 id_ed25519
# -rw-r--r-- 1 材料用途的工人 197121  100 10月 25 01:20 id_ed25519.pub
# -rw-r--r-- 1 材料用途的工人 197121 1509 10月 25 01:40 known_hosts
# -rw-r--r-- 1 材料用途的工人 197121  776 10月 25 01:40 known_hosts.old
```

## :underage: Git命令

### :no_mobile_phones: git branch

```bash
# 查看本地分支列表
git branch

# 删除分支
git branch -d <分支>

# 强制删除分支(未合并的更改将会丢失)
git branch -D <分支>
```

### :no_mobile_phones: git checkout

```bash
# 切换分支
git checkout main
```

### :no_mobile_phones: git status

```bash

```


## :underage: 远程仓库使用

### :no_mobile_phones: 克隆现有仓库

```bash
$ git clone https://github.com/mawanxiangone/interesting.git
# Cloning into 'interesting'...
# remote: Enumerating objects: 179, done.
# remote: Counting objects: 100% (39/39), done.
# remote: Compressing objects: 100% (35/35), done.
# remote: Total 179 (delta 17), reused 3 (delta 3), pack-reused 140
# Receiving objects: 100% (179/179), 48.00 KiB | 434.00 KiB/s, done.
# Resolving deltas: 100% (53/53), done.
```

### :no_mobile_phones: 仓库分支操作

```bash
# 查看本地分支列表
git branch

# 拉取远程仓库的最新更改
git pull origin main

# 创建分支
git checkout -b world

# 新分支推送到 GitHub 上
git push -u origin world

# 切换分支
git checkout main

# 删除分支
git branch -d <分支>

# 强制删除分支(未合并的更改将会丢失)
git branch -D <分支>

# 更改推送到 GitHub 上
git push origin --delete specification

# 查看本地仓库的更改状态:
git status

```

### :no_mobile_phones: 创建文件

```bash

# 切换分支
git checkout world

# 创建一个文件夹
mkdir world

# 文件夹创建文件
touch.exe world.md

# 新创建的文件夹添加到版本控制
git add world

# 提交更改
git commit -m "Add new folder"
git commit -m "Add new folder and file to repository"

# 将本地的更改推送到GitHub上的分支
git push origin world

# 
```