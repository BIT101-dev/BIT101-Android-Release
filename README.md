<!--
 * @Author: flwfdd
 * @Date: 2023-09-19 21:24:06
 * @LastEditTime: 2023-09-19 22:05:22
 * @Description: _(:з」∠)_
-->
# BIT101-Android-Release
[BIT101-Android](https://github.com/BIT101-dev/BIT101-Android) 的官网，用于展示、更新检查和下载。

**💡 该网站需要部署到`android.bit101.cn`，`APP`更新检查依赖于此。**

## 文件说明
* `/index.html`：网站展示页面，无其他功能。
* `/release`：存放软件包。
* `/version`：包含了版本更新描述文件，是`APP`内更新检查的依据，要保证访问`/version`时能正确返回，部署中设置好默认返回`index.html`即可。

## 部署方式
当前有两种方式部署：
1. 使用`Cloudflare Pages`绑定仓库
2. 使用`GitHub Action`自动上传到阿里云`OSS`

`Cloudflare`现在传输速度还比较快，但不一定能一直稳定，但阿里云流量太贵了，所以还是主用`Cloudflare`，阿里云作为冗余备份。阿里云进行了静态域名相关配置，必要时直接更改`DNS`就能快速切换。

## 更新流程
当要进行版本更新时，遵循如下步骤：

### 1、上传新的软件包
由于软件包较大，为了不让仓库膨胀导致难以维护，使用`Git LFS`（`Git Large File Storage`）存储二进制`APK`文件。

将软件包以`BIT101-x.x.x.apk`的格式命名并放置在`release`目录下，旧版本软件包可以选择性删除，然后运行：
```bash
git lfs track "*.apk"
```
即可将文件以`Git LFS`上传。注意需要先在电脑上安装`Git LFS`功能。

### 2、更新版本文件
更新`/version/index.html`，形如`JSON`：
```json
{
	"min_version_code": 233,
	"min_version_name":"x.x.x",
	"version_code": 2333,
	"version_name": "x.x.x",
	"url": "http://android.bit101.cn/release/BIT101-x.x.x.apk",
	"msg": "💡更新说明\n\n更新内容：\n1. 你说得对\n2. 但是"
}
```

其中`min_version_code`为最低支持版本号，`min_version_name`为最低支持版本名，`version_code`为当前版本号，`version_name`为当前版本名，`url`为下载链接，`msg`为更新说明。

部署后，`APP`内就会收到更新推送消息。

### 3、更新主页

更新`/index.html`中的下载链接。
