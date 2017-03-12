---
title: express和webpack配合时热刷新出错的解决方法
date: 2017-01-17 23:44:29
tags: [javascript,nodejs]
catogory: [nodejs]
---

# 出错

```
[WDS] App updated. Recompiling...
bundle.js:635 [WDS] Disconnected!
log @ bundle.js:635
bundle.js:631 [WDS] Hot Module Replacement enabled.
bundle.js:631 [WDS] App hot update...
bundle.js:8453 [HMR] Checking for updates on the server...
```

# 原因

**不能使用nodemon或者supervisor去启动webpack-dev-server**

之前因为把express服务和webpack-dev-server在同一个server.js里启动，然后执行`nodemon server.js`，然后发现出错信息。

折腾了好久，google、百度、问人、还去github提issue，最后还是靠自己解决了。

具体原因是因为**nodemon**是重启整个进程的，如果用**nodemon**来启动**webpack**，那么这样的话会使webpack丢失某些存在内存中的东西，所以不建议使用**nodemon**来启动**webpack**。

详细可以看webpack-hot-middleware的[issue](https://github.com/glenjamin/webpack-hot-middleware/issues/21)

# 解决方法

1. 把express server和webpack-dev-server分开

server.js(express server)

```
const proxy = require('proxy-middleware')
const url = require('url')
const express = require('express')
const app = express()

app.get('/',function(req,res){
  res.sendFile(__dirname+'/src/html/index.html');
})
app.listen(3001)
app.use('/build',proxy(url.parse('http://localhost:3000/build')))
```

webpack.server.js

```
const webpack = require('webpack')
const WebpackDevServer = require('webpack-dev-server')
const config = require('./webpack.config')
const compiler = webpack(config)

new WebpackDevServer(compiler, {
  publicPath: config.output.publicPath,
  inline:true,
  hot: true,
  historyApiFallback: true
}).listen(3000, 'localhost', function (err, result) {
  if (err) {
    return console.log(err);
  }
  console.log('Listening at http://localhost:3000/')
});
```

2. 分别用nodemon和node启动

终端1

`nodemon server.js`

终端2

`node webpack-dev-server`
