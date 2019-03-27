---
title: webpack 配置
tags:
  - webpack
categories:
  - webpack
comments: true
---

# webpack 配置学习

## 一. 入口[entry]

1. 介绍
   - 指示 webpack 应该使用哪个模块，来作为构建内部*依赖图*的开始
   - 每个依赖项都被处理，最后输出到 _bundles_ 文件中
   - 入口可指定一个或多个，默认值为 `./src`
2. 配置
   - 单个入口
   - 多个入口
     <!-- more -->

```javascript
module.exports = {
  entry: './src/index.js'
};
```

```javascript
module.exports = {
  entry: {
    index: './src/index.js',
    vendors: './src/vendors.js'
  }
};
```

## 二. 出口[output]

1. 介绍
   - 告诉 wepback 在哪里输出所创建的 _bundles_ ，以及如何命名这些文件，默认值为 `./dist`
   - 整个应用程序结构，都会被编译到所指定的输出路径的文件夹中
   - `output.filename` 属性配置输出文件名，`output.path` 属性配置输出文件路径
2. 配置
   - 单个入口对应出口配置
   - 多个入口对应出口配置（可配置 hash，避免缓存；publicPath 可配置静态资源 CDN 或者服务器访问地址，也可不配置）

```javascript
module.exports = {
  output: {
    filename: 'bundle.js',
    path: '/home/proj/public/assets'
  }
};
```

```javascript
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].[hash].js',
    path: path.resolve(__dirname, './dist'),
    publicPath: ''
  }
};

// 写入到硬盘：./dist/app.js, ./dist/search.js
```

## 三. loader

1. 介绍
   - `loader` 让 webpack 能够去处理那些非 javascript 文件
   - 定义在 `module.rules` 中
   - `test` 属性，设置应该被对应的 loader 进行转换的某个或某些文件
   - `use` 属性，设置对应的 loader

## 四. 插件[plugins]

1. 介绍
   - 插件功能相当强大
   - 可解决打包优化和压缩优化等
   - 定义在 `plugins` 中
   - 在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例

## 五. 模式

1. 介绍
   - `mode` 有 `development` 和 `production` 两种模式，分别用于开发环境和生产环境
   - 可设置对应模式下的相关 webpack 配置
2. 使用

   - webpack 配置中使用 `mode`
   - 可从 CLI 参数中传递

3. 注意
   - development: 会将 `process.env.NODE_ENV` 的值设为 `development`；并启用 `NamedChunksPlugin` 和 `NamedModulesPlugin`。
   - production: 会将 `process.env.NODE_ENV` 的值设为 `production`； 并启用 `FlagDependencyUsagePlugin` , `FlagIncludedChunksPlugin` , `ModuleConcatenationPlugin` , `NoEmitOnErrorsPlugin` , `OccurrenceOrderPlugin` , `SideEffectsFlagPlugin` , `UglifyJsPlugin` 。
   - 只设置 `NODE_ENV` ，则不会自动设置 `mode` 。

```javascript
module.exports = {
  mode: 'production' // development production
};
```

```javascript
webpack --mode=production
```
