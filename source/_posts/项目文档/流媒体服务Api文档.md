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

# 点播接口

## 点播接口 - 01 上传点播文件

工作流程

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

---



## 点播接口 - 02 获取标签下对应的点播列表

* GET|POST

```http
/vod/list
```

參數

| 欄位           | 類型   | 描述                                              |
| :------------- | :----- | :------------------------------------------------ |
| folder**選填** | String | 子文件夹                                          |
| start          | Number | 分页开始,从零开始                                 |
| limit          | Number | 分页大小                                          |
| sort**選填**   | String | 排序字段                                          |
| order**選填**  | String | 排序顺序Allowed values: `ascending`, `descending` |
| q**選填**      | String | 查询参数                                          |

200

| 欄位       | 類型     | 描述                                                         |
| :--------- | :------- | :----------------------------------------------------------- |
| total      | Number   | 总数                                                         |
| rows       | Object[] | 分页数据                                                     |
| id         | String   |                                                              |
| name       | String   | 名称                                                         |
| size       | String   | 文件大小                                                     |
| type       | String   | 文件类型                                                     |
| status     | String   | 转码状态:(转码中-transing、等待转码-waiting、转码完成-done、转码失败-error)Allowed values: `transing`, `waiting`, `done`, `error` |
| duration   | String   | 时长                                                         |
| videoCodec | String   | 视频编码                                                     |
| audioCodec | String   | 音频编码                                                     |
| aspect     | String   | 宽高                                                         |
| error      | String   | 错误信息                                                     |
| sharedLink | String   | 分享链接                                                     |
| snapUrl    | String   | 封面图片链接                                                 |
| videoUrl   | String   | 点播播放链接                                                 |
| playNum    | Integer  | 点播次数                                                     |
| flowNum    | Integer  | 点播总流量(B)                                                |
| createAt   | String   | 创建时间, YYYY-MM-DD HH:mm:ss                                |
| updateAt   | String   | 更新时间, YYYY-MM-DD HH:mm:ss                                |

---



## 点播接口 - 03 下载点播文件

~~~mermaid
graph LR
解析M3U8地址  --> 返回文件地址
~~~

* get

```text
/vod/download/:id
```

| 参数  | 类型     | 描述   |
|-----|--------|------|
| id	 | String | 点播ID |

入参

* 使用ffmpeg将m3u8格式的文件转换成单独的mp4文件
* 通过Api实现文件下载的功能



## 点播接口 - 04 删除点播

**0.0.0** 

POST

```http
/vod/remove
```

## 參數

| 欄位 | 類型   | 描述 |
| :--- | :----- | :--- |
| id   | String |      |

