---
layout: post
title:  "如何在vue模版里面局部引用cdn外部远程连接，比如js，css"
date:   2019-01-16 17:05:00

categories: javascript
tags: vue
author: "choi"
---


要引入第三方资源，常规的做法是在`index.html`中引入。如下：
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>vuetest</title>
    <link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"> 
  </head>
  <body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```
但是又不想影响其他页面，只想在本页显示。所以网上搜了下。我最终选择了第一种。
---

原文链接：[https://blog.yourtion.com/vue-require-remote-js.html](https://blog.yourtion.com/vue-require-remote-js.html)

### 问题
最近在使用 Vue 做东西，用到钉钉扫描登录的功能，这里需要引入远程的 js 文件，因为 Vue 的方式跟之前的不太一样，又不想把文件下载到本地应用，找了一下解决的方法，貌似都需要引入第三方的库，最后找到了解决方案，分享之。

### 思路
一开始的思路是在 Vue 加载完 Dom 之后（`mounted1`），使用 JavaScript 脚本在 body 中插入远程的脚本文件。

后来发现了 Vue 的 `createElement` 方法，简单的封装一个组件解决问题。

### 解决方法
第一版代码（直接在操作 Dom ）如下：
```
export default {
  mounted() {
    const s = document.createElement('script');
    s.type = 'text/javascript';
    s.src = 'https://g.alicdn.com/dingding/dinglogin/0.0.2/ddLogin.js';
    document.body.appendChild(s);
  },
}
```
第二种使用 createElement 方法：
```
export default {
  components: {
    'dingtalk': {
      render(createElement) {
        return createElement(
          'script',
          {
            attrs: {
              type: 'text/javascript',
              src: 'https://g.alicdn.com/dingding/dinglogin/0.0.2/ddLogin.js',
            },
          },
        );
      },
    },
  },
}

// 使用 <dingtalk></dingtalk> 在页面中调用
```
**终极方案**
通过封装一个组件 `remote-js` 实现：
```
export default {
  components: {
   'remote-js': {
    render(createElement) {
      return createElement('script', { attrs: { type: 'text/javascript', src: this.src }});
    },
    props: {
      src: { type: String, required: true },
    },
  },
  },
}
```
使用方法：
```
<remote-js src="https://g.alicdn.com/dingding/dinglogin/0.0.2/ddLogin.js"></remote-js>
```

因为刚开始学习 Vue 有什么问题欢迎大家指出，大家一起讨论讨论。

------

**我最终用了第一种，如下：**
```
mounted () {
    const link = document.createElement('link')
    link.type = 'text/css'
    link.rel = 'stylesheet'
    link.href = 'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css'
    document.head.appendChild(link)
  },
```
