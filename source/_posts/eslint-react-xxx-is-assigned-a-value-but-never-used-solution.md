---
title: eslint react xxx is assigned a value but never used 的解决办法
date: 2016-12-11 00:09:15
tags: [react,eslint]
categories: [react]
---

# ESLint中文翻译
 
- [ESLint](http://eslint.cn/)

# 问题重现(定义了变量并使用了，但是eslint还是报定义过了但是未使用的错误)

```
import React from 'react';
import ReactDOM from 'react-dom';

let Form = React.createClass({
	render: () => { < div className = "form" > < div className = "form-title" > Simple Login Form < /div>
              <div className="name-div"><label htmlFor="username" className="name-label">Name:</label > <input id="username" className="name-input" name="username" type="text"/> < /div>
              <div className="pwd-div"><label htmlFor="password" className="pwd-label">Password:</label > <input id="password" className="pwd-input" name="password" type="password"/> < /div>
        </div >;
	}
});

ReactDOM.render(<Form />,document.getElementById('loginForm'));

```

然后atom 的eslint报了`no-unused-vars 'Form' is assigned a value but never used.at line 4 col 5`的错误。

# 比较麻烦的解决办法--每一行报错的地方都加一行注释

在声明变量的当前行加上一条注释 `// eslint-disable-line no-unused-vars`

即是：
```
import React from 'react';
import ReactDOM from 'react-dom';

let Form = React.createClass({ // eslint-disable-line no-unused-vars
	render: () => { < div className = "form" > < div className = "form-title" > Simple Login Form < /div>
              <div className="name-div"><label htmlFor="username" className="name-label">Name:</label > <input id="username" className="name-input" name="username" type="text"/> < /div>
              <div className="pwd-div"><label htmlFor="password" className="pwd-label">Password:</label > <input id="password" className="pwd-input" name="password" type="password"/> < /div>
        </div >;
	}
});

ReactDOM.render(<Form />,document.getElementById('loginForm'));

```

# 比较暴力的解决方法--直接禁用变量声明但未使用的提示,但是也比较省心，不用加一堆注释

设置.eslint.json(或者是yaml等)的"no-unused-vars"规则为禁用(0为禁用)。具体可以看[这里](http://eslint.cn/docs/rules/no-unused-vars)
```
{
  "rules":{
    "no-unused-vars":0
  }
}
```
