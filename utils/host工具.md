# SwitchHosts使用简介

SwitchHosts是一个用于快速切换 hosts 文件的小程序，基于 Electron 开发，同时使用了 React、Ant Design 以及 CodeMirror 等框架/库。

## 项目地址

<https://github.com/oldj/SwitchHosts>

## 安装

```bash
brew cask install switchhosts
```

或直接在github上下载应用手动安装

## 基本使用

基本使用不多讲了，UI界面的工具，简单明了。下面讲一下如何添加toop在青云的资源的常用hosts

打开程序界面点击左下角"+"标志在弹出的窗口上选择<远程>类型。<Hosts 方案名>随意填个喜欢的名字，比如test。<URL 地址>填：<http://192.168.10.3/hosts/test>（此资源需要先连接vpn）。<自动更新>目前最频繁的是1小时，此栏右边有一个刷新标志，可以手动立即刷新。点击确定后回到主界面，左侧出现test这个hosts方案，如果此时没有看到任何hosts配置，点击此方案，出现一个笔图标，点击笔进入修改，手动更新一下。新增的方案默认为关闭状态，左侧的每个hosts方案都有一个独立的开关控制，点击即可随意组合使用各个方案。