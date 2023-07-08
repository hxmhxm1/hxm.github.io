---
title: webpack
categories:
  - 打包工具
---

### 一、主要概念

#### ●5大核心概念

```js
module.exports = {
  // 入口
  entry: '',
  // 输出
  output: {},
  // 加载器loader
  module: {
    rules: [],
  },
  // 插件
  plugins: [],
  // 模式
  mode: '', // development|production
};
```

#### ●生产模式和开发模式

○开发模式
（1）编译代码，使浏览器能识别运行
开发时我们有样式资源、字体图标、图片资源、html 资源等，webpack 默认都不能处理这些资源，所以我们要加载配置来编译这些资源
（2）代码质量检查，树立代码规范
提前检查代码的一些隐患，让代码运行时能更加健壮。提前检查代码规范和格式，统一团队编码风格，让代码更优雅美观。

○生产模式
生产模式是开发完成代码后，我们需要得到代码将来部署上线。这个模式下我们主要 对代码进行优化，让其运行性能更好。优化主要从两个角度出发:
1）优化代码运行性能（2）优化代码打包速度

#### ●处理样式资源

1）Webpack 本身是不能识别样式资源的，所以我们需要借助 Loader 来帮助 Webpack 解析样式资源

```js
// css-loader: 负责将css文件编译成webpack能识别的模块
// style-loader: 会动态创建一个style标签，里面放置webpack中css模块内容
const path = require('path');

module.exports = {
  entry: '',
  output: {},
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.less$/,
        use: ['style-loader', 'css-loader', 'less-loader'],
      },
      {
        test: /\.s[ac]ss$/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
      {
        test: /\.styl$/,
        use: ['style-loader', 'css-loader', 'stylus-loader'],
      },
    ],
  },
  plugins: [],
  mode: 'development',
};
```

2）提取css成单独的文件
css文件目前被打包到js文件中，当js文件加载时，会创建一个style标签来生成样式，这对于网站来说，会造成闪屏现象，用户体验不好，我们应该是单独的css文件，通过link标签加载性能才好

```js
npm i mini-css-extract-plugin -D
const MiniCssExtractPlugin =require("mini-css-extract-plugin");
// 将style-loader替换成 MiniCssExtractPlugin.loader
rules: [
 {
   // 用来匹配 .css 结尾的文件
   test: /\.css$/,
   // use 数组里面 Loader 执行顺序是从右到左
   use: [MiniCssExtractPlugin.loader, "css-loader"],
 }]

3）css兼容性处理
npm i postcss-loader postcss postcss-preset-env -D
rules: [
 {
   // 用来匹配 .css 结尾的文件
   test: /\.css$/,
   // use 数组里面 Loader 执行顺序是从右到左
   use: [
     MiniCssExtractPlugin.loader,
     "css-loader",
     {
       loader: "postcss-loader",
       options: {
         postcssOptions: {
           plugins: [
             "postcss-preset-env", // 能解决大多数样式兼容性问题
           ],
         },
       },
     },
   ],

 在package.json中可以更改兼容性
{
 // 其他省略
 "browserslist": ["ie >= 8"]
}
```

4）压缩css

```js
npm i css-minimizer-webpack-plugin -D
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
plugins: [
      // css压缩
    new CssMinimizerPlugin(),
  ],
```

5)合并配置处理

```js
const getStyleLoaders = (preProcessor) => {
  return [
    MiniCssExtractPlugin.loader,
    'css-loader',
    {
      loader: 'postcss-loader',
      options: {
        postcssOptions: {
          plugins: [
            'postcss-preset-env', // 能解决大多数样式兼容性问题
          ],
        },
      },
    },
    preProcessor,
  ].filter(Boolean);
};
// 下面是用法
rules: [
  {
    // 用来匹配 .css 结尾的文件
    test: /\.css$/,
    // use 数组里面 Loader 执行顺序是从右到左
    use: getStyleLoaders(),
  },
  {
    test: /\.less$/,
    use: getStyleLoaders('less-loader'),
  },
];
```

#### ●处理图片资源

