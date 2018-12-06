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
<br><br>


# Movie => [实现电影APP应用](https://github.com/CruxF/WXsmallProgram/tree/master/Movie?1544067088144)
项目的开始，是实现全局的tabBar导航，其中涉及到的新知识是小程序框架之[全局配置](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE)，下面请看相关代码
```js
{
  "pages": [
    "pages/movie/movie",
    "pages/index/index"
  ],
  "tabBar": {
    "color": "#ddd",
    "selectedColor": "#3cc51f",
    "borderStyle": "black",
    "backgroundColor": "#2B2B2B",
    "list": [
      {
        "pagePath": "pages/index/index",
        "iconPath": "pages/assets/img/dy-1.png",
        "selectedIconPath": "pages/assets/img/dy.png",
        "text": "影院热映"
      },
      {
        "pagePath": "pages/movie/movie",
        "iconPath": "pages/assets/img/tj-1.png",
        "selectedIconPath": "pages/assets/img/tj.png",
        "text": "电影推荐"
      },
      {
        "pagePath": "pages/movie/movie",
        "iconPath": "pages/assets/img/search-1.png",
        "selectedIconPath": "pages/assets/img/search.png",
        "text": "查询电影"
      }
    ]
  },
  "window": {
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#2B2B2B",
    "navigationBarTitleText": "小木学堂电影APP",
    "navigationBarTextStyle": "white"
  }
}
```

