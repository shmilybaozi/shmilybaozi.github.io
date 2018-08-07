---
title: Vue中使用 Webpack总结
date: 2018-08-07 15:42:09
tags:
  - Vue
  - Webpack
categories: 
  - Vue
  - Webpack
description:  Vue中使用 Webpack总结
---

Webpack 打包工具(模块打包器)

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

## Webpack结构

Vue中关于 Webpack的配置文件有四个: (vue-cli版本@2.9.6)

- webpack.base.conf.js 主要配置文件
- webpack.dev.conf.js 开发环境配置文件
- webpack.prod.conf.js 生产环境配置文件
- webpack.test.conf.js 需要单元测试时的配置文件

webpack.base.conf.js

```js
module.exports = {
  // 基础目录，绝对路径，用于从配置中解析入口起点(entry point)和 loader
  context: path.resolve(__dirname, '../'),
  // 入口配置: 可以配置多入口
  entry: {
    app: './src/main.js'
  },
  // 出口配置
  output: {
    // 出口路径: path必须是绝对路径
    path: config.build.assetsRoot,
    // 出口文件名
    filename: '[name].js', // [name]即入口名称
    // 
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
    }
  },
  module: {
    rules: [
      ...(config.dev.useEslint ? [createLintingRule()] : []),
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
```