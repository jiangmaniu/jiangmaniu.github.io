---
title: RESTful API
date: 2021-02-18 16:42:12
tags:
  - API
  - 后端
  - 学习笔记
description: RESTful API 设计设计理念
---

## 什么是 RESTful API

按照现在的发展趋势，前端设备是千奇百怪的，不管是客户端、手机应用、小程序还是Web应用，不管是完成部分还是整体的功能，都可以从统一的后端里面拿到对应的信息。

因此必须同一的机制，方便不同的前端应用与后端进行通信，这导致 API 架构非常的流行，RESTful API 是目前比较成熟的一套互联网应用程序的 API 设计理论。

## 域名

尽量将 API 不熟在专用域名下。

``` sh
https://api.example.com
```

如果确定 API 比较简单，不会有进一步扩展，可以考虑放在主域名下。

```sh
https://example.com/api/
```

## 版本（Versioning）

若同个 API 有多版本，需要 API 请求中添加版本号。

一般将版本号放入 URL。

```sh
https://api.example.com/v1/
```

另一种做法是，将版本号放在 HTTP 头信息中（缺点是不如放在 URL 方便和直观），[Github](https://developer.github.com/v3/media/#request-specific-version) 采用的就是这种做法。

## 路径（Endpoint）

路径又称“终点”，表示具体的 API 网址。

在 RESTful API 架构中，每个网址代表一种资源（resource)，所有网址中不能有**动词** ，只能有**名词**，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的“集合”（collection)，所以 API 中的名词也应该使用复数。

例子；

有一个 API 提供 NBA 的球队和球队成员的信息，则它的路径应该设计下面这样。

```sh
// 球队
https://api.example.com/v1/teams
// 球队成员
https://api.example.com/v1/players
```

## HTTP 动词

HTTP 动词表示根据不同操作需求类型，发送对应的 HTTP 请求。

常用的 HTTP 动词有五个（括号内是对应的 SQL 命令）。

- GET (SELECT)：从服务器取出资源（一项或多项）。
- POST (CREATE)：在服务器新建一个资源。
- PUT (UPDATE)：在服务器更新资源（客户端需提供改变后**完整**的资源）。
- PATCH（UPDATE）：在服务器更新资源（客户端提供**要改变**的资源）。
- DELETE（DELETE）：在服务器删除资源。

还有两个不常用的 HTTP 动词。

- HEAD：获取资源的元数据。
- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

例子：

- GET /teams 列出所有球队
- POST /teams 创建一个球队
- GET /teams/ID 获取某个指定的球队信息
- PUT /teams/ID 更新某个指定的球队信息（需要提供球队**完整**的信息）
- PATCH /teams/ID 更新某个指定的球队信息（需要提供球队**要更改**的信息）
- DELETE /teams/ID 删除某个指定的球队
- GET /teams/ID/players 列出指定球队的所有成员
- DELETE /teams/ID/players/ID 删除指定球队的指定成员

## 过滤信息（Filtering）

如果记录数量很多，服务器不可能都将它们返回给客户端。API 应该提供参数，过滤返回结果

常见参数：

- ?limit=10 指定返回记录的数量
- ?offset=10 指定返回记录的起始位置
- ?page=2&per_page=100 指定第几页，以及每页的记录数量
- ?sortby=name&order&order=aes 指定返回结果按照某个属性排序，以及排序顺序

## 状态码（Status Codes）

服务器向用户返回的状态码和提示信息，状态码分为五类：信息响应(100–199)，成功响应(200–299)，重定向(300–399)，客户端错误(400–499)和服务器错误 (500–599)，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

- 200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
- 201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
- 202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
- 204 NO CONTENT - [DELETE]：用户删除数据成功。
- 400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
- 401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
- 403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
- 404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
- 406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
- 410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
- 422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
- 500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

状态码的完全列表[参见](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)这里。

## 返回结果

针对不同操作，服务器向用户返回的结果应该符合以下规范。

- GET /collection：返回资源对象的列表（数组）
- GET /collection/resource：返回单个资源对象
- POST /collection：返回新生成的资源对象
- PUT /collection/resource：返回完整的资源对象
- PATCH /collection/resource：返回完整的资源对象
- DELETE /collection/resource：返回一个空文档

## 接口文档需要包含的点

- endpoints: 具体路径
- 使用怎样的 method
- 


## 参考资料

- <http://www.ruanyifeng.com/blog/2014/05/restful_api.html>
