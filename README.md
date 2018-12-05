# Calculator => [实现一个简易计算器](https://github.com/CruxF/WXsmallProgram/tree/master/Calculator?1543978668813)
这是一个使用小程序实现的简单计算器，来源于小木学堂。开发过程很简单，主要就是根据官网配置好开发环境，接着创建一个项目，将这里的[源码](https://github.com/CruxF/WXsmallProgram/tree/master/Calculator?1543915837338)copy进去即可检验效果，在该项目中涉及到了如下基础知识

- 基础组件之[视图容器](https://developers.weixin.qq.com/miniprogram/dev/component/)
- 框架之[响应的数据绑定](https://developers.weixin.qq.com/miniprogram/dev/framework/MINA.html)
- 框架之[路由](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html)
- 框架之[列表渲染](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/list.html)
- 框架之[触发事件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/events.html)
- API之[数据缓存](https://developers.weixin.qq.com/miniprogram/dev/api/wx.setStorageSync.html)

在开发的过程中遇到了以下两个问题
- 1、如何在wxss文件中调用本地图片资源？[答案](https://github.com/CruxF/WXsmallProgram/issues/1)
- 2、wx.setstoragesync和wx.setstorage的区别是什么？[答案](https://github.com/CruxF/WXsmallProgram/issues/2)<br><br>


# Weather => [实现天气预报应用](https://github.com/CruxF/WXsmallProgram/tree/master/Weather?1543978644626)
挺容易的一个项目，主要是调用第三方API，也就是结合百度地图，对此陌生的童鞋可以[先来这里](http://lbsyun.baidu.com/index.php?title=wxjsapi/guide/key)补充下知识，相信将开发指南好好动手敲一遍的童鞋都能看懂以下代码
```js
//引入 bmap-wx.js
var bmap = require('../../libs/bmap-wx.js');
var wxMarkerData = [];

Page({
  data: {
    city: "",
    today: {},
    future: {}
  },
  // 页面初始化 options为页面跳转所带来的参数
  onLoad: function (options) {  
    this.loadCity();
  },
  loadCity: function (latitude, longitude) {
    var page = this;
    // 新建百度地图对象 
    var BMap = new bmap.BMapWX({
      ak: '你的ak码'
    });
    var fail = function (data) {
      console.log(data)
    };
    var success = function (data) {
      // 城市
      wxMarkerData = data.wxMarkerData;      
      var index = wxMarkerData[0].desc.indexOf('市')
      var city = wxMarkerData[0].desc.slice(0, index+1)
      page.setData({ city: city });
      page.loadWeather()
    }
    // 发起regeocoding检索请求 
    BMap.regeocoding({
      fail: fail,
      success: success
    });
  },
  loadWeather: function () {
    var page = this;
    // 新建百度地图对象 
    var BMap = new bmap.BMapWX({
      ak: '你的ak码'
    });
    var fail = function (data) {
      console.log(data)
    };
    var success = function (data) {
      // 当天
      var weatherData = data.currentWeather[0];
      // 当天-未来三天
      var futureWeather = data.originalData.results[0].weather_data;
      var future = []
      for (var i = 0; i < futureWeather.length;i++){
        future.push({
          date: futureWeather[i].date.slice(0, 2),
          temperature: futureWeather[i].temperature.slice(0, 2) +'℃',
          weather: futureWeather[i].weather,
          wind: futureWeather[i].wind
        })
      }
      page.setData({ today: weatherData, future: future })
    }
    // 发起weather请求 
    BMap.weather({
      fail: fail,
      success: success
    }); 
  }
})
```

接下来比较重要的一个点就是微信小程序[框架之模板](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/template.html)，可能看文档会觉得模模糊糊，那么看下面的代码相信你能清晰很多
```html
// 调用模板文件
<import src="../template/itemtpl" />
<view class="future">
  <block wx:for="{{future}}" wx:key="index">
    <template is="future-item" data="{{item}}" />
  </block>
</view>

// 模板文件
<template name="future-item">
  <view class="future-item">
    <view>{{item.date}} </view>
    <view> {{item.weather}} </view>
    <view>{{item.wind}}</view>
    <view> {{item.temperature}} </view>
  </view>
</template>
```
