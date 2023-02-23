---
tags:
    - IDE
    - VS Code
---

https://segmentfault.com/a/1190000018777683

## 一、环境

- `Node.js v10.15.3`
- `npm 6.9.0`
- `Visual Studio Code 1.33.0 (user setup)`
- `2019/4/6`
- `Koa2-Node.js QQ群：481973071`

## 二、开发 `TypeScript`

### 1、建立项目目录

使用以下命令创建项目的目录：

```
mkdir ts3
cd ts3
mkdir src
mkdir dist
```

建立好的目录如下：

```
ts3
├─dist
└─src
```

### 2、初始化 `NPM`

在项目的根目录下，执行下面的命令：

```
npm init -y
```

现在项目结构如下：

```
ts3
├─dist
└─src
└─package.json
```

### 3、安装 `TypeScript`

在项目的根目录下，执行下面的命令：

```
npm i -g typescript
```

### 4、创建并配置 `tsconfig.json`

在项目的根目录下，执行下面的命令：

```
tsc --init
```

现在项目结构如下：

```
ts3
├─dist
└─src
└─package.json
└─tsconfig.json
```

在 `tsconfig.json` 中取消下面属性项的注释，并修改其属性的值：

> 这样设置之后，我们在 `./src` 中编码 `.ts` 文件，`.ts` 文件编译成 `.js` 后，输出到 `./dist` 中。

```
"outDir": "./dist",
"rootDir": "./src",
```

### 5、`Hello Typescript`

将下面代码复制到`./src/index.ts`中：

```
const hello: string = 'hello, Alan.Tang';
console.log(hello);
```

在项目的根目录下，执行下面的命令：

> ```
> tsc
> ```
>
>  
>
> 是编译命令，详情查看：
>
> ```
> https://www.tslang.cn/docs/handbook/typescript-in-5-minutes.html
> tsc` 的编译选项，详情查看：`https://www.tslang.cn/docs/handbook/compiler-options.html
> ```

```
tsc
node ./dist/index.js
```

执行结果如下：

```
PS C:\Users\Alan\TestCode\ts3> tsc
PS C:\Users\Alan\TestCode\ts3> node ./dist/index.js
hello, Alan.Tang
```

### 6、使用自动实时编译

> 手动编译还是比较麻烦，如果能够保存代码后，自动编译就好了。
>
> 详情查看：`https://go.microsoft.com/fwlink/?LinkId=733558`

`Ctrl + Shift + B` 运行构建任务，将显示以下选项：

![图片描述](/img-post/开发/IDE/VS Code/正确配置 Visual Studio Code 开发和调试 TypeScript.assets/bVbqW5J.png)

选择 `tsc: 监视 - tsconfig.json` ，回车运行之后，编辑的代码保存之后，就会自动编译。

### 7、简化运行命令

> 每次输入 `node ./dist/index.js` 执行代码，有点麻烦，因为命令太长了。

在命令行界面，按键盘 `↑` 切换历史输入命令，可以快速使用历史输入过的命令。

## 三、代码检查

> 代码检查主要是用来发现代码错误、统一代码风格。
>
> 详情查看：`https://ts.xcatliu.com/engineering/lint.html`

### 1、安装 `ESLint`

`ESLint` 可以安装在当前项目中或全局环境下，因为代码检查是项目的重要组成部分，所以我们一般会将它安装在当前项目中。可以运行下面的脚本来安装：

```
npm install eslint --save-dev
```

