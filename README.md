# webpack5 手动配置vue3的开发环境



## 01.安装webpack

首先初始化 `package.json`

```powershell
npm init -y
```

安装webpack和webpack-cli依赖

```powershell
npm install webpack webpack-cli -save-dev
```

在项目根目录创建webpack的配置文件`webpack.config.js`

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].js'
    }
}
```

创建src目录，index.js文件

```javascript
// src/index.js
console.log('test webpack')
```

在package.json的scripts字段，添加命令

```json
{
  // ...
  "scripts": {
    "build": "webpack",
  },
  // ....
}
```

此时执行`npm run build`即可打包



## 02.基本功能配置





### 1.配置typescript支持

一些浏览器无法解析es6以上的高级语法,另外都2021年了,vue3是支持typescript的,所以我们也要配置typescript支持.

另外早在2020年.babel就支持了typescript的转换,所以tsloader和babel里面选择一个安装就可以了，

因为tsloader相比于babel的问题是兼容性只到es5，不过一般情况下也够用了。

ts-loader有typescript的静态代码检查功能，因为用typescript比较合适，虽然开了这个功能会降低编译速度

另外babel和ts-loader可以一起配置，所以以后有需要也可以添加



安装typescript和ts-loader

```powershell
npm install typescript ts-loader --save-dev
```

修改webpack配置,匹配ts文件给ts-loader解析

```js
module.exports = {
    // ...
    module: {
        rules: [
            {
                test: /\.tsx?$/,
                use: [
                    'ts-loader'
                ]
            },
            // ...
        ]
    },
    // ...
}
```

初始化tsconfig.json文件

```powershell
npx tsc --init
```

注意再tsconfig 里面配置好你的输入输出目录

我这里因为是生成单文件，所以我们用比较新的system.js模块输出，

```json
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Basic Options */
    // "incremental": true,                         /* Enable incremental compilation */
    "target": "es5",                                /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', 'ES2021', or 'ESNEXT'. */
    "module": "system",                           /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    // "lib": [],                                   /* Specify library files to be included in the compilation. */
    // "allowJs": true,                             /* Allow javascript files to be compiled. */
    // "checkJs": true,                             /* Report errors in .js files. */
    // "jsx": "preserve",                           /* Specify JSX code generation: 'preserve', 'react-native', 'react', 'react-jsx' or 'react-jsxdev'. */
    // "declaration": true,                         /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                      /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                           /* Generates corresponding '.map' file. */
    "outFile": "dist/out.js",                             /* Concatenate and emit output to single file. */
    // "outDir": "",                              /* Redirect output structure to the directory. */
    "rootDir": "src",                             /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                           /* Enable project compilation */
    // "tsBuildInfoFile": "./",                     /* Specify file to store incremental compilation information */
    // "removeComments": true,                      /* Do not emit comments to output. */
    // "noEmit": true,                              /* Do not emit outputs. */
    // "importHelpers": true,                       /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,                  /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,                     /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                                 /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                       /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,                    /* Enable strict null checks. */
    // "strictFunctionTypes": true,                 /* Enable strict checking of function types. */
    // "strictBindCallApply": true,                 /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,        /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                      /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                        /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                      /* Report errors on unused locals. */
    // "noUnusedParameters": true,                  /* Report errors on unused parameters. */
    // "noImplicitReturns": true,                   /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,          /* Report errors for fallthrough cases in switch statement. */
    // "noUncheckedIndexedAccess": true,            /* Include 'undefined' in index signature results */
    // "noImplicitOverride": true,                  /* Ensure overriding members in derived classes are marked with an 'override' modifier. */
    // "noPropertyAccessFromIndexSignature": true,  /* Require undeclared properties from index signatures to use element accesses. */

    /* Module Resolution Options */
    // "moduleResolution": "node",                  /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                             /* Base directory to resolve non-absolute module names. */
    // "paths": {},                                 /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                              /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                             /* List of folders to include type definitions from. */
    // "types": [],                                 /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,        /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                        /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,                    /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,                /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                            /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                               /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                     /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                       /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,              /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,               /* Enables experimental support for emitting type metadata for decorators. */

    /* Advanced Options */
    "skipLibCheck": true,                           /* Skip type checking of declaration files. */
    "forceConsistentCasingInFileNames": true        /* Disallow inconsistently-cased references to the same file. */
  }
}
```

#### 添加vue支持

```powershell
npm install vue@next -S
npm install vue-loader@next @vue/compiler-sfc -D
```

**注意**：`Vue2.x` 时安装的是 `vue-template-complier`

详见官网的说明https://v3.cn.vuejs.org/guide/installation.html#npm

修改webpack配置

```js
const { VueLoaderPlugin } = require('vue-loader/dist/index');

