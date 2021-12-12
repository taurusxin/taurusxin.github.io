---
title: "Gravatar 头像镜像站已上线"
categories: [ "程序人生" ]
tags: [  ]
draft: false
slug: "gravatar"
date: "2021-10-27 00:23:00"
---

由于官方 https 站均被屏蔽，目前本站使用的是自建的 gravatar 镜像站，使用 nginx 进行反代

说明地址：https://gravatar.xingez.me/
镜像地址：https://gravatar.xingez.me/avatar/

由于刚刚上线，线路为美国洛杉矶服务器，预计在一个月内部署全球CDN，包括中国大陆加速。

!!!
<div>CDN是否已部署：<span style="color: red; font-weight: bold;">否</span></div><br />
!!!

将你的博客中的头像地址改为 https://gravatar.xingez.me/avatar/ 即可

缓存规则如下

对 200、301 或者 302 等有效代码缓存的时间长度，特定参数 any 表示对任何响应都缓存一定时间长度

| HTTP  Status Code | 缓存时长 |
| ----------------- | -------- |
| 200 304           | 7 天    |
| 301               | 1 天    |
| 500 502 503 504   | 不缓存 |
| any               | 1 天    |

镜像站测试
![默认头像][1]

若默认头像显示正确，则镜像站工作正常


  [1]: https://gravatar.xingez.me/avatar/