---
title: 关于cors预检-简单请求和复杂请求
date: 2022-03-03 17:01:51
categories: [work]
tags: [tips]
---

今天工作中发现了一个有意思的问题：axios 调用接口，浏览器返回了跨域错误，但是接口在服务器端还是调用成功了的，以前一直以为跨域错误会被浏览器直接拦截，今天仔细看了 mdn，才知道请求是分为简单请求和复杂请求的，简单请求不会发送 options 请求，复杂请求会发送一个预检请求 options，简单请求即使跨域，浏览器只会拦截返回值。

具体来看一下[mdn 文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS)对简单请求的描述。

### 简单请求

某些请求不会触发 CORS 预检请求。本文称这样的请求为“简单请求”，请注意，该术语并不属于 Fetch （其中定义了 CORS）规范。若请求 满足所有下述条件，则该请求可视为“简单请求”：

使用下列方法之一：

- GET
- HEAD
- POST

<!-- more -->

除了被用户代理自动设置的首部字段（例如 `Connection`，`User-Agent`）和在 Fetch 规范中定义为 禁用首部名称 的其他首部，允许人为设置的字段为 Fetch 规范定义的 对 CORS 安全的首部字段集合。该集合为：

- Accept
- Accept-Language
- Content-Language
- Content-Type （需要注意额外的限制）

Content-Type 的值仅限于下列三者之一：

- text/plain
- multipart/form-data
- application/x-www-form-urlencoded

请求中的任意 `XMLHttpRequest` 对象均没有注册任何事件监听器；`XMLHttpRequest` 对象可以使用 `XMLHttpRequest.upload` 属性访问。
请求中没有使用 `ReadableStream` 对象。

### 预检请求

与前述简单请求不同，“需预检的请求”要求必须首先使用 OPTIONS 方法发起一个预检请求到服务器，以获知服务器是否允许该实际请求。"预检请求“的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

具体参看 mdn 文档 [跨源资源共享（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS)
