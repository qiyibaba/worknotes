# mac工具使用手册

## github登录慢

获取ip地址：

[github](https://ipaddress.com/website/github.com#ipinfo)

[safe](https://ipaddress.com/website/github.global.ssl.fastly.net#ipinfo)

添加内容到/etc/hosts

151.101.121.194 github.global.ssl.fastly.net

140.82.121.4 github.com

## pip install指定国内源

```shell
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple/ pyDes
```

## brew指定国内源

```shell
# 查看 brew.git 当前源
cd "$(brew --repo)" && git remote -v
##origin    https://github.com/Homebrew/brew.git (fetch)
##origin    https://github.com/Homebrew/brew.git (push)

# 查看 homebrew-core.git 当前源
cd "$(brew --repo homebrew/core)" && git remote -v
##origin    https://github.com/Homebrew/homebrew-core.git (fetch)
##origin    https://github.com/Homebrew/homebrew-core.git (push)

# 替换为中科大源
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

# zsh 替换 brew bintray 镜像
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc

# bash 替换 brew bintray 镜像
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

# 刷新源
brew update
```