module.exports = {
    // ...
    module: {
        rules: [
            {
                test: /\.vue$/,
                use: [
                    'vue-loader'
                ]
            }
        ]
    },
    plugins: [
        new VueLoaderPlugin()
    ]
}
```

然后从main.ts引入vue

```javascript
// index.js
import { createApp } from 'vue';
import App from './App.vue';

createApp(App).mount('#app')
```

注意这里会报两个错误,

- 一个是提醒你module resolution 需要设置成node，也就是设置模块解析策略用node的
- 还有一个错误是引入App.vue的报错，因为没有APP.vue的ts类型定义
- ts标签报错

第一个我们可以直接抄一份vue-cli的tsconfig过来里面就正常设置了。

主要问题是我目前的需求是编译成单文件所以这个配置还需要改一下。

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "esnext",
    "strict": true,
    "jsx": "preserve",
    "importHelpers": true,
    "moduleResolution": "node",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "sourceMap": true,
    "baseUrl": ".",
    "types": [
      "webpack-env"
    ],
    "paths": {
      "@/*": [
        "src/*"
      ]
    },
    "lib": [
      "esnext",
      "dom",
      "dom.iterable",
      "scripthost"
    ]
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

我改一下就是下面的

```json
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */
    /* Basic Options */
    // "incremental": true,                         /* Enable incremental compilation */
    "target": "es5" /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', 'ES2021', or 'ESNEXT'. */,
    "module": "system" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
    "lib": [
      "esnext",
      "dom",
      "dom.iterable",
      "scripthost"
    ] /* Specify library files to be included in the compilation. */,
    // "allowJs": true,                             /* Allow javascript files to be compiled. */
    // "checkJs": true,                             /* Report errors in .js files. */
    "jsx": "preserve" /* Specify JSX code generation: 'preserve', 'react-native', 'react', 'react-jsx' or 'react-jsxdev'. */,
    // "declaration": true,                         /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                      /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                           /* Generates corresponding '.map' file. */
    "outFile": "dist/out.js" /* Concatenate and emit output to single file. */,
    // "outDir": "",                              /* Redirect output structure to the directory. */
    "rootDir": "src" /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */,
    // "composite": true,                           /* Enable project compilation */
    // "tsBuildInfoFile": "./",                     /* Specify file to store incremental compilation information */
    // "removeComments": true,                      /* Do not emit comments to output. */
    // "noEmit": true,                              /* Do not emit outputs. */
    "importHelpers": true /* Import emit helpers from 'tslib'. */,
    // "downlevelIteration": true,                  /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,                     /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */
    /* Strict Type-Checking Options */
    "strict": true /* Enable all strict type-checking options. */,
    // "noImplicitAny": true,                       /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,                    /* Enable strict null checks. */
    // "strictFunctionTypes": true,                 /* Enable strict checking of function types. */
    // "strictBindCallApply": true,                 /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,        /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                      /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                        /* Parse in strict mode and emit "use strict" for each source file. */
    /* Additional Checks */
    // "noUnusedLocals": true,                      /* Report errors on unused locals. */
    // "noUnusedParameters": true,                  /* Report errors on unused parameters. */
    // "noImplicitReturns": true,                   /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,          /* Report errors for fallthrough cases in switch statement. */
    // "noUncheckedIndexedAccess": true,            /* Include 'undefined' in index signature results */
    // "noImplicitOverride": true,                  /* Ensure overriding members in derived classes are marked with an 'override' modifier. */
    // "noPropertyAccessFromIndexSignature": true,  /* Require undeclared properties from index signatures to use element accesses. */
    /* Module Resolution Options */
    "moduleResolution": "node" /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */,
    "baseUrl": "." /* Base directory to resolve non-absolute module names. */,
    "paths": {
      "@/*": ["src/"]
    } /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */,
    // "rootDirs": [],                              /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                             /* List of folders to include type definitions from. */
    // "types": [
    //   "webpack-env"
    // ] /* Type declaration files to be included in compilation. */,
    "allowSyntheticDefaultImports": true /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */,
    "esModuleInterop": true /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */,
    // "preserveSymlinks": true,                    /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,                /* Allow accessing UMD globals from modules. */
    /* Source Map Options */
    // "sourceRoot": "",                            /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                               /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                     /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                       /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */
    /* Experimental Options */
    // "experimentalDecorators": true,              /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,               /* Enables experimental support for emitting type metadata for decorators. */
    /* Advanced Options */
    "skipLibCheck": true /* Skip type checking of declaration files. */,
    "forceConsistentCasingInFileNames": true /* Disallow inconsistently-cased references to the same file. */
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  "exclude": ["node_modules"]
}
```

关于Vue文件的定义问题，从vue-cli创建的定义文件，复制一份过来就行了

```javascript
/* eslint-disable */
declare module '*.vue' {
  import type { DefineComponent } from 'vue'
  const component: DefineComponent<{}, {}, any>
  export default component
}
```

关于ts标签报错,需要在ts-loader里进行配置,让他解析vue文件`appendTsSuffixTo: [/\.vue$/]`

```
module.exports = {
  entry: "./index.vue",
  output: { filename: "bundle.js" },
  resolve: {
    extensions: [".ts", ".vue"]
  },
  module: {
    rules: [
      { test: /\.vue$/, loader: "vue-loader" },
      {
        test: /\.ts$/,
        loader: "ts-loader",
        options: { appendTsSuffixTo: [/\.vue$/] }
      }
    ]
  }
};
```

还有一个是vue devtool chrome调试工具 不亮的问题,下载新的beta版问题就解决了.

### 2.处理html文件

这是个帮助你生成html文件的插件，最后产物是一个html模板导入你打包完的js

```powershell
npm install html-webpack-plugin -D
```

先在src下面创建一个index.html.里面是你生成html文件用的模板

修改webpack配置

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    // ...
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html',
            filename: 'index.html',
            title: 'Vue3 + TS -> Web App'
        })
    ]
}
```

