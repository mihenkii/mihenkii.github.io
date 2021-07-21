title: webrtc-ios-video-orientation
date: 2021-07-15 15:32:03
tags: webrtc ios video orientation
---

## 问题
写了一个通过Webrtc加入视频会议的html页面，使用iPhone手机打开，发现摄像头采集的视频画面旋转了90度, 在视频会议里面人是横向的..
PC和Android都没有这个问题

iPhone -> Janus -> OWT

## 原因分析
1. 手机采集的视频就不是正向的
2. OWT服务端渲染时对视频进行了转向

我这里手机采集到的是正向的，所以是服务端对视频进行了旋转.
在SDP协商里，通过```a=extmap:13 urn:3gpp:video-orientation``` 控制视频旋转角度

**注意：每个人遇到的extmap:后面的数字可能不一样**

## 解决办法

判断是iPhone手机，就去掉SDP里面的CVO(Coordination of Video Orientation)

```
function iOS() {
 return [
    'iPhone Simulator',
    'iPhone'
 ].includes(navigator.platform)
}

function createdOffer(description) {
  if(iOS()) {
    var oldsdp = description["sdp"];
    var pattern=/a=extmap:13 urn:3gpp:video-orientation\r\n/gi;
    var newsdp = oldsdp.replace(pattern,"");
    description["sdp"]=newsdp;
  }
```
这个办法不是很好，会在真的有旋转画面需求的时候无法使用，在服务器合流的时候判断是否需要旋转应该更好一些。

## 参考文档
https://blog.csdn.net/epubcn/article/details/101451547
https://docs.flashphoner.com/display/WCS52EN/WebRTC+stream+picture+rotation
https://stackoverflow.com/questions/9038625/detect-if-device-is-ios
