---
layout: post
title: zsh环境安装
date: '2018-12-15 11:19:54 +0800'
comments: true
categories: zsh
abbrlink: 65257
---

### oh-my-zsh
* 安装地址: https://github.com/robbyrussell/oh-my-zsh
#### 安装
1.clone项目

```bash
git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
```

2.备份原来的 `~/.zshrc`

```bash
cp ~/.zshrc ~/.zshrc.orig
```

3.创建新的zsh配置文件，直接从项目模板文件复制过来

```bash
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

4.设置系统默认shell环境

```bash
chsh -s /bin/zsh
```

5.打开新的命令窗口，它已经加载Oh My Zsh的配置。

#### 主题
* powerlevel9k (https://github.com/bhilburn/powerlevel9k)
* 安装 Powerlevel9k 主题

1.Clone项目到OMZ的`custom/themes`目录.

```bash
$ git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

2.在`~/.zshrc`中设置主题
```bash .zshrc
ZSH_THEME="powerlevel9k/powerlevel9k"
```

#### 字体
* 安装Powerline字体 (https://github.com/powerline/fonts)

```bash
# clone 项目
git clone https://github.com/powerline/fonts.git --depth=1
# 执行根目录下install.sh安装
cd fonts
./install.sh
# 卸载时删掉目录
cd ..
rm -rf fonts
```

* 安装Awesome-Powerline Fonts字体 (https://github.com/gabrielelana/awesome-terminal-fonts)
  1. 参考系统字体安装 https://github.com/gabrielelana/awesome-terminal-fonts/wiki/OS-X
  2. 复制所有`./build`目录下的 `*.sh` 文件到`~/.fonts`目录
  3. 在`~/.zshrc`文件上添加`source ~/.fonts/*.sh`
  4. 在`~/.zshrc`文件上添加`POWERLEVEL9K_MODE='awesome-fontconfig'`

```bash ./zshrc
source ~/.fonts/*.sh
POWERLEVEL9K_MODE='awesome-fontconfig'

ZSH_THEME="powerlevel9k/powerlevel9k"
```

{% img fancybox /images/2018/zsh/shell.png %}

#### 插件
* zsh-autosuggestions (https://github.com/zsh-users/zsh-autosuggestions)

1.clone项目`$ZSH_CUSTOM/plugins` (默认地址 ~/.oh-my-zsh/custom/plugins)

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

2.把插件加入到Oh My Zsh的插件注册表(修改 `~/.zshrc`, 每次修改完 `.zshrc`文件都需要`source .zshrc`)

```bash .zshrc
plugins=(zsh-autosuggestions)
```

3.打开新shell窗口

* autojump (https://github.com/wting/autojump)

1.clone项目

```bash
git clone git://github.com/wting/autojump.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/autojump
```

2.执行安装脚本
```bash
cd autojump
./install.py or ./uninstall.py
```

3.执行完装脚本后之后把提示路径加到`./zshrc`文件中

* zsh-syntax-highlighting (https://github.com/zsh-users/zsh-syntax-highlighting)

1.clone项目

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

```

2.加到插件列表

```bash .zshrc
plugins=( [plugins...] zsh-syntax-highlighting)
```

3.执行 Source `~/.zshrc`

```bash
source ~/.zshrc
```

####VScode中配置
* vscode环境中打开设置(`command+,`),设置终端字体
```json settings.json
  "terminal.integrated.fontFamily": "'Meslo LG M DZ for Powerline'" //终端样式
```

#### 常用命令使用
* `history` 查询历史使用命令
* `j folder` 快速跳转到文件夹



