---
title: whistle实战
date: 2018-11-28 14:43:48
tags: ["whistle","抓包","调试"]
---
# 前言
在日常前端开发中，各位同学多少都应该接触过网络请求代理工具（如： fiddler、charles等工具），使用它们来进行请求分析、伪造请求、移动端代理开发调试等。而 whistle 是一款 Node 实现的一款开源跨平台 Web 调试代理工具，可以简单理解为 Web 版的 fiddler / charles。从使用体验上来说，高效易用，没有fiddler复杂的规则配置，映射域名也十分方便，而且提供了更多额外的功能,还支持插件扩展。

本文章所有内容都建立在你已经安装了 Node 的前提之上。

## 安装
`npm install -g whistle`

## 运行
```javascript
whistle start
// 或
w2 start
```
启动成功后，即可见此信息（默认端口号为:8899）：
![](https://res.cloudinary.com/daq48zmrm/image/upload/v1571938672/img_whistle_01_idfsjv.png)
访问 `127.0.0.1:8899` 即可进入配置页面

### 常用功能
以往配置host的方式，不能使用端口,只能 `ip pattern` 这样的形式:
```shell
# 普通模式
127.0.0.1 www.test.com
# 组合模式
127.0.0.1 www.test.cn www.test.com.cn
```
但是 whistlek 支持添加端口，同时还支持[路由正则匹配](https://avwo.github.io/whistle/pattern.html)、替换内容、修改请求、编辑内容等强大功能。  
使用方法:
```shell
# 默认模式
127.0.0.1 test.uc.com:7001
# 组合模式
127.0.0.1 www.test.cn www.test.com.cn:7001
# 对域名路径替换host
127.0.0.1:8086 test.uc.com
# 修改返回的状态码
www.uc.com statusCode://500
# 插入HTML(Windows的路径分隔符可以用`\`和`/`)
www.uc.com html://{路径}/test.html
# 插入JS
www.uc.com js://{路径}/test.js
# 插入css
www.uc.com css://{路径}/test.css
# 把内容 append 到请求后面
www.uc.com/style.css resAppend://{cssAppend.css}
# 完全替换请求内容
test.uc.com/style.css resBody://{cssAppend.css}
# 请求改写与接口mock,注意这里的改写是whistle抓包请求的改写，浏览器调试看到的内容仍然是原来的
test.uc.com ua://{test_ua}
# 远程调试，从界面主菜单 “Weinre” 可打开 inspect 界面调试该页面
127.0.0.1 weinre://debug_mypage
```

## 接入mock数据
上面提到 whistle 是可以安装插件的，这里我们使用一个强大的mock插件，首先安装
`npm i -g whistle.vase`
然后在 `plugin` 菜单打开 `vase` 的界面,新建一个名字为 `mock_demo` 的配置，并选择模板为 mock 。再输入下面内容：
```javascript
{
  "list|10": [{
    "name": "@string",
    "id|+1": 10000
  }]
}
```

然后，在 `Rules` 下配置一条 `vase` 的规则即可
```
test.uc.com/test.json vase://mock_demo
```
搭配上 `vase` 的 `script` 模板就可以自己使用js生成数据

**附:**  
[whistle中文文档](http://wproxy.org/whistle/)