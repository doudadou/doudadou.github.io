title: 使用webpack搭建ReactJS环境安装流程
date: 2015-06-10 10:04:07
categories: code
tags: [Webpack,ReactJs]
photos:
- https://camo.githubusercontent.com/ebc085019011ababb0d35024824304831c7dc72a/68747470733a2f2f7765627061636b2e6769746875622e696f2f6173736574732f6c6f676f2e706e67
---
webpack搭建ReactJS环境首先看下项目模型
# 项目模型
- /app
	- main.js
	- TestOne.js
	- TestTwo.js
- /build
	- bundle.js(会通过webpack自动生成)
	- index.html
- package.json
- webpack.config.js

# 创建项目
```bash
mkdir app build && touch webpack.config.js app/main.js app/TestOne.js app/TestTwo.js build/index.html
```

## main.js

```javascript
/* main.js */

'use strict';

var React = require('react');
var TestOne = require('./TestOne.js');
var TestTwo = require('./TestTwo.js');

var Main = React.createClass({
    getInitialState: function() {
        return {
          switch: true
        };
    },
    _toggle() {
        this.setState({
            switch: !this.state.switch
        });
    },
    render() {
        return (
            <div>
                <input type="button" onClick={this._toggle} value="Press Me!"/>
                {this.state.switch ? <TestOne /> : <TestTwo />}
            </div>      
        );
    }
});

React.render(<Main />, document.body);
```
<!-- more -->
## TestOne.js

```javascript
/* TestOne.js */

'use strict';

var React = require('react');

var TestOne = React.createClass({
    render() {
        return (
            /* jshint ignore: start*/
            <div>Hello I am TestOne Component</div>
            /* jshint ignore: end*/
        );
    }
});

module.exports = TestOne;
```
## TestTwo.js

```javascript
/* TestTwo.js */

'use strict';

var React = require('react');

var TestTwo = React.createClass({
    render() {
        return (
            /* jshint ignore: start*/
            <h1>Hello I am TestTwo Component</h1>
            /* jshint ignore: end*/
        );
    }
});

module.exports = TestTwo;
```
# 环境搭建
```bash
npm install webpack webpack-dev-server react-hot-loader babel-loader --save-dev
npm install --save style-loader css-loader less-loader autoprefixer-loader
```

使用
```bash
npm init  // 创建 package.json
```
首先，先到 `package.json` 內加入一個新 `dev` 的指令:
```javascript
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
}
```
接著更新 `index.html`:
```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8" />
</head>

<body>
    <script src="http://localhost:8080/webpack-dev-server.js"></script>
    <script src="bundle.js"></script>
</body>

</html>
```
编辑`webpack.config.js`为
```javascript
var webpack = require('webpack');
var path = require('path');

var config = {
    entry: ['webpack-dev-server/client?http://localhost:8080',
		    'webpack/hot/only-dev-server',
		    path.resolve(__dirname, 'app/main.js')],
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js'
    },
    module: {
	        loaders: [{
		        test: /\.js$/,
		        loaders: ['react-hot', 'babel'],
		        include: path.join(__dirname, 'app')
		    },{
			    test: /\.less$/,
				loader: 'style-loader!css-loader!autoprefixer-loader!less-loader'
			}
	    ]
    },
    plugins: [
	    //new webpack.HotModuleReplacementPlugin(),
	    new webpack.NoErrorsPlugin()
	]
};

module.exports = config;
```
运行
```bash
npm run dev
```
访问 http://localhost:8080/webpack-dev-server/ 尝试是否跑起来