- [成功](http://192.168.29.157:10086/apidoc/#success-examples-02vod-PostVodRemove-0_0_0-0)

```json
HTTP/1.1 200 OK
{"code":200,"msg":"Success"}
```

----



## 点播接口 - 05 批量删除点播文件

**0.0.0** 

POST

```http
/vod/removeBatch
```

參數

| 欄位 | 類型  | 描述 |
| :--- | :---- | :--- |
| ids  | Array |      |

- [成功](http://192.168.29.157:10086/apidoc/#success-examples-02vod-PostVodRemovebatch-0_0_0-0)

```json
HTTP/1.1 200 OK
{"code":200,"msg":"Success"}
```

---



# 直播接口

## 直播接口 - 01 新建/编辑直播

* post

```bash
/live/save
```

入参

| 参数                              | 类型    | 描述                                                         |
| :-------------------------------- | ------- | ------------------------------------------------------------ |
| id                         (选填) | String  | 不填时表示新建                                               |
| cid                        (选填) | String  | 自定义ID,新建时可以传递自定义ID,可包含大小写字母下划线中划线,需保证唯一性 |
| name                              | String  | 名称                                                         |
| recordReserve                     | String  | 录像保存(天)                                                 |
| actived                (选填)     | Boolean | 推流开关,默认开启                                            |
| shared                (选填)      | Boolean | 分享开关,默认关闭                                            |
| authed                (选填)      | Boolean | 推流鉴权,默认开启                                            |
| beginAt               (选填)      | String  | 推流有效期开始, YYYY-MM-DD HH:mm:ss                          |
| endAt                  (选填)     | String  | 推流有效期结束, YYYY-MM-DD HH:mm:ss                          |
| storePath           (选填)        | String  | 新建时才有效：直播间录像实际存储路径，此处配置绝对路径       |
| shareBeginTime(选填)              | String  | 分享开始时间                                                 |
| shareEndTime   (选填)             | String  | 分享结束时间                                                 |
| allowLivePlan     (选填)          | Boolean | 允许推流开关                                                 |
| allowRecordPlan (选填)            | Boolean | 允许录像开关                                                 |
| livePlanData       (选填)         | String  | 推流计划数据                                                 |
| recordPlanData (选填)             | String  | 录像计划数据                                                 |
| turntopush         (选填)         | String  | 转推地址                                                     |

出参

200

| 参数          | 类型    | 描述                          |
| ------------- | ------- | ----------------------------- |
| id            | String  |                               |
| name          | String  |                               |
| recordReserve | Number  | 录像保存(天)                  |
| actived       | String  | 推流开关                      |
| authed        | Boolean | 推流鉴权                      |
| shared        | Boolean | 分享开关                      |
| url           | String  | 推流地址                      |
| sharedLink    | String  | 分享链接                      |
| storePath     | String  | 存储路径                      |
| createAt      | String  | 创建时间, YYYY-MM-DD HH:mm:ss |
| updateAt      | String  | 更新时间, YYYY-MM-DD HH:mm:ss |

```json
{
    "id": "Kh3NQfQn7",
    "createAt": "2024-03-25 10:40:31",
    "updateAt": "2024-03-25 16:38:13",
    "name": "测试1",
    "recordReserve": 30,
    "shared": false,
    "shareBeginTime": null,
    "shareEndTime": null,
    "ws": false,
    "wsAudio": false,
    "authed": false,
    "actived": true,
    "beginAt": null,
    "endAt": null,
    "sign": "9C1gY3oGij",
    "storePath": null,
    "userId": null,
    "allowLivePlan": true,
    "livePlan": "{\"Monday\":\"\",\"Tuesday\":\"\",\"Wednesday\":\"\",\"Thursday\":\"\",\"Friday\":\"\",\"Saturday\":\"\",\"Sunday\":\"\"}",
    "allowRecordPlan": false,
    "recordPlan": "{\"Monday\":\"\",\"Tuesday\":\"\",\"Wednesday\":\"\",\"Thursday\":\"\",\"Friday\":\"\",\"Saturday\":\"\",\"Sunday\":\"\"}",
    "localIp": "127.0.0.1",
    "turntopush": null,
    "url": "rtmp://127.0.0.1/live/Kh3NQfQn7?sign=9C1gY3oGij",
    "sharedLink": null,
    "canPushLive": true,
    "canRecord": true
}
```

---



## 直播接口 - 02 获取直播列表

* post

```bash
/live/list
```

入参

| 参数                 | 类型   | 描述                                                  |
| :------------------- | ------ | ----------------------------------------------------- |
| start                | Number | 分页开始,从零开始                                     |
| limit                | Number | 分页大小                                              |
| sort        (选填)   | String | 排序字段                                              |
| order     (选填)     | String | 排序顺序<br>Allowed values: `ascending`, `descending` |
| q             (选填) | String | 查询参数                                              |

出参200

| 参数                        | 类型     |             描述              |
| :-------------------------- | -------- | :---------------------------: |
| total                       | Number   |             总数              |
| rows                        | Object[] |           分页数据            |
| id                          | String   |                               |
| actived                     | String   |           推流开关            |
| name                        | String   |                               |
| recordReserve               | Number   |         录像保存(天)          |
| authed                      | Boolean  |           推流鉴权            |
| shared                      | Boolean  |           分享开关            |
| url                         | String   |           推流地址            |
| sharedLink                  | String   |           分享链接            |
| storePath                   | String   |           存储路径            |
| session              (选填) | Object   | 直播信息, 有值时表示正在直播  |
| Application                 | String   |           应用名称            |
| AudioBitrate                | Number   |            音频率             |
| HLS                         | String   |            HLS地址            |
| HTTP-FLV                    | String   |         HTTP-FLV地址          |
| Id                          | String   |            推流ID             |
| InBitrate                   | Number   |           推送码率            |
| NumOutputs                  | Number   |           在线人数            |
| OutBitrate                  | Number   |           输出码率            |
| OutBytes                    | Number   |           输出流量            |
| RTMP                        | String   |         RTMP直播地址          |
| Time                        | String   |           直播时长            |
| VideoBitrate                | String   |           音频码率            |
| createAt                    | String   | 创建时间, YYYY-MM-DD HH:mm:ss |
| updateAt                    | String   | 更新时间, YYYY-MM-DD HH:mm:ss |

```json
{
    "rows": [
        {
            "id": "bwc0qt5SR",
            "createAt": "2024-01-17 15:47:52",
            "updateAt": "2024-03-25 11:13:40",
            "name": "测试",
            "recordReserve": 1,
            "shared": false,
            "shareBeginTime": null,
            "shareEndTime": null,
            "ws": false,
            "wsAudio": false,
            "authed": false,
            "actived": true,
            "beginAt": null,
            "endAt": null,
            "sign": "bw5AqtcIgz",
            "storePath": "",
            "userId": "admin",
            "allowLivePlan": false,
            "livePlan": "{\"Friday\":\"\",\"Monday\":\"\",\"Saturday\":\"\",\"Sunday\":\"\",\"Thursday\":\"\",\"Tuesday\":\"\",\"Wednesday\":\"\"}",
            "allowRecordPlan": false,
            "recordPlan": "{\"Friday\":\"\",\"Monday\":\"\",\"Saturday\":\"\",\"Sunday\":\"\",\"Thursday\":\"\",\"Tuesday\":\"\",\"Wednesday\":\"\"}",
            "localIp": "",
            "turntopush": "",
            "url": "rtmp://192.168.29.157:10035/live/bwc0qt5SR?sign=bw5AqtcIgz",
            "sharedLink": "/share.html?id=bwc0qt5SR&type=live&autoplay=yes&livetype=flv",
            "canPushLive": true,
            "canRecord": true,
            "session": {
                "Id": "bwc0qt5SR",
                "Name": "测试",
                "Type": "live",
                "Application": "live",
                "HLS": "/hls/bwc0qt5SR/playlist.m3u8",
                "HTTP-FLV": "/flv/live/bwc0qt5SR.flv",
                "WS-FLV": "/ws-flv/live/bwc0qt5SR.flv",
                "RTMP": "rtmp://192.168.29.157:10035/live/bwc0qt5SR",
                "RTSP": "rtsp://192.168.29.157:5545/live/bwc0qt5SR",
                "WebRTC": "/rtc/bwc0qt5SR",
                "Time": "1 m 8 s",
                "NumOutputs": 0,
                "InBytes": 270888283,
                "OutBytes": 3499,
                "PubSessionID": "RTMPPUBSUB3",
                "PublisherIP": "192.168.40.215",
                "InBitrate": 33004544,
                "OutBitrate": 0,
                "AudioBitrate": 0,
                "VideoBitrate": 0,
                "VideoHeight": 1080,
                "VideoWidth": 1920,
                "AudioChannel": 0,
                "AudioCodec": "AAC",
                "AudioSampleRate": 0,
                "AudioSampleSize": 0,
                "StartTime": "2024-03-25 11:14:50",
                "VideoCodec": "H264"
            }
        }
    ],
    "total": 8
}
```

---



## 直播接口 - 03 获取正在直播会话信息列表

POST

```http
/live/sessions
```

參數

| 欄位                | 類型   | 描述                                                         |
| :------------------ | :----- | :----------------------------------------------------------- |
| id**選填**          | string | 传递时返回单路的session信息                                  |
| application**選填** | String | 应用名称,不传默认查询所有Allowed values: `live`, `vlive`, `openLive`, `all` |

200

| 欄位            | 類型   | 描述                               |
| :-------------- | :----- | :--------------------------------- |
| Application     | String | 应用名称                           |
| AudioBitrate    | Number | 音频率                             |
| Type            | String | 类型                               |
| HLS             | String | HLS地址                            |
| HTTP-FLV        | String | HTTP-FLV地址                       |
| WS-FLV          | String | WS-FLV地址                         |
| Id              | String | 推流ID                             |
| InBitrate       | Number | 推送码率                           |
| InBytes         | Number | 推送流量                           |
| NumOutputs      | Number | 在线人数                           |
| OutBitrate      | Number | 输出码率                           |
| OutBytes        | Number | 输出流量                           |
| RTMP            | String | RTMP直播地址                       |
| RTSP            | String | RTSP直播地址                       |
| Time            | String | 直播时长                           |
| VideoBitrate    | String | 视频码率                           |
| Name            | String | 直播流名称，匿名直播名称为空字符串 |
| VideoCodec      | String | 视频编码                           |
| AudioCodec      | String | 音频编码                           |
| StartTime       | String | 开始时间                           |
| AudioSampleRate | Number | 音频采样率                         |
| AudioChannel    | Number | 音频通道                           |
| VideoHeight     | Number | 视频高度                           |
| VideoWidth      | Number | 视频宽度                           |
| PublisherIP     | String | 推送者ip                           |

- [返回示例](http://192.168.29.157:10086/apidoc/#success-examples-03live-PostLiveSessions-0_0_0-0)

```json
[
    {
        "Id": "nmted1",
        "Name": "nmted1",
        "Type": "openLive",
        "Application": "hls",
        "HLS": "/hls/nmted1/nmted1_live.m3u8",
        "HTTP-FLV": "/flv/hls/nmted1.flv",
        "WS-FLV": "",
        "RTMP": "rtmp://demo.easydss.com:10085/hls/nmted1",
        "RTSP": "rtsp://demo.easydss.com:10554/nmted1",
        "Time": "0h 33m 1s",
        "NumOutputs": 2,
        "InBytes": 655953750,
        "OutBytes": 1378478815,
        "PublisherIP": "114.97.242.136",
        "InBitrate": 361795,
        "OutBitrate": 723591,
        "AudioBitrate": 22215,
        "VideoBitrate": 339580,
        "VideoHeight": 720,
        "VideoWidth": 1280,
        "AudioChannel": 2,
        "AudioCodec": "AAC",
        "AudioSampleRate": 44100,
        "AudioSampleSize": 16,
        "StartTime": "2020-07-29 14:37:32",
        "VideoCodec": "H264"
    },
    {
        "Id": "nmted1",
        "Name": "nmted1",
        "Type": "openLive",
        "Application": "record",
        "HLS": "/EasyDSS-windows-3.0.0-2007211602/data/record/nmted1/nmted1_live.m3u8",
        "HTTP-FLV": "/flv/record/nmted1.flv",
        "WS-FLV": "",
        "RTMP": "rtmp://demo.easydss.com:10085/record/nmted1",
        "RTSP": "rtsp://demo.easydss.com:10554/nmted1",
        "Time": "0h 33m 1s",
        "NumOutputs": 0,
        "InBytes": 655953750,
        "OutBytes": 0,
        "PublisherIP": "127.0.0.1",
        "InBitrate": 361795,
        "OutBitrate": 0,
        "AudioBitrate": 22215,
        "VideoBitrate": 339580,
        "VideoHeight": 720,
        "VideoWidth": 1280,
        "AudioChannel": 2,
        "AudioCodec": "AAC",
        "AudioSampleRate": 44100,
        "AudioSampleSize": 16,
        "StartTime": "2020-07-29 14:37:32",
        "VideoCodec": "H264"
    }
]
```

----



## 直播接口 - 04 直播流开关

POST

```http
/live/turn/actived
```

參數

| 欄位    | 類型    | 描述                    |
| :------ | :------ | :---------------------- |
| id      | String  | 直播流ID                |
| actived | Boolean | 开启：true，关闭：false |

- [成功](http://192.168.29.157:10086/apidoc/#success-examples-03live-PostLiveTurnActived-0_0_0-0)

```json
HTTP/1.1 200 OK
{"code":200,"msg":"Success"}
```

---



## 直播接口 - 05 推流鉴权

**0.0.0** 

POST

```http
/live/turn/authed
```

參數

| 欄位   | 類型    | 描述 |
| :----- | :------ | :--- |
| id     | String  |      |
| authed | Boolean |      |

- [成功]()

```json
HTTP/1.1 200 OK
{"code":200,"msg":"Success"}
```

----



## 直播接口 - 06 删除直播

**0.0.0** 

POST

```http
/live/remove
```

## 參數

| 欄位           | 類型    | 描述                                                         |
| :------------- | :------ | :----------------------------------------------------------- |
| id             | String  |                                                              |
| record**選填** | Boolean | 同时删除录像否, 该参数只对非正在直播的条目有效預設值: `false` |

- [成功]()

```json
HTTP/1.1 200 OK
{"code":200,"msg":"Success"}
```

---

## 直播接口 - 07 获取单条/多条直播流信息

**0.0.0** 

GET|POST

```http
/live/get
```

參數

| 欄位 | 類型   | 描述                                                         |
| :--- | :----- | :----------------------------------------------------------- |
| id   | String | 传递单个id，返回单条直播流信息；若英文逗号拼接传递多个，返回的多个直播流信息数组； |

200

| 欄位            | 類型    | 描述                          |
| :-------------- | :------ | :---------------------------- |
| id              | String  |                               |
| name            | String  |                               |
| recordReserve   | Number  | 录像保存(天)                  |
| actived         | String  | 推流开关                      |
| authed          | Boolean | 推流鉴权                      |
| shared          | Boolean | 分享开关                      |
| url             | String  | 推流地址                      |
| sharedLink      | String  | 分享链接                      |
| storePath       | String  | 存储路径                      |
| createAt        | String  | 创建时间, YYYY-MM-DD HH:mm:ss |
| updateAt        | String  | 更新时间, YYYY-MM-DD HH:mm:ss |
| session**選填** | Object  | 直播信息, 有值时表示正在直播  |
| Application     | String  | 应用名称                      |
| AudioBitrate    | Number  | 音频率                        |
| HLS             | String  | HLS地址                       |
| HTTP-FLV        | String  | HTTP-FLV地址                  |
| Id              | String  | 推流ID                        |
| InBitrate       | Number  | 推送码率                      |
| InBytes         | Number  | 推送流量                      |
| NumOutputs      | Number  | 在线人数                      |
| OutBitrate      | Number  | 输出码率                      |
| OutBytes        | Number  | 输出流量                      |
| RTMP            | String  | RTMP直播地址                  |
| Time            | String  | 直播时长                      |
| VideoBitrate    | String  | 音频码率                      |

---





# 录像接口

## 录像回看接口 - 01 查询有录像设备

* post

```bash
/record/query_devices
```

入参

| 参数                 | 类型   | 描述                                                  |
| :------------------- | ------ | ----------------------------------------------------- |
| start                | Number | 分页开始,从零开始                                     |
| limit                | Number | 分页大小                                              |
| sort        (选填)   | String | 排序字段                                              |
| order     (选填)     | String | 排序顺序<br>Allowed values: `ascending`, `descending` |
| q             (选填) | String | 查询参数                                              |

出参

| 欄位     | 類型     | 描述                          |
| :------- | :------- | :---------------------------- |
| total    | Number   | 总数                          |
| rows     | Object[] | 分页数据                      |
| id       | String   |                               |
| name     | String   |                               |
| updateAt | String   | 更新时间, YYYY-MM-DD HH:mm:ss |

```json
{
    "rows": [
        {
            "id": "rAl_qtcIg",
            "createAt": null,
            "updateAt": "2024-03-14 16:27:49",
            "name": "测试直播间",
            "recordReserve": 1,
            "type": "live",
            "storePath": "",
            "localIp": "172.17.0.1"
        }
    ],
    "total": 12
}
```

---



## 录像回看接口 - 02 指定时间段录像播放及下载

* GET

```http
/record/video/:operate/:id/:starttime/:endtime
```

參數

| 欄位      | 類型   | 描述                                                         |
| :-------- | :----- | :----------------------------------------------------------- |
| operate   | String | 调用操作 play:播放 download下载 synthesis 合成Allowed values: `play`, `download` |
| id        | String | 设备ID                                                       |
| starttime | String | 开始时间, YYYYMMDDHHmmss                                     |
| endtime   | String | 结束时间, YYYYMMDDHHmmss                                     |

- [播放示例]()

```json
http://127.0.0.1:10080/record/video/play/teet/20180911101139/20180911101248
```

- [下载示例]()

```json
http://127.0.0.1:10080/record/video/download/teet/20180911101139/20180911101248
```

---



## 录像回看接口 - 03 查询设备所有录像记录

**0.0.0** 

GET|POST

```http
/record/query_flags
```

參數

| 欄位 | 類型   | 描述 |
| :--- | :----- | :--- |
| id   | String |      |

200

| 欄位  | 類型   | 描述                                               |
| :---- | :----- | :------------------------------------------------- |
| key   | String | 月份, YYYYMM                                       |
| value | String | 标记当月每一天是否有录像, 0 - 没有录像, 1 - 有录像 |

- [成功示例](http://192.168.29.157:10086/apidoc/#success-examples-05record-Get|postRecordQuery_flags-0_0_0-0)

```json
{201803: "0000000011000000000000000000000"}
```
