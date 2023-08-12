# 前端项目创建笔记

## 1.NodeJs下载

官网下载：https://nodejs.org/en

 nvm：Nodejs版本管理工具，通过nvm下载指定版本。

## 2.yarn下载

```shell
# 下载安装yarn(全局)
npm install -g yarn

# 查看yarn版本
yarn -v

# 卸载yarn
npm uninstall -g yarn
```

## 3.[Vite](https://cn.vitejs.dev/guide/)下载

Vite是一种新型前端构建工具。它主要由两部分组成：

- 一个开发服务器，它基于 [原生 ES 模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) 提供了 [丰富的内建功能](https://cn.vitejs.dev/guide/features.html)，如速度快到惊人的 [模块热更新（HMR）](https://cn.vitejs.dev/guide/features.html#hot-module-replacement)。
- 一套构建指令，它使用 [Rollup](https://rollupjs.org/) 打包你的代码，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。

Vite 意在提供开箱即用的配置，同时它的 [插件 API](https://cn.vitejs.dev/guide/api-plugin.html) 和 [JavaScript API](https://cn.vitejs.dev/guide/api-javascript.html) 带来了高度的可扩展性，并有完整的类型支持。

```shell
#使用 NPM:
npm create vite@latest

#使用 Yarn:
yarn create vite
```

创建项目后，再次执行yarn命令配置依赖

```shell
#下载包
yarn
```

运行项目

```shell
#使用 NPM:
npm run dev

#使用 Yarn
yarn dev
```

## 4.项目目录结构调整

※粗字为新建目录

| 第一级 | 第二级                                   | 第三级                                        |      |
| ------ | ---------------------------------------- | --------------------------------------------- | ---- |
| public |                                          |                                               |      |
| src    |                                          |                                               |      |
|        | **api **※ajax后端交互                    |                                               |      |
|        | assets                                   |                                               |      |
|        |                                          | **fonts** ※字体图标                           |      |
|        |                                          | **imgs** ※图片                                |      |
|        |                                          | **styles** ※全局CSS，去除默认样式，滚动条样式 |      |
|        | **bmod** ※业务模块相关的文件             |                                               |      |
|        | **config** ※全局配置文件，系统初始化文件 |                                               |      |
|        | **controller** ※全局控制器               |                                               |      |
|        | **locales **※语言包文件                  |                                               |      |
|        | **router** ※全局路由                     |                                               |      |
|        | **store** ※全局状态管理                  |                                               |      |
|        | **utils** ※全局公共方法                  |                                               |      |
|        | **views** ※UI组件                        |                                               |      |
|        | **global.d.ts** ※全局变量                |                                               |      |

## 5.第三方库下载以及配置

| 库            | 命令行                                             | 说明                                               |
| ------------- | -------------------------------------------------- | -------------------------------------------------- |
| axios         | npm install axios / yarn add axios                 | Http异步请求                                       |
| js-cookie     | npm install js-cookie / yarn add js-cookie         | cookie操作管理                                     |
| lodash        | npm install lodash / yarn add lodash               | Js/ts工具库                                        |
| nanoid        | npm install nanoid / yarn add nanoid               | 生成唯一字符串标识符（IDs）                        |
| normalize.css | npm install normalize.css / yarn add normalize.css | CSS 重置和标准化库                                 |
| pinia         | npm install pinia / yarn add pinia                 | Vue3状态管理                                       |
| qs            | npm install qs / yarn add qs                       | URL 查询字符串和对象之间进行序列化和反序列化的转换 |
| vant          | npm install vant / yarn add vant                   | 移动端 UI 组件库                                   |
| vue-router    | npm install vue-router / yarn add vue-router       | Vue3路由管理                                       |

```shell
# 生产环境的第三方库下载
yarn add -S axios js-cookie lodash nanoid normalize.css pinia qs vant vue-router
```



```shell
# 开发环境的第三方库下载
yarn add -D @types/node @types/js-cookie @types/lodash @types/qs postcss postcss-modules autoprefixer sass
```

##### 5-1.修改vite配置文件vite.config.ts

##### 5-2.修改tsconfig.json

```shell
# 往compilerOptions里添加以下配置
"baseUrl":".",

"path":{
      "@/*":["src/"],
    },
"allowArbitraryExtensions": true,

```

##### 5-3.在main.ts中引入配置文件



## 6.初始化全局变量，并绑定到根组件

| 全局变量              | 描述                   |
| --------------------- | ---------------------- |
| app                   | config目录下的配置信息 |
| tools                 | utils目录下公共方法    |
| lpk                   | 语言包                 |
| 方法initLoginUserInfo | 加载基础模块           |
| bmod                  | 加载业务模块           |
|                       |                        |

## 7.注册全局组件

##### 7.1全局组件放components里 //组件做一个全局注册，使用时不需要import

| 目录       | 子目录                  |                            |                        |
| ---------- | ----------------------- | -------------------------- | ---------------------- |
| Components |                         |                            |                        |
|            | **Icon**  // 子组件目录 |                            |                        |
|            |                         | **src**                    |                        |
|            |                         |                            | **Icon.vue**  //子组件 |
|            |                         |                            | **type.ts**            |
|            |                         | **index.ts**  //暴露子组件 |                        |

## 8.按需加载组件与国际化支持

//这次使用[Vant4](https://vant-ui.github.io/vant/#/zh-CN/quickstart) 

##### 8.1安装插件

```shell
# 通过 yarn 安装
yarn add unplugin-vue-components -D
```

##### 8.2配置插件

如果是基于 `vite` 的项目，在 `vite.config.js` 文件中配置插件：

```js
import vue from '@vitejs/plugin-vue';
import Components from 'unplugin-vue-components/vite';
import { VantResolver } from 'unplugin-vue-components/resolvers';

export default {
  plugins: [
    vue(),
    Components({
      resolvers: [VantResolver()],
    }),
  ],
};
```

##### 8.3国际化（[多语言切换](https://vant-ui.github.io/vant/#/zh-CN/locale)）

导入并初始化第三方语言包

## 9.主题定制

通过CSS变量来实现主题的定制

## 10.路由配置（初始化状态管理与路由，并渲染根组件）

##### 10.1初始化基础模块的路由配置

##### 10.2初始化各业务模块的路由配置

  业务路由导出到基础路由模块中，初始化

##### 10.3 路由鉴权

路由守卫中，进行路由鉴权

## 11.KeepAlive 组件化



## 12.基于Axios的ajax封装



## 13.BaseApi

##### 13.1BaseApi的封装

##### 13.2 BaseApi对Ajax请求Url与Params的自定义支持

