接着涉及的新知识点不过是小程序里面的基础内容，比如
- 组件之[swiper](https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html)，也就是轮播图组件
- API之[wx.showLoading](https://developers.weixin.qq.com/miniprogram/dev/api/wx.showLoading.html?search-key=loading)，也就是页面数据加载时出现的动画

有两点比较重要的是模板的使用，之前有说到；另外一点就是wxss样式代码相互之间能够引用，这就能够让我少写十分多重复的代码，以及我们能够封装一个公共的方法，然后在多个地方进行调用，这些都能够在源码中看到，具体点的就不提了。由于豆瓣接口现在已经无法调用了，因此电影详情以及电影搜索这两个功能无法实现，豆瓣客服如下说：
```text
你好， 

豆瓣Api持续维护中，Apikey暂不对个人开放申请。电影/图书资料信息等基础数据信息，暂不做任何形式的Api输出合作。 

感谢对豆瓣的关注。
```

更多小程序使用技巧请看下篇分享<br><br>


# FirstWxPro => [入门微信小程序](https://www.imooc.com/learn/974)

这是一门十分简单的小程序入门指南视频（应该说小程序本来就很简单），根据老师的提供的源码和素材，将其做了部分的结构和样式更改，有兴趣的可以下载该项目，运行查看结果。<br>

起步过于简单，在这不说明，请直接到[官方网站](https://developers.weixin.qq.com/miniprogram/dev/)开启自己的小程序之旅，下面整理一些自己认为重要的知识点。<br>

### 全局配置文件
app.json文件用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等，具体的请[点击这里](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)<br>

### swiper组件的使用
[这个组件](https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html?search-key=swiper)是比较常用的，说它比较重要，不仅仅是因为它的常用性，也因为它的栗子有十分好的借鉴性。<br>

结构代码<br>
```html
<swiper indicator-dots="{{indicatorDots}}" autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}" indicator-dots="true">
  <block wx:for="{{imgUrls}}" wx:key="">
    <swiper-item>
      <image src="{{item}}" class="slide-image" width="355" height="150"/>
    </swiper-item>
  </block>
</swiper>
```

脚本代码<br>
```js
Page({
  data: {
    imgUrls: [
      '/images/home1.jpg',
      '/images/home2.jpg',
      '/images/home3.jpg',
    ],
    indicatorDots: false,
    autoplay: false,
    interval: 5000,
    duration: 1000,
    proList: null,
  }
})
```
说这个组件的使用具有借鉴性是因为将属性和属性值完全分离了来进行管理，在一定程度上来说，这是十分好的一种方式，结构代码和脚本代码分离便于管理和维护。<br>

### 调用数据的三种方式
在小程序中，一共有三种调用数据的方式，其中一种是调用后台数据，另外两种是调用本地数据，现在我们先来看一看最简单的本地数据调用方式：
```js
// 定义本地脚本数据
Page({
  data: {
    imgUrls: [
      '/images/home1.jpg',
      '/images/home2.jpg',
      '/images/home3.jpg',
    ]
  }
})

//调用本地数据
<block wx:for="{{imgUrls}}" wx:key="">
  <swiper-item>
    <image src="{{item}}" class="slide-image" width="355" height="150"/>
  </swiper-item>
</block>
```
第二种调用本地数据稍微复杂一些，它和Vue程序中调用json数据的方式完全不同，在小程序不能直接调用json文件中的数据，只能将数据保存在一个脚本文件中，然后将其exports出来，最后在需要调用数据的文件中导入，具体请看以下代码：
```js
// 步骤一：分离数据，将数据定义在顶层的data目录下
var joinList_data = [
  {
    "proName": "关于NBA",
    "proDesc": "NBA（National Basketball Association）是美国职业篮球联赛的简称",
    "id": "001"
  }
]
module.exports = {
  joinList: joinList_data
}

// 步骤二：在需要调用数据的脚本文件中引入
var joinListData = require("../../data/joinList.js");

// 步骤三：在周期函数中赋值
Page({
  data: {
    joinList: null
  },
  // 生命周期函数--监听页面加载
  onLoad: function () {
    this.setData({
      joinList: joinListData.joinList
    })
  }
})

// 步骤四：在结构代码中遍历数据
<view class='block' wx:for="{{joinList}}" wx:key="">
  <view class='block-title'>{{item.proName}}</view>
  <text class='block-desc'>{{item.proDesc}}</text>
</view>
```
第三种数据调用方式最重要，因为那是必须会的，这种方式是从后台请求回来数据进行使用，具体方式请看以下代码：
```js
Page({
  data: {
    proList: null,
  },
  onLoad: function () {
    this.getProList();
  }
  // API请求方法
  getProList: function(){
    var self = this;
    wx.request({
      url: 'http://guozhaoxi.top/wx/data.json',
      method: 'GET',
      success: function(res){
        self.setData({
          proList: res.data,
        })
      },
      fail: function(){
        console.log('调用数据失败');
      }
    })
  }
})

// 使用请求回来的数据
<view class='pro-item' wx:for="{{proList}}" wx:key=""></view>
```

### 组件之间的三种传值方式
一说到组件传值，相信有经验的开发人员都知道它的重要性，下面简单的来看看三种传值方式的代码。<br>

第一种：全局传值<br>
```js
// 步骤一：在全局脚本文件中定义数据
App({
  globalData: {
    userInfo: null,
    userName: '全局变量传值',
  }
})

// 步骤二：获取应用实例，不然无法调用全局变量
const app = getApp()

// 步骤三：调用全局变量
Page({
  data: {
  
  },
  onLoad: function (options) {
    console.log(app.globalData.userName);
  },
})
```

第二种：url传值
```js
// 步骤一：使用关键字bindtap绑定一个点击事件方法，data-index是传入一个值
<image class="btn-detail" src='/images/btn_detail.png' bindtap='toDetail' data-index='{{index}}'></image>

// 步骤二：在脚本文件中定义这个方法（方法并不是定义在一个methods集合中的）
Page({
  data: {},
  onLoad: function () {},
  toDetail: function(e){
    // index代表的遍历对象的下标
    var index = e.currentTarget.dataset.index;
    var proList = this.data.proList;
    var title = proList[index].proName;
    wx.navigateTo({
      url: '/pages/detail/detail?title='+title,
    })
  }
})

// 步骤三：在detail组件的脚本文件中接收title这个传入过来的值
Page({
  data: {},
  onLoad: function (options) {
    console.log(options.title);
  },
})
```

第三种：setStorageSync传值
```js
// 步骤一：使用关键字bindtap绑定一个点击事件方法，data-index是传入一个值
<image class="btn-detail" src='/images/btn_detail.png' bindtap='toDetail' data-index='{{index}}'></image>

// 步骤二：在脚本文件中定义这个方法（方法并不是定义在一个methods集合中的）
Page({
  data: {},
  onLoad: function () {},
  toDetail: function(e){
    var index = e.currentTarget.dataset.index;
    var proList = this.data.proList;
    var title = proList[index].proName;
    wx.setStorageSync('titleName', title);
    wx.navigateTo({
      url: '/pages/detail/detail',
    })
  }
})

// 步骤三：在detail组件的脚本文件中使用wx.getStorageSync接收titleName这个传入过来的值
Page({
  data: {},
  onLoad: function (options) {
    var titlen = wx.getStorageSync('titleName');
    console.log(titlen);
  },
})
```

#### 组件传值的应用场景
关于组件传值的应用，老师在视频中并没有给出，自己瞎琢磨出一个栗子，记住：**在真正开发中，千万别这么用。** 这个栗子的作用是让我们对组件的传值有一个大概的应用场景，下面请看代码实现：
```js
// 数据接收方
Page({
  data: {
    iskebi: false,
    iszhan: false,
    isqiao: false,
    isgeli: false
  },
  onLoad: function (options) {
    
  },
  onReady: function () {
    var titlee = wx.getStorageSync('titleName');
    console.log(titlee);
    if (titlee == '科比·布莱恩特') {
      this.setData({iskebi: true});
    } else if (titlee == '勒布朗·詹姆斯'){
      this.setData({ iszhan: true });
    } else if (titlee == '迈克尔·乔丹') {
      this.setData({ isqiao: true });
    } else{
      this.setData({ isgeli: true });
    }
  }
})

// 数据显示层
<view wx:if="{{iskebi}}">
  我是科比的球迷
</view>
<view wx:if="{{iszhan}}">
  我是詹姆斯的球迷
</view>
<view wx:if="{{isqiao}}">
  我是乔丹的球迷
</view>
<view wx:if="{{isgeli}}">
  我是格里芬的球迷
</view>
```


### 基础库兼容
这个东西其实也不是太重要，知道有个玩意，以及如何去判断和解决就行，下面看代码：
```js
Page({
  data: {},
  onLoad: function () {},
  // copy事件
  copy: function(){
    // 检测版本是否具备wx.setClipboardData这个API
    if (wx.setClipboardData){
      wx.setClipboardData({
        // 复制的内容，可以设置为动态的数据
        data: '232323232',
        success: function (res) {
          // 模态框
          wx.showModal({
            title: '复制成功',
            content: '内容已经复制成功！',
          })
        }
      })
    }
    else{
      wx.showModal({
        title: '提示',
        content: '您的微信版本太低，请升级',
      })
    }
  }
})
```

### 尾声
以上就是我所做的一些总结，源码都在[这里](https://github.com/CruxF/WXsmallProgram/tree/master/FirstWxPro?1544067560085)，有疑问的可以加我慕课账号（Zz皓）私信聊。<br><br>






