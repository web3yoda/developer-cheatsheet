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
```