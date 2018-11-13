title: 前端代理实践
speaker: 李建宇
url: https://github.com/ksky521/nodeppt
transition: kontext
files: /js/demo.js,/css/global.css,/img/bg1.png
theme: light

[slide]

# 前端代理实践 {:.text-lg}
## 演讲者：李建宇

[slide]
# Table of Contents  {:.text-super}

* webpack-dev-server  {:.text-lg}
   * wds 介绍 {:.text-md}
   * webpack-dev-server 实践 {:.text-md}
   * webpack-dev-server结合环境变量的开发 {:.text-md}
   
* nginx  {:.text-lg}
   * nginx 在前端代理接口中的应用  {:.text-md}

* 其他代理方式  {:.text-lg}

[slide data-transition="zoomin"]

# webpack-dev-server 介绍  {:.text-lg.vleft}

* 前端工程化
 - webpack(大部分前端开发的选择) 
 - rollup
 - Parcel 

* 前端开发环境服务器的构建-webpack-dev-server
 - 小型Express.js 服务器 
 - 使用webpack-dev-middleware服务webapck的包
 - Sock.js连接服务器

[slide]

# webpack-dev-server 介绍

`webpack --watch`
####  可以实时观察文件的变化，    
      但是要在浏览器看到修改代码后的效果，  
      需要手动刷新浏览器  

`webpack-dev-server`
####  为你提供了一个简单的 web 服务器，     
      并且能够实时重新加载(live reloading)  

[slide data-transition="vertical3d"]

# webpack-dev-server实践  {:&.flexbox.vleft}
## 1、安装
```
npm install webpack-dev-server --save-dev
or
yarn add webpack-dev-server -D
```
## 2、执行命令
```
node_modules/.bin/webpack-dev-server
```

## 3、使用npm，配置package.json
下面是`package.json文件`配置(部分)
```
{
 "name": "fe_proxy_practice"
 "version": "1.0.0",
 "description": "proxy from server in development by wds",
 "scripts": {
    "dev": "webpack-dev-server --hot --inline --host=0.0.0.0 --port=9099 --config webpack.config.js",
    "start": "node_modules/.bin/webpack-dev-server --port=9001",
  },
 "devDependencies": {
    "babel-core": "^6.26.3",
    "babel-loader": "^7.1.4",
    "babel-preset-es2015": "^6.24.1",
    "webpack": "^4.6.0"    
    "webpack-cli": "^2.1.2",
    "webpack-dev-server": "^3.1.3"
  },
}
```

[slide]

# webpack-dev-server 实践  {:&.flexbox.vleft}  
 >  在webpack.config.js中配置代理服务 {:.text-md background="yellow"}  

`webapck.config.js配置`(devServer部分)
```javascript
devServer: {
    port: 9000,
    host: '0.0.0.0',
    historyApiFallback: {
      rewrites: [
        {from: /./, to: '/404.html'}
      ]
    },
    hot: true,
    inline: true,
    // overlay: true,
    //stats: 'errors-only',
    //contentBase: path.join(__dirname, 'dist'),
    disableHostCheck: true,
   
    proxy: { // proxy URLs to backend development server
       '/user_info': 'http://localhost:8099',
      
       '/api': {
          target: 'http://test.linkdoc.com:8886',
          changeOrigin: true,// Set the option changeOrigin to true for name-based virtual hosted sites
          autoRewrite: true,//rewrites the location host/port on (301/302/307/308) redirects based on requested host/port. Default: false.
          secure: true,// true/false, if you want to verify the SSL Certs
          protocolRewrite: null,//rewrites the location protocol on (301/302/307/308) redirects to 'http' or 'https'. Default: null.
          auth: '',//Basic authentication i.e. 'user:password' to compute an Authorization header.
          headers: {host: 'test.linkdoc.com', 'Content-Type': 'application/x-www-form-urlencoded'}, //add request headers
          ws: true // true/false:if you want to proxy websockets
        },
    }
 }
```

