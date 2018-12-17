---
layout: post
title: Vue环境搭建
date: '2018-11-05 12:20:23 +0800'
comments: true
categories: vue
abbrlink: 32040
---

## vsCode插件安装
* 安装插件： https://zhangzhenfei.github.io/15337782581565.html

<!-- more -->

## 授权安装目录
* 执行`sudo chown -R $(whoami) /usr/local/lib`目录授权

### 安装nvm (node版本管理)
* `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash` #执行后添加nvm添加环境变量

```
=> Profile not found. Tried ~/.bashrc, ~/.bash_profile, ~/.zshrc, and ~/.profile.
=> Create one of them and run this script again
   OR
=> Append the following lines to the correct file yourself:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm

=> You currently have modules installed globally with `npm`. These will no
=> longer be linked to the active version of Node when you install a new node
=> with `nvm`; and they may (depending on how you construct your `$PATH`)
=> override the binaries of modules installed with `nvm`:

/usr/local/lib
├── @angular/cli@1.7.2
├── @vue/cli@3.0.0-beta.2
├── @vue/cli-init@3.0.0-beta.2
├── ionic@3.19.1
├── node-sass@4.7.2
└── whistle@1.10.10
=> If you wish to uninstall them at a later point (or re-install them under your
=> `nvm` Nodes), you can remove them from the system Node as follows:

     $ nvm use system
     $ npm uninstall -g a_module

=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
```

* 添加nvm到环境变量
  * ~~两个环境 `vim ~/.bash_profile`与`vim ~/.zshrc` 添加文档最末，重启shell~~
  * 执行vi ~/.zshrc打开.zshrc,将`source $HOME/.bash_profile`粘贴到最下面，保存即可。

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```
* `nvm ls-remote` 列表可安装的Node.js版本
* 执行 `nvm install 8.11.4` 安装node指定版本
* 执行 `nvm uninstall 8.11.4` 卸载node指定版本
* 执行 `nvm use 8.11.4` 设置shell的node版本
* `nvm deactivate` 还原环境变量PATH

#### .nvmrc
1. `echo 6.2.1 > .nvmrc` 它存储在工程根目录中，用于记录该工程依赖的Node.js版本
2. 进入工程目录（当前目录），运行 `nvm use` ,将根据.nvmrc指定shell的Nodejs版本

* 执行`npm i -g cnpm`安装淘宝源

### 使用nrm安装
* `npm install -g nrm` #安装nrm
* `nrm add syt http://npm.sythealth.com` #添加公司的npm镜像地址
* `nrm use syt` #使用公司npm地址
* `nrm ls`查看源

```
  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
* syt ---- http://npm.sythealth.com/
```

### 使用yarn安装
* `npm i -g yarn` *yarn相当于是npm的升级*
* `npm i -g yrm`
* `yrm add syt http://npm.sythealth.com`
* `yrm ls` #

```
* npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
  yarn --- https://registry.yarnpkg.com
  syt ---- http://npm.sythealth.com/
```
* yrm use syt

```
  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
  yarn --- https://registry.yarnpkg.com
* syt ---- http://npm.sythealth.com/
```

* 进入项目根目录
* 执行 ```yarn``` 安装项目依赖
* 执行下面安装chromedriver谷歌驱动

```
npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
```

### 解决项目安装chromedriver失败问题

* 使用npm解决方式

```
npm config set chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
```

* 使用yarn解决方式

```
yarn config set "chromedriver_cdnurl" "https://npm.taobao.org/mirrors/chromedriver"
```

### APPStore安装Helm,切换host环境
### chrome浏览器安装vue插件

### 项目启动
* `npm run dev` #启动项目