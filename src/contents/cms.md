# 小程序数据管理
涉及到 UGC 用户创作时，或有数据产生时，难免会需要建设一个管理控制台。但管理控制台的建设，是一个繁琐又枯燥的工作。

不过，如果你使用了云开发，就可以借助云开发提供的[内容管理能力](https://console.cloud.tencent.com/tcb/cms)，几分钟建立起一个功能强大的后台。

![](https://postimg.aliavv.com/mbp/vs3kd.png)


## 操作步骤

想要开通云开发提供的内容管理能力，首先，你需要使用小程序账号登录**腾讯云控制台**。在云开发控制台中找到「**内容管理**」，点击「**安装拓展**」，填写相关信息，并点击下一步，几分钟就可以创建出一个全功能的管理后台

![](https://postimg.aliavv.com/mbp/prkct.png)


管理后台创建完成后，可以使用你刚刚填写的管理员信息，登录 CMS

![](https://postimg.aliavv.com/mbp/uoa00.png)

CMS 的地址可以点击左侧的静态网站托管中找到域名，访问域名 + 你刚刚自己设定的路径，比如我这里是 editor.91qcloud.com/tcb-cms/

![](https://postimg.aliavv.com/mbp/anxju.png)

登录后，点击左侧的**内容管理**，可以找到具体的内容配置，根据你的需要配置相应的字段就可以查看数据库相关的信息了。

![](https://postimg.aliavv.com/mbp/sjhns.png)

关于 CMS 的具体配置，这里不再一一赘述，你可以参考官方提供的教程来查看

![](https://postimg.aliavv.com/mbp/g8l9u.png)

## 总结

管理后台的建设是一个脏活累活，对于我们的活动场景，借助云开发提供的内容管理能力，快速的创建出一个内容管理系统，可以让我们将更多的精力投放在用户可见的小程序侧，而在管理端，则使用云开发提供的标准的控制台来完成。

## Reference

- https://github.com/TencentCloudBase/cloudbase-extension-cms