---
title: 建站笔记
date: 2023-04-19 14:27:10
tags: 
- Blog 
- hexo
- nvm
categories:
- cat1
- cat1.1
---

记录一下建立博客过程中关键的步骤和遇到的问题，不一定非常有逻辑。

## update node.js

Ubuntu 中 apt 默认安装的 nodejs 版本为 v10.19.0. 这一版本过低，会导致之后使用 hexo 时报 TypeError 错误。[(参考)](https://stackoverflow.com/questions/67550901/object-fromentries-is-not-a-function-error-when-using-chakra-ui-and-next-js)

为此需要更新 nodejs。因为无法使用 apt 直接更新到更高的版本，需要借助 nvm (Node Version Manager) 实现更新。

### install nvm using curl

使用 curl 复制网页页上的脚本然后执行：

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
```

实际操作时，有时候 curl 的复制速度比较慢，也可以直接到对应网址上手工复制脚本，copy 到本地后执行。

特别需要注意，脚本执行完后需要 reopen 终端后才会看到 "$ nvm --version" 的版本信息。

### Installing NodeJS using NVM

使用 nvm 更新 nodejs

```bash
$ nvm install node
```

未指定更新到哪个版本，默认更新到最新版本：

```bash
$ node --version
v20.0.0
```

完成 nodejs 更新后，即可正常使用 hexo。

More Info: [How to install NodeJs in WSL ?](https://medium.com/@ashkash.aishwarya/how-to-install-nodejs-in-wsl-6129afb2d3e3)

## get hexo

第一次安装hexo之后，第二天重新打开wsl后不知为什么提示不存在hexo工具，因此只能再重新安装。

```bash
$ npm install hexo-cli -g # 全局安装hexo命令行工具
```

## Other stuff

To be continue...