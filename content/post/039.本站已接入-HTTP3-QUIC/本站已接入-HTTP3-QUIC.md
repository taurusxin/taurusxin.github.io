---
title: "本站已接入 HTTP3/QUIC"
categories: [ "前沿技术探索" ]
tags: [  ]
draft: false
slug: "http3-enabled"
date: "2021-03-14 12:32:00"
---

在去年的时候，本站正式接入了TLS1.3，使得TLS握手更快，从而加快体验。

2020年10月，Google 正式宣布 Chrome 将支持 HTTP3

在最近一次的网站升级中，本站正式支持了 HTTP3。

![http3.png][1]

目前本站支持HTTP3最新草案，版本为 `H3/29`。

Chrome 启用 HTTP3 方法，在chrome启动命令行后添加如下选项

` --enable-quic --quic-version=h3-29`

即

`"C:\Program Files\Google\Chrome\Application\chrome.exe" --enable-quic --quic-version=h3-29`

![chrome.png][2]

注意空格

macOS上也是相同方法，只不过需要使用命令行来启动

#### 后言

因为国内运营商对于UDP支持欠佳，所以目前HTTP3只是在实践过程中，期待以后的表现。


  [1]: https://cdn.rhyland.cn/typecho/2021/03/14/http3.png
  [2]: https://cdn.rhyland.cn/typecho/2021/03/14/chrome.png