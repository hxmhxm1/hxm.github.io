---
title: h5中的video
---

### 视频分辨率

指视频的宽度✖️视频高度，衡量分辨率的单位是像素
4K/2K/1080P/720P， 720P成为标准高清，低于720P的就不属于高清范围

### 视频码率

视频每秒传输的比特数（kbps mbps)c, 码率多大视频质量越好,但随着视频也越大

### 视频编码格式

标准格式的h264, 其它格式有MOV、 mpeg4等

### 在h5中使用

```js
<video
  id=''
  class=''
  poster='XX.jpg'
  src='XX.mp4'
  controls
  muted
  autoplay
  webkit-playsinline
  playsinline
></video>

// controls 显示进度条等控件
// autoplay 自动播放，这个属性需要和muted静音一起使用才会生效
// playsinline 和 webkit-playsinline 在ios系统中不设置该属性会导致视频直接弹出页面
```
