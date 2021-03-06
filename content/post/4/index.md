---
title: "Axios 入门"
description: "Axios 是一个使用广泛 JavaScript Http 库，可以在浏览器和 Node.js 环境中使用，本文将对 Axios 做简要介绍。"
date: "2018-04-21"
slug: "4"
categories:
    - 技术
tags:
    - Axios
    - JavaScript
keywords:
    - axios
    - javascript
---

# 🤔 axios是什么
`axios` 是一个使用 `JavaScript` 编写的 `http` 库，可以在浏览器和 `Node.js` 环境中使用。你大可在 `React` 和 `Vue.js` 等框架中使用它，目前它是 `Vue.js` 官方推荐的 `http` 库之一。

对于不太了解 `http` 库的前端人员，可以直接把它理解成一个 `ajax` 库，你可以使用它来发送 `ajax` 请求(当然功能不局限于此)，就像 `jQuery` 中集成的 `ajax` 请求那样简单。

# 📦 安装 axios
直接使用 `CDN` 镜像:

```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

或者使用 `npm` 安装:

```
npm install axios
```

你也可以使用 `yarn` 或者其他包管理工具安装:

```
yarn add axios
```

# 🎉 使用 axios 发送异步 http 请求
发送 `GET` 请求：

```javascript
axios
  .get('/test?page=2')
  .then(function (response) {
    // 响应完成的钩子函数

    // 响应的body在response.data中，如果是以json格式传回，则可以直接使用，response中还有一些其他的响应内容
  })
  .catch(function (error)) {
    // 产生错误的钩子函数
  };
```

`GET` 请求传递参数的另外一种方式：

```javascript
axios
  .get('/test', {
    params: {
      page: 2
    }
  })
  .then(function (response) {
    // ...
  })
  .catch(function (error) {
    // ...
  });
```

发送 `POST` 请求：

```javascript
axios
  .post('/test', {
    page: 2
  })
  .then(function (response) {
    // ...
  })
  .catch(function (error) {
    // ...
  });
```

