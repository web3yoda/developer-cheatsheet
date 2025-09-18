---
weight: 101
title: gh cli
---

## gh cli

相关链接: 
* https://cli.github.com/
* https://github.com/cli/cli

> git不用PAT或者SSHKEY, 使用sso认证, 更安全方便

```shell

# 根据提示，选Github.com > 选HTTPS > 选Y > 跳转到github认证
gh auth login

# 认证完成后git就可以push了
git push

# 检查认证状态
gh auth status 
github.com
  ✓ Logged in to github.com as web3yoda (/Users/user/.config/gh/hosts.yml)
  ✓ Git operations for github.com configured to use https protocol.
  ✓ Token: *******************
```

> 本地github多账号随意切换(通过环境变量GH_CONFIG_DIR)

```shell
# 检查认证状态
gh auth status 
github.com
  ✓ Logged in to github.com as web3yoda (/Users/user/.config/gh/hosts.yml)
  ...

# 更改配置保存位置的环境变量 https://cli.github.com/manual/gh_help_environment
export GH_CONFIG_DIR=${HOME}/.config/.gh2

# 检查认证状态处于未认证状态
gh auth status
You are not logged into any GitHub hosts. Run gh auth login to authenticate.

# 使用第二个github user认证
gh auth login

# 检查认证状态为已认证及配置文件路径为 ~/.config/.gh2
gh auth status
github.com
  ✓ Logged in to github.com as web3yoda (/Users/user/.config/.gh2/hosts.yml)
  ...

# gh可以通过GH_CONFIG_DIR切换用户session了，但是 git还不能，可以这样切换，让git使用第2个用户的token
gh auth setup-git

# 两条命令两个账户来回切换
# 切换第二个账户：
export GH_CONFIG_DIR=${HOME}/.config/.gh2; gh auth setup-git
# 操作完后，切换回主账户
unset GH_CONFIG_DIR; gh auth setup-git

# 也可以通过 git switch切换不同user

git auth switch

git auth status

git auth setup-git

```

> gh 触发一个支持 workflow_dispatch 的action workflow
```shell

# trigger 一次 run
gh workflow run my-workflow.yaml -R web3yoda/example -f input1=value1 -f input2=value2

# 列出所有的 run的记录
gh run list -R web3yoda/example --workflow my-workflow.yaml

# 查看run的日志
gh run view  -R web3yoda/example --log 5000000001

# 

```