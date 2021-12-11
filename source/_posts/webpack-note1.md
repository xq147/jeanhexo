---
title: webpack学习笔记
date: 2021-12-09 12:06:23
tags:
---

<!--webpack学习笔记-->


## DefinePlugin
- 允许创建一个在编译时可以配置的全局常量

## WebpackShellPlugin(webpack构建之前或之后运行)

## MiniCssExtractPlugin(css提取)
- 将CSS提取为独立的文件的插件，对每个包含css的js文件都会创建一个CSS文件，支持按需加载css和sourceMap
- 仅用于webpack4,暂不支持HMR
![可参考](https://www.jianshu.com/p/91e60af11cc9)

## copy-webpack-plugin(拷贝插件)
- copy-webpack-plugin 并非旨在复制从构建过程中生成的文件，而是在构建过程中复制源树中已经存在的文件。
- 如需要webpack-dev-server在开发过程中将文件写入输出目录，则可以使用writeToDisk选项或强制执行write-file-webpack-plugin
- ![文档地址](https://www.npmjs.com/package/copy-webpack-plugin)

```typescript

const CopyPlugin = require('copy-webpack-plugin');
module.exports = {
  plugins: [
    new CopyPlugin({
      patterns: [
        {
          from: 'manifest.json', // 从哪里拷贝
          to: 'manifest.json', // 被拷贝的地址
          globOptions: { // 允许配置插件使用的全局模式匹配库，查看支持的选项列表要从选择中排除文件，应使用globOptions.ignore选项
              ignore: ['.*']
          },
          transform: function () { // 允许修改文件内容。启用转换缓存 Allows to modify the file contents. Enable transform caching
            return {};
          }
        },
      ],
    }),
  ],
};

```

## write-file-webpack-plugin()