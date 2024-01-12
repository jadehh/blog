---
title: 流媒体服务Api文档
categories: 项目文档
author: 简得辉
tags:
  - 项目文档
keywords: 流媒体服务Api文档
abbrlink: 20240109143105
data: 2024/1/9 14:31
---

流媒体Api文档

### 点播接口 - 01 上传点播文件

#### 工作流程

~~~mermaid
graph LR
上传文件 --> 服务器判断有效性 --> FFMPeg切片处理 --> 生成M3U8点播文件 --> 返回视频URL 
~~~

* post

```text
/vod/upload
```

入参

| 参数             | 类型      | 描述                                                       |
|----------------|---------|----------------------------------------------------------|
| file	          | File    | 上传文件                                                     |
| path (选填)      | 	String | 上传子目录                                                    |
| resolution(选填) | String  | 多清晰度转码 如 yh,hfd,hd,sd 其中yh:原始视频(必须包含)，hfd:超清，hd:高清，sd:标清 |

出参
200

| 参数         | 类型      | 描述                                                                                                      |
|------------|---------|---------------------------------------------------------------------------------------------------------|
| id         | String  | 名称                                                                                                      |
| size       | String  | 文件大小                                                                                                    |
| type       | String  | 文件类型                                                                                                    |
| status     | String  | 转码状态:(转码中-transing、等待转码-waiting、转码完成-done、转码失败-error)<br>Allowed values: transing, waiting, done, error |
| duration   | String  | 时长                                                                                                      |
| videoCodec | String  | 视频编码                                                                                                    |
| audioCodec | String  | 音频编码                                                                                                    |
| aspect     | String  | 宽高                                                                                                      |
| error      | String  | 错误信息                                                                                                    |
| sharedLink | String  | 分享链接                                                                                                    |
| snapUrl    | String  | 封面图片链接                                                                                                  |
| videoUrl   | String  | 点播播放链接                                                                                                  |
| playNum    | Integer | 点播次数                                                                                                    |
| flowNum    | Integer | 点播总流量(B)                                                                                                |
| createAt   | String  | 创建时间, YYYY-MM-DD HH:mm:ss                                                                               |
| updateAt   | String  | 更新时间, YYYY-MM-DD HH:mm:ss                                                                               |

```json
{
  "id": "VcN09XFSg",
  "createAt": "2024-01-10 15:26:31",
  "updateAt": "2024-01-10 15:26:31",
  "name": "20220518093511934749_back",
  "size": 9069143,
  "type": "video/mp4",
  "path": "/home/samples/SoftWare/EasyDSS-linux-4.6.0-23100914/data/vodsrc/VcN09XFSg.mp4",
  "folder": "VcN09XFSg",
  "status": "waiting",
  "duration": 14,
  "videoCodec": "H.264",
  "audioCodec": "",
  "videoCodecOriginal": "H.264",
  "audioCodecOriginal": "",
  "aspect": "1908x1080",
  "error": "",
  "shared": false,
  "shareBeginTime": null,
  "shareEndTime": null,
  "rotate": 0,
  "resolution": "",
  "isresolution": false,
  "resolutiondefault": "hd",
  "transvideo": false,
  "userId": "",
  "localIp": "172.17.0.1",
  "snapUrl": "",
  "videoUrl": "/fvod/VcN09XFSg/video.m3u8",
  "sharedLink": "/share.html?id=VcN09XFSg&type=vod",
  "flowNum": 0,
  "progress": 0,
  "playNum": 0
}
```

## 视频下载

* 使用ffmpeg将m3u8格式的文件转换成单独的mp4文件
* 通过Api实现文件下载的功能