比如我们用vscode自动生成一个html模板,title里面用`<%= htmlWebpackPlugin.options.title %>`就可以调用webpack里面html插件定义的变量title。

我们直接抄vue-cli的模板,吧index.html放到public文件夹里面。然后webpack里面html插件的入口也改成`./public/index.html`

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong
        >We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work
        properly without JavaScript enabled. Please enable it to
        continue.</strong
      >
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

```

他最后生成的html文件是如下

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />
    <title>Vue3 + TS -> Web App</title>
  <script defer src="index.js"></script></head>
  <body>
    <noscript>
      <strong
        >We're sorry but Vue3 + TS -> Web App doesn't work
        properly without JavaScript enabled. Please enable it to
        continue.</strong
      >
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

```

### 3.处理样式

webpack默认只能处理js文件，所以html，css这些都要相应的loader来处理

安装依赖

```powershell
npm install style-loader css-loader sass sass-loader -D
```

其中css-loader是处理css之间的依赖关系的，加载 CSS 文件并解析 import 的 CSS 文件，最终返回 CSS 代码

而style-loader是将模块导出的内容作为样式并添加到 DOM 中

sass 和sass-loader处理sass

修改webpack配置如下,这样样式就能引入到js了。

```javascript
const path = require('path');

module.exports = {
    // ...
    module: {
        rules: [
            // ...
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.s[ac]ss$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'sass-loader'
                ]
            }
        ]
    }
}

```

