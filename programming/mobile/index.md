# 微信内视频播放相关设置

```html
<video
  id="videoALL"
  src="video/01.mp4"
  poster="images/1.jpg" /*视频封面*/
  preload="auto"
  /*启用下面这三个可以设置微信内的视频在点击播放后不全屏,因为默认是全屏播放的*/
  webkit-playsinline="true" /*这个属性是ios 10中设置可以让视频在小窗内播放，也就是不是全屏播放*/
  playsinline="true"  /*IOS微信浏览器支持小窗内播放*/
  x-webkit-airplay="allow"

  x5-video-player-type="h5-page"  /*启用H5播放器,是wechat安卓版特性*/
  x5-playsinline="true"
  x5-video-player-fullscreen="true" /*全屏设置，设置为 true 是防止横屏*
  x5-video-orientation="portraint" /*播放器支付的方向，landscape横屏，portraint竖屏，默认值为竖屏*/
  style="object-fit:fill">
</video>


/*
X5内核视频之问答汇总  https://docs.qq.com/doc/DTUxGdWZic0RLR29B
微信视频播放全屏问题  https://cloud.tencent.com/developer/article/1399098
*/
```
