---
title: "Gravatar 头像镜像站已上线"
categories: [ "程序人生" ]
tags: [ "cdn", "gravatar" ]
draft: false
slug: "gravatar"
date: "2021-10-27 00:23:00"
---

由于官方 https 站均被屏蔽，目前本站使用的是自建的 gravatar 镜像站，使用 Cachefly 全球 CDN 进行分发

说明地址：<https://gravatar.taurusxin.com/>

镜像地址：<https://gravatar.taurusxin.com/avatar/>

将你的博客中的头像地址改为 <https://gravatar.taurusxin.com/avatar/> 即可

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

  [1]: https://gravatar.taurusxin.com/avatar/
