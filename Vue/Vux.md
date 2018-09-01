### Vux

- 安装

  ```
  npm install --save vux
  ```


- Main.js

  ```js
  import { WechatPlugin } from 'vux'
  import { AjaxPlugin } from 'vux'
  
  Vue.use(WechatPlugin)
  Vue.use(AjaxPlugin)
  ```


- Build/webpack.base.conf.js

  ```js
  // 把module.exports赋值给webpackConfig
  // 引入vuxloader
  const vuxLoader = require('vux-loader')
  
  // resolve 加入less
    resolve: {
      extensions: ['.js', '.vue', '.json'],
      extensions: ['.js', '.vue', '.json','.less'],
    	...
    }
  // 再赋值给module.exports
      module.exports = vuxLoader.merge(webpackConfig, { plugins: ['vux-ui'] })
  ```



  完整文件

  ```js
  'use strict'
  const path = require('path')
  const utils = require('./utils')
  const config = require('../config')
  const vueLoaderConfig = require('./vue-loader.conf')
  
  function resolve (dir) {
    return path.join(__dirname, '..', dir)
  }
  
  
  const vuxLoader = require('vux-loader')
  const webpackConfig = {
    context: path.resolve(__dirname, '../'),
    entry: {
      app: './src/main.js'
    },
    output: {
      path: config.build.assetsRoot,
      filename: '[name].js',
      publicPath: process.env.NODE_ENV === 'production'
        ? config.build.assetsPublicPath
        : config.dev.assetsPublicPath
    },
    resolve: {
      extensions: ['.js', '.vue', '.json','.less'],
      alias: {
        'vue$': 'vue/dist/vue.esm.js',
        '@': resolve('src'),
      }
    },
    module: {
      rules: [
        {
          test: /\.vue$/,
          loader: 'vue-loader',
          options: vueLoaderConfig
        },
        {
          test: /\.js$/,
          loader: 'babel-loader',
          include: [resolve('src'), resolve('test'), resolve('node_modules/webpack-dev-server/client')]
        },
        {
          test: /\.scss$/,
          loaders: ["style", "css", "sass"]
        },
        {
          test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
          loader: 'url-loader',
          options: {
            limit: 10000,
            name: utils.assetsPath('img/[name].[hash:7].[ext]')
          }
        },
        {
          test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
          loader: 'url-loader',
          options: {
            limit: 10000,
            name: utils.assetsPath('media/[name].[hash:7].[ext]')
          }
        },
        {
          test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
          loader: 'url-loader',
          options: {
            limit: 10000,
            name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
          }
        }
      ]
    },
    node: {
      // prevent webpack from injecting useless setImmediate polyfill because Vue
      // source contains it (although only uses it if it's native).
      setImmediate: false,
      // prevent webpack from injecting mocks to Node native modules
      // that does not make sense for the client
      dgram: 'empty',
      fs: 'empty',
      net: 'empty',
      tls: 'empty',
      child_process: 'empty'
    }
  }
  
  module.exports = vuxLoader.merge(webpackConfig, { plugins: ['vux-ui'] })
  ```
