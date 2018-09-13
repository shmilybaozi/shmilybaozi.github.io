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

Vue中 关于Webpack的配置文件有四个: (vue-cli版本@2.9.6)

- build文件夹
  - webpack.base.conf.js 主要配置文件
  - webpack.dev.conf.js 开发环境配置文件
  - webpack.prod.conf.js 生产环境配置文件
  - webpack.test.conf.js 需要单元测试时的配置文件

## webpack.base.conf.js

```js
'use strict'
const path = require('path')
const utils = require('./utils')
const config = require('../config')
const vueLoaderConfig = require('./vue-loader.conf')

function resolve (dir) {
  return path.join(__dirname, '..', dir)
}

const createLintingRule = () => ({
  test: /\.(js|vue)$/,
  loader: 'eslint-loader',
  enforce: 'pre',
  include: [resolve('src'), resolve('test')],
  options: {
    formatter: require('eslint-friendly-formatter'),
    emitWarning: !config.dev.showEslintErrorsInOverlay
  }
})

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
    // 对于按需加载(on-demand-load)或加载外部资源(external resources)（如图片、文件等）来说，output.publicPath 是很重要的选项。
    // 该选项的值是以 runtime(运行时) 或 loader(载入时) 所创建的每个 URL 为前缀。因此，在多数情况下，此选项的值都会以/结束。
    // 可能以下情况中的一种:
    publicPath: 'https://cdn.example.com/assets/', // CDN（总是 HTTPS 协议）
    publicPath: '//cdn.example.com/assets/', // CDN（协议相同）
    publicPath: '/assets/', // 相对于服务(server-relative)
    publicPath: 'assets/', // 相对于 HTML 页面
    publicPath: '../assets/', // 相对于 HTML 页面
    publicPath: '', // 相对于 HTML 页面（目录相同）
    // Vue中: 判断环境变量的值,去对应的环境变量中寻找设置
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath // 默认为"/"，一般会修改为"./"
      : config.dev.assetsPublicPath // 默认为"/"
  },
  // 配置模块如何解析
  resolve: {
    // 自动解析确定的扩展: 能够使用户在引入模块时不带扩展
    extensions: ['.js', '.vue', '.json'],
    // 创建 import 或 require 的别名，来确保模块引入变得更简单。
    alias: {
      'vue$': 'vue/dist/vue.esm.js', // 在给定对象的键后的末尾添加 $,以表示精准匹配: 必须以XXX结尾
      '@': resolve('src'), // js文件中@即表示为src文件夹
    }
  },
  // 决定如何处理项目中的不同类型的模块
  module: {
    // 创建模块时，匹配请求的规则数组。这些规则能够修改模块的创建方式。这些规则能够对模块(module)应用 loader，或者修改解析器(parser)
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