```js
// webpack4中，处理图片要通过file-loader和url-loader进行处理，但是webpack5已经将两个loader功能内置到webpack里了，我们只需要简单配置即可处理图片资源
rules: [
{
    test: /\.(png|jpe?g|gif|webp)$/,
    type: "asset",
}],

// 优化
{
    test: /\.(png|jpe?g|gif|webp)$/,
    type: "asset",
    //将小于某个大小的图片转化成 data URI 形式（Base64 格式， 优点是能减    少请求次数，缺点是会导致体积变大
    parser: {
      dataUrlCondition: {
        maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
      },
    },
// 修改资源输入路径
generator: {
  // 将图片文件输出到 static/imgs 目录中
  // 将图片文件命名 [hash:8][ext][query]
  // [hash:8]: hash值取8位
  // [ext]: 使用之前的文件扩展名
  // [query]: 添加之前的query参数
  filename: "static/imgs/[hash:8][ext][query]",
}},
```

#### ●处理js资源

```js
1)针对 js 兼容性处理，我们使用 Babel 来完成
// 此处eslint具体配置省略，只写webpack配置
const ESLintWebpackPlugin = require("eslint-webpack-plugin");
 plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "src"),
    }),
  ],
```

2）针对代码格式，我们使用 Eslint 来完成

```js
npm i babel-loader @babel/core @babel/preset-env -D
// babel.config.js
module.exports = {
  presets: ["@babel/preset-env"],
};
```

#### ●处理html资源

```js
npm i html-webpack-plugin -D
const HtmlWebpackPlugin = require("html-webpack-plugin");
 plugins: [
    new HtmlWebpackPlugin({
      // 以 public/index.html 为模板创建文件
      // 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源
      template: path.resolve(__dirname, "public/index.html"),
    }),
  ],
```

### 二、优化

#### ●提升开发体验

（1）sourceMap方便出现问题时定位到具体代码

```js
module.exports = {
  // 其他省略
  mode: 'production',
  devtool: 'source-map',
};

module.exports = {
  // 其他省略
  mode: 'development',
  devtool: 'cheap-module-source-map',
};
```

#### ●提升打包构建速度

（1）oneOf

```js
module: {
    rules: [
      {
    oneOf: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        include: XXXX,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      }]
```

（2） include exclude
（3）hotMdouleReplacement（HMR）
（4）cache

```js
// 每次js打包都要经过eslint和babel编译，速度比较慢，可以缓存之前的eslint检查和babel编译结果，这样二次打包速度会快很多
plugins: [
  new ESLintWebpackPlugin({
    // 指定检查文件的根目录
    context: path.resolve(__dirname, '../src'),
    exclude: 'node_modules', // 默认值
    cache: true, // 开启缓存
    // 缓存目录
    cacheLocation: path.resolve(
      __dirname,
      '../node_modules/.cache/.eslintcache'
    ),
  }),
];
```

（5）Thead多核，多进程

#### ●减少代码体积

（1）tree shaking
（2）Image Minimizer
当项目中引入图片过多时（针对本地文件）可以进行压缩

```js
npm i image-minimizer-webpack-plugin imagemin -D
// 无损压缩
npm install imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemin-svgo -D
// 有损压缩
npm install imagemin-gifsicle imagemin-mozjpeg imagemin-pngquant imagemin-svgo -D

const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");
optimization: {
    minimizer: [
      // css压缩也可以写到optimization.minimizer里面，效果一样的
      new CssMinimizerPlugin(),
      // 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了
      new TerserPlugin({
        parallel: threads, // 开启多进程
      }),
      // 压缩图片
      new ImageMinimizerPlugin({
        minimizer: {
          implementation: ImageMinimizerPlugin.imageminGenerate,
          options: {
            plugins: [
              ["gifsicle", { interlaced: true }],
              ["jpegtran", { progressive: true }],
              ["optipng", { optimizationLevel: 5 }],
              [
                "svgo",
                {
                  plugins: [
                    "preset-default",
                    "prefixIds",
                    {
                      name: "sortAttrs",
                      params: {
                        xmlnsOrder: "alphabetical",
                      },
                    },
                  ],
                },
              ],
            ],
          },
        },
      }),
    ],
  },
```

#### ●优化代码运行性能

（1）code split
●分割文件：将打包生成的文件进行分割，生产多个js文件
●按需加载，需要哪个文件就加载哪个

```js
optimization: {
    // 代码分割配置
    splitChunks: {
      chunks: "all", // 对所有模块都进行分割
      }
}
```
