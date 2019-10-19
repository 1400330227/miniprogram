微信小程序

# 起步

微信小程序项目里面包含了四个文件：

- `.json` 后缀的 `JSON` 配置文件
- `.wxml` 后缀的 `WXML` 模板文件
- `.wxss` 后缀的 `WXSS` 样式文件
- `.js` 后缀的 `JS` 脚本逻辑文件

`.json` 文件是数据格式，扮演静态配置的角色

## 小程序配置 `app.json` 

`app.json` 是当前小程序的全局配置，包括了所有页面的路径、界面表现、网络超时时间、底部 `tab`

```json
{
  "pages": [
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window": {
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "WeChat",
    "navigationBarTextStyle": "black"
  },
  "sitemapLocation": "sitemap.json"
}
```

## 页面配置

每个页面都有一个 `.json` 文件对本页面进行静态配置。页面配置项会覆盖 `app.json` 的 `window` 中相同的配置

```json
{
  "navigationBarBackgroundColor": "#ffffff",
  "navigationBarTextStyle": "black",
  "navigationBarTitleText": "微信接口功能演示",
  "backgroundColor": "#eeeeee",
  "backgroundTextStyle": "light"
}
```

# 注册小程序 `App(Object object)`

## 注册小程序

每个小程序都需要在 `app.js` 中调用 `App({})` 方法注册小程序，绑定生明周期回调函数、错误监听和页面不存在监听函数等。

```JavaScript
App({
    // 监听小程序初始化。
    onLaunch(options) {},
    // 监听小程序启动或切前台。
    onShow() {},
    // 监听小程序切后台。
    onHide() {},
    // 错误监听函数。
    onError() {},
     //页面不存在监听函数。
    onPageNotFound () {}
})
```



## 生明周期函数

`onLaunch`  生命周期回调——监听小程序初始化。  小程序初始化完成时触发，全局只触发一次。 

`onShow`  生命周期回调——监听小程序启动或切前台。  小程序启动，或从后台进入前台显示时触发。 

`onHide`  命周期回调——监听小程序切后台。  小程序从前台进入后台时触发。 

# 注册页面 `Page(Object object)`

## 注册页面

对于小程序中的每个页面，都需要在页面对应的 `.js` 文件中注册，指定页面的初始数据、生命周期回调、事件处理函数等。

```javascript
Page({
    // 初始化数据
    data: {},
    // 页面创建时执行
    onLoad(options) {},
    // 监听页面显示
    onShow() {},
    // 监听页面初次渲染完成
    onReady() {},
    // 听页面隐藏
    onHide() {},
    // 监听页面卸载
    onUnload() {}
})
```

简单页面适合使用 `Page({})` 构造器注册页面，复杂页面适合使用 `Component({})` 注册页面， `Component` 构造器的主要区别是：方法需要放在 `methods: { }` 里面。 

```javascript
Component({
    // 初始化数据
    data: {},
    method: {
        // 页面创建时执行
        onLoad(options) {},
        // 监听页面显示
        onShow() {},
        // 监听页面初次渲染完成
        onReady() {},
        // 听页面隐藏
        onHide() {},
        // 监听页面卸载
        onUnload() {}
    }
})
```

## 页面生命周期

生明周期回调函数

```JavaScript
// 页面创建时执行。页面加载时触发。一个页面只会调用一次
onLoad(options) {},
// 监听页面显示。页面显示/切入前台时触发
onShow() {},
// 监听页面初次渲染完成。一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。
onReady() {},
// 听页面隐藏.页面隐藏/切入后台时触发。 
onHide() {},
// 监听页面卸载.页面卸载时触发。
onUnload() {}
```

## 页面路由

小程序的路由全部由框架进行处理，框架以栈的方式维护当前的所有页面。当路由发生切换的时候发生如下变化：

