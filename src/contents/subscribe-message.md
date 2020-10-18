# 小程序订阅消息
和公众号不同，小程序主要承载的是业务功能。在开发小程序的过程中，如果我们想要触达用户，基本上只有两种办法：

1. 借助小程序自带的订阅消息来触达用户
2. 借助小程序关联的服务号，再通过服务号的模板消息触达用户

很显然，当我们的小程序仅仅是一个内部活动的小程序的时候，为其开发关联的服务号成本就太高了，在这种情况下，借助小程序自带的订阅消息来触达用户是一个不错的选项。

**在使用云开发的情况下，可以直接使用云开发提供的云调用能力，免 access token 调用相关的接口。**

## 订阅消息

消息能力是小程序能力中的重要组成，微信为开发者提供了订阅消息能力，以便实现服务的闭环和更优的体验。

- 订阅消息推送位置：服务通知
- 订阅消息下发条件：用户自主订阅
- 订阅消息卡片跳转能力：点击查看详情可跳转至该小程序的页面

![](https://postimg.aliavv.com/mbp/x12b2.jpg)

## 订阅消息的使用流程

1. 在小程序后台获取模板 ID：登录 mp.weixin.qq.com  可以获取到模板消息的相关信息。
2. 在小程序侧调用 `wx.requestSubscribeMessage` ，获取下发权限
3. 在云函数侧使用云调用触发消息下发接口，完成消息触达

## 订阅消息发送的示例代码

**小程序侧**

```js
wx.requestSubscribeMessage({
  tmplIds: [''],
  success (res) { }
})
```

**云函数侧**
config.json
```json
{
  "permissions": {
    "openapi": ["subscribeMessage.send"]
  }
}
```
index.js
```js
const cloud = require('wx-server-sdk')
cloud.init()
exports.main = async (event, context) => {
  try {
    const result = await cloud.openapi.subscribeMessage.send({
        touser: 'OPENID',
        page: 'index',
        lang: 'zh_CN',
        data: {
          number01: {
            value: '339208499'
          },
          date01: {
            value: '2015年01月05日'
          },
          site01: {
            value: 'TIT创意园'
          },
          site02: {
            value: '广州市新港中路397号'
          }
        },
        templateId: 'TEMPLATE_ID',
        miniprogramState: 'developer'
      })
    return result
  } catch (err) {
    return err
  }
}
```