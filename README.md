# 建议改用 webrtc 方式实现视频查看，本库此方案已经太老了

# Chrome 播放 RTSP demo

> 该项目是参考这篇文章产出的，所有文件也是该文章中提供的
> [https://blog.csdn.net/qq_34817440/article/details/105644393](https://blog.csdn.net/qq_34817440/article/details/105644393)

使用 vxg 播放器进行转码，可直接在 chrome 中播放 rtsp 监控视频。

只支持旧版的 Chrome 最新版的不支持。该库中的版本是 v72 可正常使用

## 一、安装 Chrome
安装后，为了避免浏览器自动升级成最新的，将目录中 `Update` 目录权限设为谁都不能改。

```bash
用户目录\AppData\Local\Google
# 如
C:\Users\Administrator\AppData\Local\Google
```
![screen2021_0803_1730](https://user-images.githubusercontent.com/12215982/127995792-e12ff156-4d01-4eb9-980f-f8a3877f5fc9.jpg)


或者为了避免覆盖自己在用的 Chrome 版本，可以直接使用绿色版，使用方法在这个链接里有说明：
> [https://blog.csdn.net/KimBing/article/details/119415005](https://blog.csdn.net/KimBing/article/details/119415005)


## 二、安装 Chrome 插件
1. 解压项目目录中的 `/chrome-extension/VXG-Media-Player_v1.8.42.rar` 文件
2. 【设置】 - 【更多工具】 - 【扩展程序】 打开浏览器的扩展程序页面，把开发者模式点开
3. 选择【加载已解压的扩展程序】，选择第 1 步解压的文件夹
4. 此时就能看到已经多出一个扩展程序名这 `VXG Media Player 1.8.42`

![screen2021_0803_1733](https://user-images.githubusercontent.com/12215982/127995796-6dcd0e44-c319-4855-860f-1d6b9a24bb11.jpg)

## 三、访问
修改 `index.html` 中的 rtsp 地址
将项目文件放到 `nginx` 或 `apache` 中访问该目录即可

![screen2021_0803_1704](https://user-images.githubusercontent.com/12215982/127995785-2767850c-4ef7-4556-a82b-ca8e6a10885d.jpg)


# Vue 版本中使用 vxg player
使用的文件是一样的，但需要将文件分开处理
因为目前插件是适用于浏览器版本的，不适用于 npm 版，所以需要将 js、css 文件在 `public/index.html` 中插入
将插件文件夹放于 `index.html` 同级目录下，也就是 `public` 下

```html
<link rel="stylesheet" href="./vxg-plugin/vxgplayer-1.8.40.min.css">
<script src="./vxg-plugin/vxgplayer-1.8.40.min.js"></script>
```
注意使用 `./` 引用文件

```html
<div class="vxgplayer" :id="`rtsp_player_${index}`"
     url="rtsp://admin:DEVdev123@192.168.30.235:554/h264/ch1/main/av_stream" autostart controls
     avsync nmf-src="./vxg-plugin/pnacl/Release/media_player.nmf" nmf-path="media_player.nmf"
     width="720" height="405">
</div>
```

这样就可以使用了，在页面载入完成后停留一会，再获取 vxgplayer 的 id 对其进行操作。
