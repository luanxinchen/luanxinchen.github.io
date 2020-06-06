---
title: Git代理的使用和取消
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-24 10:21:26
photos: [
    ["https://github.blog/wp-content/uploads/2019/08/DL-V2-LinkedIn_FB.png"]
]
tags: [Git,Proxy,网络]
categories: [Git]
---

因为git被半墙的原因，部署Hexo时一直卡在git，使用`npm config set registry`更换npm为淘宝源后，又卡在git clone landscape ，landscape是hexo的默认主题，托管在 Github上，clone 的指令是内置在 hexo-cli 的 install.js 中的。这个时候就算 npm 换源，这一步也绕不过去，看着5Kb/s的clone速度忍不住再一次对GFW爆粗，无奈只能使用代理服务器
<!-- more -->

## 设置代理

*假设代理服务器ip为127.0.0.1*

http/https
```
git config --global https.proxy https://127.0.0.1:1080
git config --global http.proxy http://127.0.0.1:1080
```

ss/ssr
```
git config --global https.proxy 'socks5://127.0.0.1:1080'
git config --global http.proxy 'socks5://127.0.0.1:1080'
```

## 取消代理

```
git config --global --unset http.proxy
git config --global --unset https.proxy
npm config delete proxy
```