由于 `ESLint` 默认使用 [`Espree`](https://github.com/eslint/espree) 进行语法解析，无法识别 `TypeScript` 的一些语法，故我们需要安装 `typescript-eslint-parser`，替代掉默认的解析器，别忘了同时安装 `typescript`：

```
npm install typescript typescript-eslint-parser --save-dev
```

由于 `typescript-eslint-parser` 对一部分 `ESLint` 规则支持性不好，故我们需要安装 `eslint-plugin-typescript`，弥补一些支持性不好的规则。

```
npm install eslint-plugin-typescript --save-dev
```

现在项目结构如下：

```
ts3
├─dist
└─node_modules
└─src
└─package-lock.json
└─package.json
└─tsconfig.json
```

### 2、创建配置文件 `.eslintrc.js`

> ```
> ESLint
> ```
>
>  
>
> 需要一个配置文件来决定对哪些规则进行检查，配置文件的名称一般是
>
>  
>
> ```
> .eslintrc.js
> ```
>
>  
>
> 或
>
>  
>
> ```
> .eslintrc.json
> ```
>
> 。
>
> 当运行 `ESLint` 的时候检查一个文件的时候，它会首先尝试读取该文件的目录下的配置文件，然后再一级一级往上查找，将所找到的配置合并起来，作为当前被检查文件的配置。

在项目的根目录下，执行下面的命令：

> 创建配置文件

```
./node_modules/.bin/eslint --init
```

按需求，选择相应的选项：

```
您想如何使用ESLint?
? How would you like to use ESLint?
To check syntax, find problems, and enforce code style

您的项目使用什么类型的模块?
? What type of modules does your project use?
JavaScript modules (import/export)

您的项目使用哪个框架?
? Which framework does your project use?
None of these

你的代码在哪里运行?(按<space>选择，<a>切换所有，<i>反转选择)
? Where does your code run? (Press <space> to select, <a> to toggle all, <i> to invert selection)
Node

您想如何为您的项目定义一个样式?
? How would you like to define a style for your project?
Use a popular style guide

您想遵循哪种风格指南?
? Which style guide do you want to follow?
Airbnb (https://github.com/airbnb/javascript)

您希望配置文件的格式是什么?
? What format do you want your config file to be in?
JavaScript

Checking peerDependencies of eslint-config-airbnb-base@latest
您所选择的配置需要以下依赖项:
The config that you've selected requires the following dependencies:

eslint-config-airbnb-base@latest eslint@^4.19.1 || ^5.3.0 eslint-plugin-import@^2.14.0
您想现在用npm安装它们吗?
? Would you like to install them now with npm?
Yes

Installing eslint-config-airbnb-base@latest, eslint@^4.19.1 || ^5.3.0, eslint-plugin-import@^2.14.0
npm WARN ts3@1.0.0 No description
npm WARN ts3@1.0.0 No repository field.

+ eslint-config-airbnb-base@13.1.0
+ eslint-plugin-import@2.16.0
+ eslint@5.16.0
added 53 packages from 37 contributors, updated 1 package and audited 286 packages in 10.303s
found 0 vulnerabilities

Successfully created .eslintrc.js file in C:\Users\Alan\TestCode\ts3
```

现在项目结构如下：

```
ts3
├─dist
└─node_modules
└─src
└─.eslintrc.js
└─package-lock.json
└─package.json
└─tsconfig.json
```

编辑 `.eslintrc.js`，增加 `parser: 'typescript-eslint-parser',` 替换掉默认的解析器，使之识别 `TypeScript` 的一些语法，如下面所示：

```
module.exports = {
  parser: 'typescript-eslint-parser',
  env: {
    es6: true,
    node: true,
  },
  extends: 'airbnb-base',
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
  },
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  rules: {
  },
};
```

### 3、在 `VSCode` 中集成 `ESLint` 检查

在编辑器中集成 `ESLint` 检查，可以在开发过程中就发现错误，极大的增加了开发效率。

要在 `VSCode` 中集成 `ESLint` 检查，我们需要先安装 `ESLint` 插件，点击「扩展」按钮，搜索 `ESLint`，然后安装即可。

`VSCode` 中的 `ESLint` 插件默认是不会检查 `.ts` 后缀的，需要在「文件 => 首选项 => 设置」中，添加以下配置：

```
{
  "eslint.validate": [
    "typescript"
  ]
}
```

将下面代码复制到`./src/index.ts`中：

```
let num: number = 1;
if (num == 2) {
  console.log(num);
}
```

现在项目结构如下：

```
ts3
├─dist
└─node_modules
└─src
  └─index.ts
└─.eslintrc.js
└─package-lock.json
└─package.json
└─tsconfig.json
```

现在编辑器，应该会提示 `4` 个错误：

![图片描述](/img-post/开发/IDE/VS Code/正确配置 Visual Studio Code 开发和调试 TypeScript.assets/bVbqW5Y.png)

我们按照错误提示，修改成正确的代码风格：

> `console.log` 一般是在调试阶段使用，发布正式版本时，应该移除。所以这里没有提示红色的致命错误，而是使用了警告。

![图片描述](/img-post/开发/IDE/VS Code/正确配置 Visual Studio Code 开发和调试 TypeScript.assets/bVbqW5S.png)

### 4、无法解析到模块的路径

将下面代码复制到`./src/index.ts`中：

```
import Cat from './Cat';

const kitty: Cat = new Cat('kitty');
kitty.say();
```

将下面代码复制到`./src/Cat.ts`中：

```
export default class Cat {
  private name: string;

  constructor(name: string) {
    this.name = name;
  }

  say() {
    console.log(this.name);
  }
}
```

现在项目结构如下：

```
ts3
├─dist
└─node_modules
└─src
  └─Cat.ts
  └─index.ts
└─.eslintrc.js
└─package-lock.json
└─package.json
└─tsconfig.json
```

上述代码复制粘贴，保存之后，会提示这样的错误：

```
Unable to resolve path to module './Cat'.eslint(import/no-unresolved)
```

解决办法是使用 [`eslint-import-resolver-alias`](https://github.com/johvin/eslint-import-resolver-alias) ，先安装依赖，执行下面的命令：

```
npm install eslint-plugin-import eslint-import-resolver-alias --save-dev
```

然后，在 `.eslintrc.js` 配置中，编辑成如下代码：

```
module.exports = {
  parser: 'typescript-eslint-parser',
  env: {
    browser: true,
    es6: true,
  },
  extends: 'airbnb-base',
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
  },
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  rules: {
  },
  settings: {
    'import/resolver': {
      alias: {
        map: [
          ['@', './src']
        ],
        extensions: ['.ts'],
      },
    },
  },
};
```

## 四、调试 `TypeScript`

> 如何
>
>  
>
> ```
> F5
> ```
>
>  
>
> 开始调试
>
>  
>
> ```
> TypeScript
> ```
>
>  
>
> ，并且还具备断点调试功能，答案是，使用
>
>  
>
> ```
> TS-node
> ```
>
> 。
>
> 详情查看：`https://github.com/TypeStrong/ts-node`

在项目的根目录下，执行下面的命令：

```
npm install -D ts-node
```

在 `VScode` 调试中，添加配置：

```
{
  // 使用 IntelliSense 了解相关属性。 
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "runtimeArgs": [
        "-r",
        "ts-node/register"
      ],
      "args": [
        "${workspaceFolder}/src/index.ts"
      ]
    }
  ]
}
```

![图片描述](/img-post/开发/IDE/VS Code/正确配置 Visual Studio Code 开发和调试 TypeScript.assets/bVbqW5T.png)

按 `F5` 开始愉快的调试吧，`F9` 是添加断点：

![图片描述](/img-post/开发/IDE/VS Code/正确配置 Visual Studio Code 开发和调试 TypeScript.assets/bVbqW5U.png)

## 五、参考文章

1. [`配置 Webpack resolve alias 简化相对路径 import`](https://my.oschina.net/someok/blog/2050469)
2. [`eslint-import-resolver-alias`](https://github.com/johvin/eslint-import-resolver-alias)
3. [`ESLint入门`](https://cn.eslint.org/docs/user-guide/getting-started)
4. [`ts-node`](https://github.com/TypeStrong/ts-node)
5. [`编译选项`](https://www.tslang.cn/docs/handbook/compiler-options.html)
6. [`使用ts-node和vsc来调试TypeScript代码`](https://segmentfault.com/a/1190000010605261)
7. [`通过任务与外部工具集成`](https://code.visualstudio.com/docs/editor/tasks#vscode)
8. [`vscode 调试 TypeScript`](https://segmentfault.com/a/1190000011935122)
9. [`ESLint`](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
10. [`代码检查 · TypeScript 入门教程`](https://ts.xcatliu.com/engineering/lint.html)