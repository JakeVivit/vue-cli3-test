---
title: Vue Cli3----由浅入深
---

本文主要介绍vue cli3的功能和特点，以及它和vue cli2X的对比，vue cli3具有功能丰富，易于扩展，无需Eject，更具有面向未来的特点，那么把你的项目构建于vue cli3之上吧



### 1.此前，Vue CLI 3.0 已发布，该版本经历了重构，旨在：

+ 1.减少现代前端工具的配置烦扰，尤其是在将多个工具混合在一起使用时；

+ 2.尽可能在工具链中加入最佳实践，让它成为任意 Vue 应用程序的默认实践。

--------------------

### 2.&nbsp;&nbsp;&nbsp;功能丰富
+ 不仅支持Babel、TypeScript、ESLint，还支持了[PostCSS](https://segmentfault.com/a/1190000011595620)、
[PWA](https://blog.csdn.net/u010009623/article/details/54313233)   
+ 支持多页面模式---构建具有多个 HTML / JS 入口点的应用程序
+ 无需Eject

------------------------

### 3.&nbsp;&nbsp;&nbsp;起步


***安装(node >= 8.9)***

```javascript
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

```
vue create vue-cli3-test
```
来初始化一个项目

![图片1](https://zhangchongvip.oss-cn-beijing.aliyuncs.com/vue-cli3/cli3-1.png "选择执行方式")

我们选中的是 Manually select features, 用来手动设置选项，在面向生产的项目是更加需要。

![图片2](https://zhangchongvip.oss-cn-beijing.aliyuncs.com/vue-cli3/QQ%E6%88%AA%E5%9B%BE20180816120338.png "手动选择如上特性")

***在本文中我们会简单介绍 PWA, CSS Pre-processors和Unit Testing的用法，并在之后会写Blog做更详细的介绍***


***项目目录介绍***

![图片3](https://zhangchongvip.oss-cn-beijing.aliyuncs.com/vue-cli3/QQ%E6%88%AA%E5%9B%BE20180816130203.png "目录结构")

目录结构相比vue-cli2简洁了不少，主要配置文件说明如下：

+ tests-----单元测试目录

+ .browserslistrc-----浏览器兼容配置文件

+ .eslintrc.js-----eslint代码检查配置

+ .postcssrc.js-----postcss配置文件

+ .package.json-----项目依赖和启动的配置文件

但是我们看到文件目录中缺少了有关的webpack的配置，在package.json文件中有这样的一段代码

```javascript
"scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "test:unit": "vue-cli-service test:unit"
  },
```

当我们在控制台输入 npm dev server 很显然项目可以启动，并带有热更新和热重载，然而我们并没有在项目目录中发现 devServer 的配置项，
其实在Vue CLI的项目，中，@vue/cli-service 安装了一个名为 vue-cli-service 的命令。你可以在 npm scripts 中以 vue-cli-service、
或者从终端中./node_modules/.bin/vue-cli-service 访问这个命令。

------------------------

### 4.&nbsp;&nbsp;&nbsp;vue-config.js配置详解

首先我们直接启动本地服务：
```javascript
npm run serve
```
在默认端口8080已经可以启动，然后我们在根目录下新建一个 ***vue.config.js*** 文件，并写上一下代码：
```javascript
module.exports = {
    devServer: {
        host: '0.0.0.0',
        port:8082,
        https:false
    }
}    
```

把端口改为8082并执行 ***npm run serve*** 我们发现本地服务启动的端口号改为了8082，显然，在执行***npm run serve*** 的时候，服务指向了 ***vue.config.js***这个文件。从[官方文档](https://cli.vuejs.org/zh/config/#全局-cli-配置)中我们了解到，vue-cli3简化了webpack的配置，并且有丰富的API可以供我们使用。

下面是我们常用的一些基本配置，很显然，即便是我们不修改vue.config.js用官方木人的配置一样可以完美运行，但是在实际项目中，往往需要一些特殊需求，所以修改配置还是蛮重要的一个环节。

```javascript
module.exports = {
    //部署应用时的基本 URL。默认情况下，Vue CLI 会假设你的应用是被部署在一个域名的根路径上，
    //例如 https://www.my-app.com/。如果应用被部署在一个子路径上，你就需要用这个选项指定这个子路径。
    //例如，如果你的应用被部署在 https://www.my-app.com/my-app/，则设置 baseUrl 为 /my-app/。
    //这个值在开发环境下同样生效。如果你想把开发服务器架设在根路径，你可以使用一个条件式的值
    baseUrl: process.env.NODE_ENV === 'production' ? '/my-app/' : '/',

    //当运行 vue-cli-service build 时生成的生产环境构建文件的目录。
    //注意目标目录在构建之前会被清除 (构建时传入 --no-clean 可关闭该行为)。
    outputDir: 'build',

    //放置生成的静态资源 (js、css、img、fonts) 的 (相对于 outputDir 的) 目录
    assetsDir: 'static',

    //指定生成的 index.html 的输出路径 (相对于 outputDir)。也可以是一个绝对路径。
    indexPath: 'index_prod.html',

    //默认情况下，生成的静态资源在它们的文件名中包含了 hash 以便更好的控制缓存。
    //然而，这也要求 index 的 HTML 是被 Vue CLI 自动生成的。如果你无法使用 Vue CLI 生成的 index HTML，你可以通过将这个选项设为 false 来关闭文件名哈希。
    // filenameHashing: false,

    //当 lintOnSave 是一个 truthy 的值时，eslint-loader 在开发和生产构建下都会被启用。
    //如果你想要在生产构建时禁用 eslint-loader，你可以用如下配置：
    lintOnSave: process.env.NODE_ENV !== 'production',

    //是否使用包含运行时编译器的 Vue 构建版本。
    //设置为 true 后你就可以在 Vue 组件中使用 template 选项了，但是这会让你的应用额外增加 10kb 左右。
    runtimeCompiler: false,

    //默认情况下 babel-loader 会忽略所有 node_modules 中的文件。
    //如果你想要通过 Babel 显式转译一个依赖，可以在这个选项中列出来。
    transpileDependencies: [],

    //如果你不需要生产环境的 source map，可以将其设置为 false 以加速生产环境构建。
    productionSourceMap: true,

    //设置生成的 HTML 中 <link rel="stylesheet"> 和 <script> 标签的 crossorigin 属性。
    //使用crossorigin属性，使得加载的跨域脚本可以得出跟同域脚本同样的报错信息。
    crossorigin: undefined,

    //在生成的 HTML 中的 <link rel="stylesheet"> 和 <script> 标签上启用 Subresource Integrity (SRI)。
    //如果你构建后的文件是部署在 CDN 上的，启用该选项可以提供额外的安全性, 这个标签是为了防止 CDN 篡改 javascript 用的。 。
    integrity: false,


    //（高级用法）这是一个一个函数，这个库提供了一个 webpack 原始配置的上层抽象，
    //使其可以定义具名的 loader 规则和具名插件，并有机会在后期进入这些规则并对它们的选项进行修改。
    //允许对内部的 webpack 配置进行更细粒度的修改。
    chainWebpack: config => {
        //config.module
        //.rule('vue')
        //.use('vue-loader')
        //.loader('vue-loader')
        //.tap(options => {
        // 修改它的选项...
        // return options
        //})
    },

    //如果想在 JavaScript 中作为 CSS Modules 导入 CSS 或其它预处理文件，
    //该文件应该以 .module.(css|less|sass|scss|styl) 结尾：
    css: {
        //如果你想去掉文件名中的 .module，可以设置 vue.config.js 中的 css.modules 为 true
        modules: true,
        //如果你希望自定义生成的 CSS Modules 模块的类名
        loaderOptions: {
            css: {
                localIdentName: '[name]-[hash]',
                camelCase: 'only'
            },
            //比如你可以这样向所有 Sass 样式传入共享的全局变量：
            sass: {
                // @/ 是 src/ 的别名
                // 所以这里假设你有 `src/variables.scss` 这个文件
                // data: `@import "@/variables.scss";`
            }
        },
        //生产环境下是 true，开发环境下是 false
        //是否将组件中的 CSS 提取至一个独立的 CSS 文件中 (而不是动态注入到 JavaScript 中的 inline 代码)。
        //提取 CSS 在开发环境模式下是默认不开启的，因为它和 CSS 热重载不兼容。
        //然而，你仍然可以将这个值显性地设置为 true 在所有情况下都强制提取
        extract: process.env.NODE_ENV === 'production' ? true : false,
        sourceMap: false
    },

    //调整 webpack 配置最简单的方式就是在 vue.config.js 中的 configureWebpack 选项提供一个对象：
    //如果你需要基于环境有条件地配置行为，或者想要直接修改配置，
    //那就换成一个函数 (该函数会在环境变量被设置之后懒执行)。
    //该方法的第一个参数会收到已经解析好的配置。在函数内，你可以直接修改配置，或者返回一个将会被合并的对象：
    configureWebpack: config => {
        if (process.env.NODE_ENV === 'production') {
            //为生产环境修改配置
        } else {
            //为开发环境修改配置
        }
    },

    //第三方插件
    pluginOptions: {
        //...
    },



    devServer: {
        host: '0.0.0.0',
        port: 8082,
        https: false,
        //设置为 true 时，eslint-loader 会将 lint 错误输出为编译警告。默认情况下，警告仅仅会被输出到命令行，且不会使得编译失败。
        //如果你希望让 lint 错误在开发时直接显示在浏览器中，你可以使用 lintOnSave: 'error'。
        //这会强制 eslint-loader 将 lint 错误输出为编译错误，同时也意味着 lint 错误将会导致编译失败。
        //或者，你也可以通过设置让浏览器 overlay 同时显示警告和错误：
        overlay: {
            warnings: true,
            errors: true
        },
        //服务器接口代理
        // proxy: ''
    }

}
```

以上是vue.config.js详细配置，注释也是比较详细，vue-cli3也是希望通过简单的语言来抛弃之前webpack的繁杂的配置，能更好的让我们专注于逻辑代码层面。
在[官方文档](https://cli.vuejs.org/zh/config/#全局-cli-配置)介绍中, 注入的webpack.config.js自带支持 [PostCSS、CSS Modules 和包含 Sass、Less、Stylus 预处理器](https://vue-loader.vuejs.org/zh/guide/css-modules.html#用法)。在[链式操作](https://cli.vuejs.org/zh/guide/webpack.html#链式操作-高级)中我们可以修改更加贴合项目的loader.

------------------------------

### 5.&nbsp;&nbsp;&nbsp;Vue Cli3配置PWA

如果你还没有听说PWA或者对PWA知之甚少，请移步[这里](https://lzw.me/a/pwa-service-worker.html)，对PWA的解释在这里不做过多的赘述，作为前端开发的一员，网站渐进式增强体验(PWA)已经成为未来前端的趋势，所以我们应该积极的了解相关的知识。幸运的是，Vue Cli3已经全面支持在Cli阶段PWA的配置，这，才是该有的姿态。

对于serverWorker.js如果手动注入逻辑代码，显然会消耗程序员一大部分精力，尤其是对一个逻辑业务比较复杂的项目来说，这可能会让我们变的异常头痛，在很久之前，
webpack官方就出了自动构建PWA web应用的 pligins , [sw-precache-webpack-plugin](https://github.com/goldhand/sw-precache-webpack-plugin),后来还有了[offline-plugin](https://github.com/NekR/offline-plugin),无论哪一种都极大待简化我们构建PWA的成本。

想更多的了解 Vue Cli3请移步[这里](https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa)