[slide]
# 环境变量的应用 
在`package.json`配置环境变量 {:.vleft}

```
{
  ”name”: “linkdoc-fe”,
  "scripts": {
      "dev": "webpack-dev-server --hot --inline --host=0.0.0.0 --port=8106 --config build/webpack.client.config.js",
      "dist": "rimraf dist && npm run build",
      "build": "rimraf dist/static && cross-env NODE_ENV=production webpack --config build/webpack.client.config.js “,
      "testing": "rimraf dist/static && cross-env NODE_ENV=testing webpack --config build/webpack.client.config.js "
  },
   ….
},
```

 在`webpack.config.js`中配置环境变量

```
const env = process.env.NODE_ENV || ‘development’;
注：从package.json命令中读取当前NODE_ENV的值
const isProd = env === 'production';
const isTest = env === ‘testing’;
```

根据环境变量做其他操作
```
1、 配置登录地址
const UC_BASE_URL = process.env.UC_BASE_URL || (
  isProd 
  ? '//passport.linkdoc.com' 
  :  ( isTest ? 'https://test.linkdoc.com:9111' : ‘开发环境下用户登录的URL所在地址 http://192.168.1.213:9000’ )
);
new webpack.DefinePlugin({
  'process.env.NODE_ENV': JSON.stringify(env),
  'process.env.UC_BASE_URL': JSON.stringify(UC_BASE_URL),
})
```
[slide]

# 环境变量的应用
根据环境变量做其他操作

```
2、配置接口代理地址

 let proxyTargetForRc = process.env['FE_DEV_PROXY_RC'] || 'http://192.168.1.213:9110';
devServer = {
  disableHostCheck: true,
  proxy: {
    '/api/rc/': {
      target: proxyTargetForRc,
      changeOrigin: true,
      autoRewrite: true,
      secure: false,
    },
}

上述两种操作都可以在启动环境的时候方便的进行更改
例： 代理后端到后端同学的机器192.168.1.101:9111
控制台执行命令 UC_BASE_URL=‘http://192.168.1.102:8085'  FE_DEV_PROXY_RC='http://192.168.1.102:8085'  npm run dev
```



[slide]

# Nginx 在前端代理中的使用介绍
## 1、代理接口
   * `nginx.conf`文件的配置（部分）

```
http {
   ...
   server {
      listen 9001
      location /back/ {
            proxy_pass http://homesite213.dev.linkdoc.com;
            proxy_set_header Host homesite213.dev.linkdoc.com;
        }
       
      #add_header Access-Control-Allow-Origin: "*"
   }
}
```

## 2、代理路由 {:.vleft}

```nginx.conf
http {
   ...
   server {
      listen       8110;
      access_log logs/www.access.log;
      error_log logs/www.error.log info;
 
      location / {
            proxy_pass http://loc.linkdoc.com:8109;
            proxy_set_header Host $host;
            index  index.html index.htm;
        }
   }
}
```

[slide]
## 其他方式
1. CORS（Cross-Origin Resource Sharing) <br> 这是现代浏览器支持跨域资源请求的一种方式，当使用XMLHttpRequest发送请求时，浏览器若发现请求不符合同源策略，会加Origin请求头，对应
后台在返回结果中加上响应头Access-Control-Allow-Origin:"*"
2. JSONP <br> 利用浏览器对script对资源饮用没有同源限制对原理（前后端约定callback函数名）
<br>资源加载会立即执行, 无法发送post请求，确定请求是否失败困难，大多许框架配合超时时间来判断
3. XDomainRequest(IE8\9，协议不同https发送http请求，不支持)
[slide]

## 总结:   {:.text-lg &.flexbox.vleft}
----

 使用webpack-dev-server和nginx辅助开发，可以极大的方便前端调试。特别是在前后端分离的场景下，使前端可以更加灵活的构建自己的开发环境。   {:.text-md &.flexbox.vleft}

[slide style="background-image:url('/img/bg1.png')"]

## Thks! {:.text-lg.text-success}
