---
title: "使用原生 JavaScript 封装 Ajax 操作"
description: "Ajax 是异步 Http 请求的代名词，它往往被封装在了各种现有前端框架中，但是如果要手动实现，则需要使用到原生的 XMLHttpRequest，本文将给出一个简要的原生 JavaScript Ajax 封装。"
date: "2018-04-24"
slug: "5"
categories:
    - 技术
tags:
    - JavaScript
    - Http
    - Ajax
keywords:
    - ajax
    - javascript
---

# 📦 封装举例

```javascript
export class Ajax {
    static get(url, data, hook) {
        let xmlHttpRequest = new XMLHttpRequest();

        url += '?';
        let count = -1;
        for (let key in data) {
            count++;
            if (data.hasOwnProperty(key)) {
                url += count === 0 ? key + '=' + data[key] : '&' + key + '=' + data[key];
            }
        }

        xmlHttpRequest.open('GET', url, true);
        xmlHttpRequest.onreadystatechange = () => {
            if (xmlHttpRequest.readyState === 4 &&
                xmlHttpRequest.status === 200 ||
                xmlHttpRequest.status === 304) {
                hook(xmlHttpRequest.responseText)
            }
        };
        xmlHttpRequest.send();
    }

    static post(url, data, hook) {
        let xmlHttpRequest = new XMLHttpRequest();

        let formatData = '';
        let count = -1;
        for (let key in data) {
            count++;
            if (data.hasOwnProperty(key)) {
                formatData += count === 0 ? key + '=' + data[key] : '&' + key + '=' + data[key];
            }
        }

        xmlHttpRequest.open('POST', url, true);
        xmlHttpRequest.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xmlHttpRequest.onreadystatechange = () => {
            if (xmlHttpRequest.readyState === 4 &&
                xmlHttpRequest.status === 200 ||
                xmlHttpRequest.status === 304) {
                hook(xmlHttpRequest.responseText)
            }
        };
        xmlHttpRequest.send(formatData);
    }
}
```

# 🎫 关于 XMLHttpRequest
其实 `ajax` 无非就是异步网络请求而已，各种语言都有自己的 `http` 库，只要使用 `http` 库基本上都能自己实现 `ajax` 的功能，在 `js` 中的原生 `http` 库则是 `XMLHttpRequest`，使用 `XMLHttpRequest` 发送一个请求有几个步骤，第一步是打开连接。

```javascript
let xmlHttpRequest = new XMLHttpRequest();
// 三个参数分别是请求类型，URL和是否异步
xmlHttpRequest.open(TYPE, URL, ASYNC);
```

如果是 `POST` 请求或是一些自定义的请求，则还需要添加头部内容

```javascript
// 两个参数分别是请求头键值
xmlHttpRequest.setRequestHeader(HEADER_KEY, HEADER_VALUE);
```

如果是异步请求，则需要设定完成相应之后的回调

```javascript
// 这个是指readystate变化的时候触发的事件，如果请求成功，会返回200或者304，所以我们在这里面调用回调，当然你也可以在这里设置出错的时候调用的回调函数
xmlHttpRequest.onreadystatechange = () => {
  if (xmlHttpRequest.readyState === 4 &&
      xmlHttpRequest.status === 200 ||
      xmlHttpRequest.status === 304) {
      hook(xmlHttpRequest.responseText)
  }
};
```

然后则可以发送请求

```javascript
// 如果是get，则数据以键值对的形式带在url中发送，如果是post，发送的data应该写在这里
xmlHttpRequest.send(DATA);
```

请求完成后悔自动调用之前设定的钩子函数

