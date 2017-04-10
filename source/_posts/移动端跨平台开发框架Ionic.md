---
title: 移动端跨平台开发框架Ionic
date: 2017-03-01 22:10:04
tags:
---
## Ionic
Ionic是一款接近原生的HTML5移动App开发框架，使用Ionic可以迅速开发出一些轻量级的混合应用，你可以像构建网站一样开发应用，而Ionic框架可以把你的“网站”应用Android、IOS以及windows Phone等移动设备上。
Ionic的最新版本是Ionic2 beta ，他提供了丰富的UI组件来帮助开发者开发强大的应用，使用JavaScript MVVM框架和Angular2 框架来增强应用。使用Ionic开发应用，首先要学习的是HTML+CSS+JavaScript，这是标准的网页开发技术，然后还需要学习Angular框架，目前Angular2 使用TypeScript语言来开发。掌握了这些基本技术后，就可以开发你自己的混合应用了。
## Ionic 2 的安装和Hello World
安装Ionic 首先要安装NodeJS，使用NodeJS提供的npm工具来安装。在Mac或者Linux上需要加上sudo。-g表示全局安装。

    $ npm install -g ionic@beta
新建Ionic项目

    $ ionic start myIonicApp --v2
进入到新建项目目录下

    $ cd myIonicApp
在浏览器上预览新建的项目，Ionic默认的为开发者新建了几个页面，相当于其他框架的Hello World。使用ionic serve命令便可以在浏览器上预览效果了。

    $ ionic serve
在移动设备或者模拟器上运行应用。Ionic应用可以运行在iOS simulator、Android simulator、Genymotion 和手机设备上。要运行在设备上的时候需要安装Cordova，Cordova可以将标准的HTML/CSS/JS 转换成能在native App上运行的文件，Cordova提供了JavaScript API ，使得Ionic应用可以手机设备的native功能，比如照相机、传感器、相册等。Cordova也提供了将Ionic应用build为IOS、Android、Windows phone应用的工具。
安装 Cordova

    $ npm install -g cordova
运行iOS前要添加iOS平台：

    $ ionic platform add ios
在模拟器上运行：

    $ ionic emulate ios
在iOS设备上运行：运行条件是首先要安装Xcode工具

    $ ionic run ios

运行在Android模拟器及设备上：

    $ ionic platform add android

然后需要安装Android SDK ，开发过native Android 应用的都会使用到，或者使用Genymotion，一款更快的Android模拟器设备。安装好后运行下面命令，就可以在模拟器上运行自己的App了。

    $ ionic emulate android
在Android设备上运行：

    $ ionic run android
## 开发工具

最常见的就是使用Visual Studio Code 来进行开发调试Ionic工程，在VS code 中可以设置断点来调试项目，调整页面布局的时候可以先在浏览器中调试，我使用的是chrome浏览器，chrome的开发者工具为开发者提供了丰富的调试工具。在开发者工具中的Elements中可以看到网页源代码，可以更好的让你修改HTML及CSS样式。Network中可以看到网络请求的相关参数。console中可以打印日志。

## 总结
工欲善其事必先利其器，框架选好，开发工具准备好后，就可以快乐的进行开发了，在项目中边写边学，这是一个不断学习完善的过程，其中避免不了要走一些弯路，入一些深坑，希望能在今后好好做好总结，不断提升自己的技术。从Android native开发转向Ionic应该是一个很不错的选择，既可以做移动应用，以后慢慢也可以转前端，希望这条路没有走错！

## About Me：
[个人主页](https://xinhehappy.github.io/)
[GitHub](https://github.com/xinhehappy)
