---
title: "React Router 使用 Url 传参后改变页面参数不刷新的解决方法"
description: "如果在 React Router 中使用了 Url 传参功能，如果只有 Url 参数发生了改变，可能不会触发页面的刷新，这是因为对 React 组件的生命周期理解不到位导致的，本文将详细阐述 bug 的发生的原因。"
date: "2018-05-07"
slug: "7"
categories:
    - 技术
tags:
    - React
    - JavaScript
keywords:
    - react
    - router
    - url
---

# 🤔 问题
今天在写页面的时候发现一个问题，就是在 `React Router` 中使用了 `Url` 传参的功能，像这样:

```javascript
export class MainRouter extends React.Component {
    render() {
        return (
            <BrowserRouter>
                <Switch>
                    ...
                    <Route exact path={'/channel/:channelId'} component={ChannelPerPage}/>
                    ...
                </Switch>
            </BrowserRouter>
        );
    }
}
```

按照官方文档的说法，可以在 `ChannelPerPage` 这个组件中使用

```javascript
this.props.match.params
```

来获取 `url` 参数的值，但是我发现如果你在这个 `url` 下只将 `url` 中的参数部分改变，比如 `channelId` 从 `1` 变成 `2` 的时候，页面并不会重新渲染。

# 🧐 解决办法
查阅资料后发现这样的根本原因是 `props` 的改变并不会引起组件的重新渲染，只有 `state` 的变化才会引起组件的重新渲染，而 `url` 参数属于 `props`，故改变 `url` 参数并不会引起组件的重新渲染。

后来发现React的组件中有一个可复写的方法

```javascript
componentWillReceiveProps(nextProps) {
  ...
}
```

这个方法可以在 `React` 组件中被复写，这个方法将会在 `props` 改变的时候被调用，所以你可以使用这个方法将 `nextProps` 获取到，并且在这个方法里面修改 `state` 的内容，这样就可以让组件重新被渲染。

