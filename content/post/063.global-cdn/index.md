---
title: "博客正式接入国内备案及 CDN"
categories: [ "事件" ]
tags: [ "备案", "CDN" ]
draft: false
slug: "global-cdn-beian"
date: "2022-09-01 12:13:00"
---

## 前言

> 如果你从世界上任何一个接入优质互联网的地点打开本站，会发现页面内容基本都是秒开。

博客自 2021 年 3 月 13 日接入了 Cloudflare 的全球 CDN（不包含中国大陆区域），在全球都有着不错的响应速度，从按下回车到页面的完整呈现，都不会超过 3 秒，但是由于 CF 接入大陆需要企业订阅，并且需要备案，所以着将域名迁移到国内（腾讯云），然后接入备案，最后接入国内的 CDN。

## 备案

2022 年 8 月 22 日提交备案，8 月的最后一天，也就是昨天 31 号，拿到了工信部的备案号，立即被我添加到网站底部。

## 运作方式

### 构建系统

博客基于 Hugo 静态网页构建系统，使用的主题是 [hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack)

![hugo-theme-stack](https://cdn.taurusxin.com/hugo/2022/09/01/hugo-theme-stack.jpg)

### 源代码

源代码托管于 [GitLab](https://gitlab.com/taurusxin/hugo)

### 主站 CDN

目前将访问接入改为以下布局，即国外走 CF，国内走腾讯云 CDN。

### 静态资源 CDN

之前一直用的是公司的 `cdn.rhyland.cn` 作为博客的图片分发，备案通过后启用 `cdn.taurusxin.com` 静态资源域名，内容位于腾讯云上海 COS

### DNS

由于需要分离成国内外的 CDN，所以需要使用 DNS 进行 CNAME 分流，在经过对比之后，选用了华为云的 DNS 服务，优点在于其在中国大陆和全球各地均有 DNS 服务器，使得全球解析更快。

## 完成图

![taurusxin.com](https://cdn.taurusxin.com/hugo/2022/09/01/taurusxin.com.png)
