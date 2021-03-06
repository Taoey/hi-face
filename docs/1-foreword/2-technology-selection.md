---
title: 1.2 技术选型的依据
---

技术选型的参考依据：

* 核心特色功能：腾讯云的人脸五官分析
* 服务侧：云开发支持多端调用
* UI侧：Taro支持多端运行，遵从 React 语法规范

## 特色功能 - 人脸五官分析

[腾讯云 人脸识别](https://cloud.tencent.com/product/facerecognition)是基于腾讯优图强大的面部分析技术，提供包括人脸检测与分析、五官定位、人脸搜索、人脸比对、人脸验证、人员查重、活体检测等多种功能，为开发者和企业提供高性能高可用的人脸识别服务。 可应用于智慧零售、智慧社区、在线娱乐、智慧楼宇、在线身份认证等多种应用场景，充分满足各行业客户的人脸属性识别及用户身份确认等需求。 

![](https://n1image.hjfile.cn/res7/2020/03/30/31bfe7a9d5019902cf28ae98bea5085c.png)

具体配置可以参考《[人脸五官分析的环境配置](2-project-config/2-tencent-cloud-ai-face.md)》

## 服务侧

### 云开发概念解析

[云开发](https://docs.cloudbase.net/)，含小程序云开发、Web 云开发。开发者可以使用云开发开发微信小程序、小游戏，无需搭建服务器，即可使用云端能力。

云开发为开发者提供完整的原生云端支持和微信服务支持，弱化后端和运维概念，无需搭建服务器，使用平台提供的 API 进行核心业务开发，即可实现快速上线和迭代，同时这一能力，同开发者已经使用的云服务相互兼容，并不互斥。 

<!-- TODO 配图 -->

### 我的选择

**高效安全，Serverless 前后端一体化开发模式、免运维；通过安全认证，稳定可靠有保障。**

实际改变1：Hi头像的人脸识别功能本来是部署在我自己搭建的云服务器上的，需要配置 nginx 反向代理、nodejs服务以及 face-api 的node版本，但之后是借助于云开发的云函数以及腾讯云的五官分析服务快速搞定了。

实际改变2：用户保存头像时，不仅要将图片存储到腾讯云的对象存储中，还要将这条记录到云服务器上nodejs服务调用的MongoDB数据库内，而借助云开发，可以直接将图片存储到云存储上，并且该条记录可以存到云数据库上。

通过上面两条的实际改变，我可以无需像配置 `nginx` 反向代理、`Nodejs` 服务、`MongoDB` 数据库等运维人员才能稳妥完成的内容，而只需要创建云开发的云环境即可。
并且，在对象存储及数据库都是需要设置权限的，但云开发将这些权限都尽量简化或者操作非常简单方便。

**多端支持，不仅支持小程序，也提供了丰富的 SDK，包括小程序、Web、移动端支持；各类应用可以快速接入。**

微信小程序最先支持云开发，在之后呢，web端可以通过 `tcb-js-sdk` 来调用云开发的功能。

而在微信开发者工具中提供了新的扩展能力——[静态网站托管](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/staticstorage/introduction.html)，这就逐渐说明微信开发者会将腾讯云的web云开发控制台里面的功能迁移过来，也会将 Web 云开发的功能做得更加明朗化。


**云调用，鉴权方便，快速获取核心账户信息**

如果是普通小程序想获取openId及unionId的话，还需要在自己的服务器上做一次解密的操作，但使用小程序云开发，就可以直接通过 `wx-server-sdk` 来获取这些信息了。
具体可以参考 [云调用](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/openapi/openapi.html)。

## UI侧

### Taro 概念解析

[Taro 多端统一开发解决方案](https://taro-docs.jd.com/taro/docs/README.html) 是一套遵循 `React` 语法规范的 **多端开发** 解决方案。使用 Taro，我们可以只书写一套代码，再通过 Taro 的编译工具，将源代码分别编译出可以在不同端（微信/百度/支付宝/字节跳动/QQ/京东小程序、快应用、H5、React-Native 等）运行的代码。

<!-- TODO 配图 -->

### 我的选择

**React技术栈**

我近几年主要是以 `React` 作为 Web 开发的技术栈，而 `Taro` 恰好是遵循 `React` 语法规范的，这减少我的学习成本，并且在公司里面的团队协作上省下不少力气。

`Taro` 的 jsx 语法和生命周期都不仅对标了 React 的规范，并且还包含了小程序里面的生命周期，是非常齐全的。

`Taro` 是以微信小程序的组件元素为蓝本，扩展到Web端的DOM元素上，这符合我先完成微信小程序开发，再适配到 `Web` 端的思路。

**前端工程化及静态站点托管。**

前端工程化的一个典型用途就是持续集成，而云开发不仅提供了小程序端，还提供了Web端的静态站点托管功能。借助Taro的工程化编译、TencentCloudbase Action将静态站台托管到云开发上，这是持续集成的实际用法。

完整的文章请参考《[小程序云开发环境部署静态网站托管的实战](tencent-cloud/web-devops-github-action.md)》


**为初学者提供原生小程序的基础功能**

即使我用 Taro 完成了Hi头像的大部分功能，但本着对初学者友好的态度，我也简单写了Hi头像的原生小程序版本。

* [hi-face](https://github.com/hi-our/hi-face) 项目是基于 `Taro 2.x` 版本上进行研发的，后续也会逐步升级到 Taro 3.0版本上。功能包含了头像戴口罩、节日主题、个人中心、海报分享等功能。
* [hi-face-wx](https://github.com/hi-our/hi-face-wx) 是原生小程序版本的Hi头像戴口罩功能小程序，上面有人脸魅力识别、戴口罩等基础功能。

> PS：`hi-face-wx` 中人脸魅力识别、图像标签等功能由李欢来帮助编写。



