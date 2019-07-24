### node反向代理

- 安装express

  ```js
  $ npm install express --save
  $ express projectname
  ```

- 安装中间件

  ```
  $ npm install --save-dev express http-proxy-middleware connect-timeout
  ```

- `App.js`

  ```js
  const express = require('express');
  const timeout = require('connect-timeout');
  const proxy = require('http-proxy-middleware');
  const app = express();
  
  // 这里从环境变量读取配置，方便命令行启动 HOST 指目标地址 PORT 服务端口
  const {
    HOST = 'yourhost',
    PORT = '9526'
  } = process.env;
  
  // 超时时间
  const TIME_OUT = 30 * 1e3;
  
  // 设置端口
  app.set('port', PORT);
  
  // 设置超时 返回超时响应
  app.use(timeout(TIME_OUT));
  app.use((req, res, next) => {
    if (!req.timedout) 
      next();
    }
  );
  
  // 静态页面 这里一般设置你的静态资源路径
  app.use('/', express.static('public'));
  
  // 反向代理（这里把需要进行反代的路径配置到这里即可） eg:将/api 代理到 ${HOST}
  app.use('/api', proxy({
    target: HOST,
    changeOrigin: true,
    pathRewrite: {
      '^/api': '' //url重写
    }
  },));
  // 监听端口
  app.listen(app.get('port'), () => {
    console.log(`server running @${app.get('port')}`);
  });
  ```

- post请求发送失败问题

  下列代码放在proxy设置之后

  ```js
  app.use(express.json());
  app.use(express.urlencoded());
  ```

- 前端代码

  - 正式环境配置

    正式环境需要代理的url 配置为  `api/${url}`

    config/prod.env.js

    ```js
    'use strict'
    module.exports = {
      NODE_ENV: '"production"',
      BASE_API: '"/api"',
    }
    
    ```

  - 开发环境配置

    `config/dev.env.js`

    ```js
    'use strict'
    const merge = require('webpack-merge')
    const prodEnv = require('./prod.env')
    
    module.exports = merge(prodEnv, {
      NODE_ENV: '"development"',
      BASE_API: '"/api"',
    })
    
    ```



    `config/index.js`
    
    ```js
      proxyTable: {
        '/api': {
          target: 'http://0.0.0.0:9526/api', // 接口的域名
          // secure: false,  // 如果是https接口，需要配置这个参数
          changeOrigin: true, // 如果接口跨域，需要进行这个参数配置
          pathRewrite: {
            '^/api': ''
          }
        }
      },
    ```

- 文件位置

  webpack打包完的`dist/static`放入`public`文件夹 ，`dist/index.html`放入`views`文件夹