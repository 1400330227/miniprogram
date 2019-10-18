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