- 初始化。新页面入栈
- 打开新页面。新页面入栈
- 页面重定向。当前页面出栈，新页面入栈
- 页面返回。页面不断出栈，直到目标返回页
- Tab 切换。页面全部出栈，只留下 Tab 页面
- 重加载。页面全部出栈，只留下新页面

```html
<!--打开新页面-->
<navigator open-type="navigate" url=""></navigator>

<!--页面重定向-->
<navigator open-type="redirect" url=""></navigator>

<!--页面返回-->
<navigator open-type="navigatorBack" url=""></navigator>

<!--Tab 切换-->
<navigator open-type="switchTab" url=""></navigator>

<!--重加载-->
<navigator open-type="exit" url=""></navigator>
```

我们可以使用 `getCurrentPages()` 函数获取当前页面栈。 

# 视图层

## `wxml` 语法

### 数据绑定

```html
<!--index.wxml-->
<!--内容渲染-->
<view>{{name}}</view>
<!--属性渲染-->
<view class="kind-list-item-hd {{item.isOpen ? 'kind-list-item-hd-show' : ''}}"></view>

<!--index.js-->
Page({
	data: {
		name: 'hello, world'
	}
})
```

### 列表渲染

```html
<!--index.wxml-->
<block wx:for-items="{{pages}}" wx:for-item="page" wx:for-index="idx" wx:key="*this"></block>

<!--index.js-->
Page({
	data: {
		pages: ['view', 'scroll-view', 'swiper', 'movable-view', 'cover-view']
	}
})
```

### 条件渲染

```html
<view wx:if="{{isOpen}}">true</view>
<view wx:elif="{{length > 2}}">true</view>
<view wx:else>true</view>
```

### 模板

```html
<template name="head">
    <view>
        <text>{{name}}</text>
        <image src="{{path}}"></image>
    </view>
</template>

<template is="head" data="{{name: 'tang', path: 'imagePath'}}"></template>
```

### 应用

  `item.wxml` 中定义了一个叫`item`的`template`： 

```html
<!-- item.wxml -->
<template name="item">
  <text>{{text}}</text>
</template>
```

 在 `index.wxml` 中引用了 `item.wxml`，就可以使用`item`模板： 

```html
<import src="item.wxml"/>
<template is="item" data="{{text: 'forbar'}}"/>
```

## 基础组件

组件是视图层的基本组成单元。组件自带一些功能与微信风格一致的样式。组件包含开始标签、结束标签、属性和内容四大板块。

```html
<tagname property="value">content</tagname>
```

公共属性是所有组件都用的属性

- id。string 组件的唯一标识符
- class。string 组件的样式类
- style。string 组件的内联样式
- hidden。 true|false 组件是否显示
- data-*。自定义属性，在组件触发时，提供给事件对象
- bind* / catch*。组件的事件

## 事件系统

- 事件是视图层到逻辑层的通讯方式。
- 事件可以将用户的行为反馈到逻辑层进行处理。
- 事件可以绑定在组件上，当达到触发事件，就会执行逻辑层中对应的事件处理函数。
- 事件对象可以携带额外信息，如 id, dataset, touches。

```html
<!--在组件中绑定一个事件处理函数-->
<view id="tapId" bindtap="tapName">click me!</view>

Page({
	tapName:function(event) {
 		console.log(event)
	}
})
```

打印信息如下：

```JavaScript
{
  "type":"tap",
  "timeStamp":895,
  "target": {
    "id": "tapId",
    "dataset":  {
      "hi":"WeChat"
    }
  },
  "currentTarget":  {
    "id": "tapId",
    "dataset": {
      "hi":"WeChat"
    }
  },
  "detail": {
    "x":53,
    "y":14
  },
  "touches":[{
    "identifier":0,
    "pageX":53,
    "pageY":14,
    "clientX":53,
    "clientY":14
  }],
  "changedTouches":[{
    "identifier":0,
    "pageX":53,
    "pageY":14,
    "clientX":53,
    "clientY":14
  }]
}
```

