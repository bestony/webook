# 小程序对接内容安全识别
当你负责的小程序涉及到了用户创造内容（UGC），比如用户上传图片、用户上传文字的时候，就应当接入图片/文本安全识别，对于用户上传的内容进行一定的安全校验，确保不会有一些违反国内法律法规的内容上传到小程序当中。

在这个过程中，我推荐你使用微信官方提供的内容安全校验接口，最为稳妥。如果你已经在使用云开发，有打算接入内容安全校验，那么你将会体验到云开发的一项极为强大的能力 —— 云调用，**你可以用一行代码，完成内容安全校验接口的调用**。



## 调用时序对比
### 传统方式
![](https://postimg.aliavv.com/mpb/asgpp.png)
### 云调用方式
![](https://postimg.aliavv.com/mpb/yj993.png)
### 结论

通过对比时序图，你可以看到，相比于传统方式，云调用方式中间环节少，逻辑简单，可以更快速的完成业务的开发。

具体的逻辑，你可以参考下方代码。

## 云调用方式调用示例代码

首先，你需要创建一个云函数，比如名为`msgCheck`，随后，在该函数的 config.json 文件中，添加内容安全接口的云调用声明。

```json
{
  "permissions": {
    "openapi": ["security.msgSecCheck"]
  }
}
```

并编写如下代码

```javascript
// 云函数入口文件
const cloud = require('wx-server-sdk')

cloud.init()

// 云函数入口函数
exports.main = async (event, context) => {
    const { text } = event;
    try {
      // 通过云调用进行内容安全校验
      await cloud.openapi.msgSecCheck({
        content: text,
      });
    } catch (error) {
      return {
        code: 87014,
        msg: "数据存在安全风险",
      };
    }
    // 新增数据逻辑
}
```

编写完成后，上传并部署云函数，就可以进行具体的测试了，测试时，只需要在小程序侧调用函数即可查看效果。

```js
wx.cloud.callFunction({
    name:"msgCheck",
    data:{
        text:"特3456书yuuo莞6543李zxcz蒜7782法fgnv级"
    },
    success: res => {
        console.log(res)
    }
})
```

当你在 success 的回调中看到 `{ code:87014, msg:"数据存在安全风险" }`，就说明你的安全校验已经正常恢复工作了。
## 注意

这里有一个注意的点是，云函数返回值必须是 87014 ，这是因为微信侧的审核同学是根据这个返回值来判断是否接入了安全校验接口的。因此，大家要注意，函数的返回值 Code 必须是 87014.

## 总结

如果你做了一个 UGC 的小程序，那么内容安全识别是必然要上的。既然要上，为什么不试一试用一行代码来完成相关的能力接入呢？简单明了就可以完成一个需求，Why Not？

## Reference 

- https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/sec-check/security.msgSecCheck.html
- https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/sec-check/security.imgSecCheck.html
- https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/sec-check/security.mediaCheckAsync.html