### 4.处理图片等静态资源（asset module）

资源模块(asset module)是一种模块类型，它允许使用资源文件（字体，图标等）而无需配置额外 loader。

webpack 5 版本之前我们通常使用

- [`raw-loader`](https://webpack.docschina.org/loaders/raw-loader/) 将文件导入为字符串
- [`url-loader`](https://webpack.docschina.org/loaders/url-loader/) 将文件作为 data URI 内联到 bundle 中
- [`file-loader`](https://webpack.docschina.org/loaders/file-loader/) 将文件发送到输出目录

资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：

- `asset/resource` 发送一个单独的文件并导出 URL。之前通过使用 `file-loader` 实现。
- `asset/inline` 导出一个资源的 data URI。之前通过使用 `url-loader` 实现。
- `asset/source` 导出资源的源代码。之前通过使用 `raw-loader` 实现。
- `asset` 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 `url-loader`，并且配置资源体积限制实现。

如果你想和以前的url-loader之类的兼容，可以添加一行`javascript/auto`来解决

```javascript

module.exports = {
  module: {
   rules: [
      {
        test: /\.(png|jpg|gif)$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
            }
          },
        ],
       type: 'javascript/auto'
      },
   ]
  },
}
```

如果向停止asset loader对新url的处理添加下面的一行

```
  dependency: { not: ['url'] },
```



这里我们就整一个比较通用的设置，那就是设置一个限制大小，小于这个大小的资源会被内联，大于这个大小的资源是采用引用的方式，直接把文件移动过去。

我们添加的配置如下,其中webpack5 默认内联资源的分界线是8kb

```javascript
const path = require('path');

module.exports = {
//...
  module: {
    rules: [
      {
        test: /\.(jpg|png|jpeg|gif|bmp|mp4|ogg|mp3|wav)$/,
        type: 'asset',
          parser: {
              dataUrlCondition: {
                  maxSize: 8 * 1024 // 8kb
              }
          }
      }
        //...
    ]
  },
};

```

### 5.配置开发服务器

有了开发服务器调试项目就会更方便，项目在代码改变后可以自动重新构建。

```powershell
npm install webpack-dev-server -D
```

webpack配置修改

```javascript
module.exports = {
    // ...
    devServer: {
        port: 3000,
        hot: true,
        open: true,
        contentBase: '../dist'
    },
    // ...
}
```

之后我们可以在package.json中添加一条命令

```
    "start": "webpack serve --config ./webpack.config.js",
```

在开发环境下，你还要添加sourceMap，sourceMap记录了源代码和编译后代码的映射关系，可以方便我们调试。在编译后的代码出错时，我们能通过sourceMap定位到源文件的错误发生的位置

inline-source-map会将sourceMap转换为dataURL插入到js文件中

```javascript
devtool: 'inline-source-map',
```



### 6.清理打包文件

webpack每次打包生成的文件,如果添加了hash,那么就不会覆盖原文件,而是在dist文件夹内保留,我们可以用一个 clean-webpack-plugin插件帮我们在打包前完成清理文件夹的工作,

```shell
npm install clean-webpack-plugin -D
```

配置

```javascript
const path = require('path');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    // ...
    plugins: [
        // ...
        new CleanWebpackPlugin()
    ]
}
```



## 03.代码检查和格式化

### 1.配置prettier

prettier是一个好用的代码格式化工具,直接安装,然后配置就完了.

```powershell
npm install --save-dev prettier
```

下面是我常用的最小配置

```javascript
//.prettierrc.js
module.exports = {
  semi: false,//行末无分号
  singleQuote: true,//字符串使用单引号
}
```

另外prettier和eslint是会产生冲突的,最典型的是trailing comma的问题,也就是对象和数组结尾的逗号.prettier默认的设置是不支持这个逗号的.而eslint是添加这个逗号的.

后面我们要安装一个兼容的插件,让prettier和eslint不发生冲突

### 2.配置eslint

安装eslint和typescript相关插件

```powershell
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```



webpack插件,其实eslint单独就够用了,配合lint-staged,没必要在webpack编译的时候也加上了.纯属降低速度

https://webpack.docschina.org/plugins/eslint-webpack-plugin/

加上这个其实主要作用还是在用没有eslint功能的ide写代码的时候.但是vscode有eslint插件所以没必要配置了.




关于eslint也需要配置文件`.eslintrc.js`,可以用`npx eslint --init` 进行创建

因为我要用vue和 standardjs规范,所以实际会安装那么多插件

```
eslint-plugin-vue@latest
@typescript-eslint/eslint-plugin@latest
eslint-plugin-import@^2.22.1
eslint-plugin-node@^11.1.0
eslint-plugin-promise@^4.2.1 || ^5.0.0
@typescript-eslint/parser@latest
```



生成的配置文件我们再修改一下,如下

```JavaScript
//.eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  extends: ['plugin:vue/vue3-recommended', 'prettier'],
  parserOptions: {
    ecmaVersion: 12,
    parser: '@typescript-eslint/parser',
    sourceType: 'module',
  },
  plugins: ['vue', '@typescript-eslint'],
  rules: {},
}
```

我们再看一下vue-cli生成的eslint配置,他那边就插件还是挺多的,

他的插件也太多太乱了.

```
'plugin:vue/vue3-essential',
'eslint:recommended',
'@vue/typescript/recommended',
'@vue/prettier',
'@vue/prettier/@typescript-eslint',
```

### 3.兼容eslint和prettier

然后关于配置prettier和eslint的兼容,除了自己手动修改配置文件以外,也可以安装一个插件实现

```undefined
npm install --save-dev eslint-config-prettier
```

只需要在eslint的配置文件extends最后加上

```
  extends: ['plugin:vue/vue3-recommended', 'prettier'],
```

### 4.配置stylelint

stylelint可以帮我们检查css代码

```powershell
npm install --save-dev stylelint-config-prettier stylelint
```

创建stylelintrc.js,添加规则,并且添加prettier的兼容

```javascript
module.exports = {
  extends: [
    //
    'stylelint-config-standard',
    'stylelint-config-prettier',
  ],
}
```

### 5.配置EditorConfig

EditorConfig因为vscode没有原生支持,说实话也没什么配的必要了,要安装一个插件纯属给自己添加麻烦,所以一般情况下也不用配了,vscode设置就够用了.

下面是我写前端代码时的配置

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
indent_size = 2
indent_style = space
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
trim_trailing_whitespace = false
```

### 6.配置git message检查

git message检查也是一般用不到的,用vscode commit插件就够了

但是lint-staged和husky配一下还是有用的

## 04.优化

我们可以直接抄vue-cli用的一些插件

执行下面这行命令可以查看vue-cli的插件列表

```powershell
vue inspect --plugins
```

目前的输出是这样的：

```javascript
[
  'vue-loader',
  'feature-flags',
  'define',
  'case-sensitive-paths',
  'friendly-errors',
  'html',
  'preload',
  'prefetch',
  'copy',
  'fork-ts-checker'
]
```

其中

- vue-loader是webpack解析vue文件必须的插件
- feature-flags 是控制发布代码用的一个插件,你可以定义一些flag,在你的代码中判断这些flag决定最终的功能.
- DefinePlugin 允许在 **编译时** 创建配置的全局常量,这是个webpack自带的插件
- case-sensitive-paths 这个插件强制模块路径与磁盘上的实际路径匹配,主要是mac系统会有这个问题,所以win和linux不用装这个.
- friendly-errors 更友好的错误提示
- html 前面的基础配置里已经装过了
- preload 预加载js代码
- prefetch 预加载资源
- copy  就是复制文件,把已经存在的文件或目录复制到某个目录,比如你写了个项目,要把readme也复制到输出里....
- fork-ts-checker 在独立进程上运行ts检查.

很明显 DefinePlugin ,friendly-errors,还有fork-ts-checker可以抄一下,比较通用,其他插件需求度倒是没那么高,要用的时候再引入就是了

### 1.更友好的错误提示

```
npm install friendly-errors-webpack-plugin --save-dev
```

https://github.com/geowarin/friendly-errors-webpack-plugin

配置

```javascript
var FriendlyErrorsWebpackPlugin = require('friendly-errors-webpack-plugin');

var webpackConfig = {
  // ...
  plugins: [
    new FriendlyErrorsWebpackPlugin(),
  ],
  // ...
}
```

### 2.提升typescript代码检查速度 

https://github.com/TypeStrong/fork-ts-checker-webpack-plugin

安装fork-ts-checker-webpack-plugin插件,可以开启一个新进程执行代码检查. 支持typescript和eslint,同时会有更友好的错误提示

```
npm install --save-dev fork-ts-checker-webpack-plugin
```

配置webpack

我们需要再ts-loader里面配置transpileOnly为true,也就是只进行转译,这样代码检查就会交给这个插件进行,如下

```javascript
// webpack.config.js
const ForkTsCheckerWebpackPlugin = require('fork-ts-checker-webpack-plugin');

module.exports = {
  context: __dirname, // to automatically find tsconfig.json
  entry: './src/index.ts',
  resolve: {
    extensions: [".ts", ".tsx", ".js"],
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: 'ts-loader',
        exclude: /node_modules/,
        options: {
          // disable type checker - we will use it in fork plugin
          transpileOnly: true
        }
      }
    ]
  },
  plugins: [new ForkTsCheckerWebpackPlugin()]
};
```

这个插件虽然支持eslint,但是没必要.eslint检查vscode在编码的时候就能执行,而且git提交的时候拦截一下就好了.



添加webpack的类型提示

```javascript
npm install --save-dev @types/webpack
```



### 3.分析文件打包大小

webpack-bundle-analyzer https://github.com/webpack-contrib/webpack-bundle-analyzer

### 4.缩小打包范围

webpack配置exclude 和include exclude的优先级更高

### 5.缓存

babel-loader可以配置缓存,ts-loader也有实验性的缓存选项.

cache-loader 插件可以实现各种loader的缓存,只推荐对性能较大的loader使用,因为缓存文件是在磁盘上的,保存和读取这些缓存也会有时间开销

https://github.com/webpack-contrib/cache-loader

```powershell
npm install --save-dev cache-loader
```

配置

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.ext$/,
        use: ['cache-loader', ...loaders],
        include: path.resolve('src'),
      },
    ],
  },
};

```



### 7.设置环境变量

我们可以用NODE_ENV环境变量指定webpack的工作模式。

也可以直接命令中指定环境变量。

但是有一个问题是windows环境下和linux环境下指定环境变量的方式存在差异

我们可以用`cross-env`设置环境变量来消除这一差异

```powershell
npm install cross-env -D
```

在package.json中添加

```javascript
{
    // ...
    "scripts": {
        "webpack": "cross-env NODE_ENV=development webpack"
    }
    // ...
}
```

### 8.分环境打包

项目开发中一般会分为开发环境，测试环境和生产环境。

生产环境我们一般会对代码进行各种压缩，减少打包的体积。

#### 打包压缩

##### 压缩html文件

我们只需要配置html-webpack-plugin

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    // ...
    plugins: [
        new HtmlWebpackPlugin({
            // ...
+            minify: {
+                collapseWhitespace: true, // 去掉空格
+                removeComments: true // 去掉注释
+            }
        }),
        // ...
    ]
}
```

##### 压缩css文件

MiniCssExtractPlugin 可以提取css文件到单个文件里面

css-minimizer-webpack-plugin 则是提供css压缩的功能

purgecss-webpack-plugin

因为还要分文件，还要用webpack merge，整体的配置实在太麻烦了，所以先了解到这里。