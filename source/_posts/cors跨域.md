---
layout: post
title: cors跨域
date: 2017-09-17 15:17:43
tags:
---
  出于安全的考虑，浏览器会限制跨域的ajax请求。但是实际场景确实需要跨域请求，所以w3c定制了cors跨域方案。

其实跨域ajax请求请求并返回了，只不过浏览器在返回时做判断服务器向响应是否可以跨域。有一个chrome的cors插件配置跨域其实就时告诉浏览器别做这个判断，还有在chrome启动的时候传参数 允许跨域访问

## cors分为简单跨域请求和复杂跨域请求  ##

### 简单请求 ###
1. GET和HEAD请求
2. POST请求且Content-Type为text/plain、multipart/form-data、application/x-www-form-urlencoded其中一个
满足上面条件就是简单请求，否则时复杂请求

发起简单请求时浏览器会在这个请求header中自动加一个origin字段，值是当前请求源（协议+域名+端口），服务器在响应请求header中指定了Access-Control-Allow-Origin: * | 请求Origin值，则简单跨域请求完成。


### 复杂请求 ###
复杂请求比简单请求多了个预检查请求。预检查请求会会向服务器发送一个OPTIONS请求，用来检查服务器是否允许当前实际请求。主要是为了避免跨域请求非法请求对服务器数据带来影响。

预检查请求除了添加了origin字段还有加了Access-Control-Request-Method用来告诉服务器实际请求方式，Access-Control-Request-Heaers用来告诉服务器额外的header信息。
服务器收到预检查后，检查这三个字段确认是否允许跨域请求。如果允许响应header包括Access-Control-Allow-Orign: * | 请求Origin值,
Access-Control-Allow-Methods: GET, POST, PUT(其中有实际请求方,Access-COntrol-Allow-Headers: 包含浏览器传来的header信息
预检查请求通过后发出实际请求。

如果请求带cookie ajax请求必须把withCredentials设为true, 服务器必须响应Access-Control-Allow-Credentials: true, 且Access-Control-Allow-Origin值不能为*|多个域名


复杂请求发出两次请求，但通常我们只看到了一次请求，那是因为为避免过多无用请求通过Access-Control-Max-Age设置了预检查请求缓存，

## demo ##
服务器端跨域