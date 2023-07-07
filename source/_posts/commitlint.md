---
title: commitlint+husky+lint-staged
categories:
  - 代码规范
---

### 为什么要使用

多人开发的时候，commit message五花八门，统一规范有利于及时查找出错的提交代码，从而定位到具体的问题
如何使用

#### ●husky

git hooks工具，可以在执行git命令时，执行自定义脚本程序

```js
npm i -D husky
npx husky install
npm set-script prepare "husky install"
```

//执行完上面的操作之后，项目中会多一个.husky文件夹，并且package.json中增加以下命令

```js
{
  "scripts": {
    "prepare": "husky install"
  }
}
// 现在husky已经安装好了，接下来添加对应的钩子函数，比如我需要在每次git commit提交前执行某些操作，就可以添加一个commit-msg钩子
npx husky add .husky/commit-msg 'npm test'
//然后.husky目录下会增加一个commit-msg文件，每一次git commit 都会执行一词npm test(注：这一行只是测试，后面加了npx husky add .husky/commit-msg 'npx --no -- commitlint --edit $1'需要把这一行注释掉，不然提交代码会报错
//如果遇到特殊情况，要绕过git hooks，可以使用--no-verify
```

#### ●commitLint

检查git commit 内容是否符合规范,只有符合规范的commit命令才能提交

```js
npm i -D @commitlint/{config-conventional,cli}

// 创建一个commitlint.config.js文件
module.exports = { extends: ['@commitlint/config-conventional'] };

// 上面已经引入了husky，所以修改一下commit-msg脚本，在每次git commmit时执行commitlint校验
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit $1'
# or
yarn husky add .husky/commit-msg 'yarn commitlint --edit $1'
```

#### ●lint-staged

对暂存区的文件执行脚本，在提交代码的时候，可以通过eslint+prettier等工具格式化代码，但是如果直接处理全部代码，首先就是可能存在性能问题，其次就是可能会修改掉其他同事的代码，这时可以引入lint-staged，它可以过滤出git 暂存区的代码，这样就不要影响到未更改的文件

```js
npm i lint-staged -D
```

//然后在项目根目录创建.lintstagedrc，配置所需的规则

```
{
  "*.{js,jsx,less,md,json}": [
    "prettier --write"
  ],
  "*.ts?(x)": [
    "prettier --parser=typescript --write",
    "eslint --quiet"
  ]
}
```

最后通过husky来执行lint-staged

```js
npx husky add .husky/pre-commit 'npx lint-staged'
```
