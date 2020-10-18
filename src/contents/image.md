# 小程序图片/视频处理
图片/视频处理部分将会介绍两个常用的操作

- 上传视频/图片时生成缩略图
- 为上传图片新增水印

## 上传视频时生成缩略图


在进行小程序开发的时候，我们经常会有需求，希望用户上传一个视频或者图片。这个时候，我们的第一反应会是使用 `wx.chooseImage` 来上传图片，使用 `wx.chooseVideo` 来上传视频。

上传图片的时候，没有问题，我们选择了图片，并获取到图片的 tempPath 时，就可以借助云开发的接口进行上传。

```javascript
wx.chooseImage({    
    success: async(data) => {
        console.log(data)
        wx.cloud.uploadFile({
             filePath: data.tempFilePaths[0],
             cloudPath: "image/user-upload.jpg"
        })
    }
})
```

上传视频的时候，我们也可以照猫画虎，实现类似的功能。

```javascript
wx.chooseVideo({
    success: async(data) => {
        console.log(data)
        wx.cloud.uploadFile({
             filePath: data.tempFilePath,
             cloudPath: "video/user-upload.mp4"
        })
    }
})
```

**但是，这个接口有一个缺陷，我们上传的视频并没有缩略图，这导致我们在后续开发视频/照片的列表页面的时候，没有缩略图可用**

你可能下意识想到可以借助一些服务来生成缩略图，但毕竟存在费用问题，因此，并不推荐你使用。

其实，在小程序开发中，你还有另外一种方式，来实现这样的一个需求 —— 那就是使用 `wx.chooseMedia` 接口。

`wx.chooseMedia` 接口在选取图片时，返回结果和 `wx.chooseImage` 相同。而它在选择视频的时候，表现和 `wx.chooseVideo` 就不同了。


![](https://postimg.aliavv.com/mbp/w1mj3.png)

你可以在接口中的 `thumbTempFilePath` 中，获取到图片的缩略图的文件路径，你只需要将这个地址上传到服务端存储，就免于在服务端使用 FFmpeg 对视频进行处理啦。即节约成本，又能获得不错的体验。

## 为上传图片新增水印

?> _TODO_ 插图

有些时候，我们希望用户上传的图片能够加入水印，这样在用户传播图片的时候，就可以自然的为小程序带来流量。

一提到图片处理，估计你又想到了 ImageMagick 或者 images 之类的库来完成。不过，除了自己费心费力去调整这些接口以外，你还可以考虑使用[云开发提供给你的拓展能力](https://cloud.tencent.com/document/product/876/42103)来快速完成。

使用你的小程序账号登录腾讯云，找到你的云开发环境，选择其中的「云调用」，安装「图像处理」，就可以快速的使用图像处理调用能力，来完成图片加水印这个需求。

不仅如此，还可以对图片进行多种操作。



|类型|介绍|
|--| -- | 
|缩放|等比缩放、设定目标宽高缩放等多种方式|
|裁剪|普通裁剪、缩放裁剪、内切圆、人脸智能裁剪|
|旋转|普通旋转、自适应旋转|
|格式转换|jpg、bmp、gif、png、webp、yjpeg格式转换，gif 格式优化，渐进显示功能|
|质量变换|针对 JPG 和 WEBP 图片进行质量变换|
|高斯模糊|对图片进行模糊处理|
|锐化|对图片进行锐化处理|
|图片水印|提供图片水印处理功能|
|文字水印|提供实时文字水印处理功能|
|获取图片基本信息|查询图片基本信息，包括格式、长、宽等|
|获取图片 EXIF|查询图片 EXIF 信息，如照片的拍摄参数、缩略图等|
|获取图片主色调|获取图片主色调信息|
|去除元信息|去除图片元信息，减小图像体积|
|快速缩略模板|快速实现图片格式转换、缩略、剪裁等功能，生成缩略图|
|管道操作符|对图片按顺序进行多种处理|

开通成功后，你只需在你自己的图片获取地址上拼接特定的参数，就可以实现图片的处理，比如，如果我们要将图片宽高调整为原来的 50% ，可以这么操作。

```
https://${您的云开发文件访问域名，格式如：xxx.tcb.qcloud.la}/sample.jpeg?imageMogr2/thumbnail/!50p
```

关于具体这个功能的使用，你可以参考 https://cloud.tencent.com/document/product/876/42103 或者咨询相关的产品 wilsonsliu(刘盛)


## Reference

- https://developers.weixin.qq.com/miniprogram/dev/api/media/video/wx.chooseMedia.html