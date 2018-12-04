# Calculator => 实现一个简易计算器
这是一个使用小程序实现的简单计算器，来源于小木学堂。开发过程很简单，主要就是根据官网配置好开发环境，接着创建一个项目，将这里的[源码](https://github.com/CruxF/WXsmallProgram/tree/master/Calculator?1543915837338)copy进去即可检验效果，在该项目中涉及到了如下基础知识

- 基础组件之[视图容器](https://developers.weixin.qq.com/miniprogram/dev/component/)
- 框架之[响应的数据绑定](https://developers.weixin.qq.com/miniprogram/dev/framework/MINA.html)
- 框架之[路由](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html)
- 框架之[列表渲染](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/list.html)
- 框架之[触发事件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/events.html)
- API之[数据缓存](https://developers.weixin.qq.com/miniprogram/dev/api/wx.setStorageSync.html)

在开发的过程中遇到了以下两个问题
- 1、如何在wxss文件中调用本地图片资源？[答案](https://github.com/CruxF/WXsmallProgram/issues/1)
- 2、wx.setstoragesync和wx.setstorage的区别是什么？[答案](https://github.com/CruxF/WXsmallProgram/issues/2)
