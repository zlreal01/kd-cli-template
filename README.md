# kd-cli-template

基于 vue-cli4.0 + webpack 4 + vant ui + sass+ rem 适配方案+axios 封装，构建vue手机端模板脚手架

### Node 版本要求

`Vue CLI` 需要 Node.js 8.9 或更高版本 (推荐 8.11.0+)。可以使用nvm-windows在同一台电脑中管理多个 Node 版本。

本示例 Node.js 12.16.1

### 启动项目

```bash

cd 项目目录

npm install

npm run serve
```

### 配置多环境变量

`package.json` 里的 `scripts` 配置 `serve` `stage` `build`，通过 `--mode xxx` 来执行不同环境

- 通过 `npm run serve` 启动本地 , 执行 `development`
- 通过 `npm run stage` 打包测试 , 执行 `staging`
- 通过 `npm run build` 打包正式 , 执行 `production`


##### 配置介绍

以 `VUE_APP_` 开头的变量，在代码中可以通过 `process.env.VUE_APP_` 访问。  
比如,`VUE_APP_ENV = 'development'` 通过`process.env.VUE_APP_ENV` 访问。  
除了 `VUE_APP_*` 变量之外，在你的应用代码中始终可用的还有两个特殊的变量`NODE_ENV` 和`BASE_URL`

在项目根目录中新建`.env.*`

- .env.development 本地开发环境配置
- .env.staging 测试环境配置
- .env.production 正式环境配置

配置对应环境的变量，拿本地环境文件 `env.development.js` 举例，用户可以根据需求修改

### 集成瑞信api

Vue.prototype.$ruixin

###  VantUI 组件按需加载

项目采
用[Vant 自动按需引入组件 (推荐)](https://youzan.github.io/vant/#/zh-CN/quickstart#fang-shi-yi.-zi-dong-an-xu-yin-ru-zu-jian-tui-jian)下
面安装插件介绍：

[babel-plugin-import](https://github.com/ant-design/babel-plugin-import) 是一款 `babel` 插件，它会在编译过程中将
`import` 的写法自动转换为按需引入的方式

#### 安装插件

```bash
npm i babel-plugin-import -D
```

在`babel.config.js` 设置

```javascript
// 对于使用 babel7 的用户，可以在 babel.config.js 中配置
const plugins = [
  [
    'import',
    {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    },
    'vant'
  ]
]
module.exports = {
  presets: [['@vue/cli-plugin-babel/preset', { useBuiltIns: 'usage', corejs: 3 }]],
  plugins
}
```

#### 使用组件

项目在 `src/plugins/vant.js` 下统一管理组件，用哪个引入哪个，无需在页面里重复引用


#### 目录结构

所有全局样式都在 `@/src/assets/css` 目录下设置

```bash
├── assets
│   ├── css
│   │   ├── index.scss               # 全局通用样式
│   │   ├── mixin.scss               # 全局mixin
│   │   └── variables.scss           # 全局变量
```

#### 自定义 vant-ui 样式

现在我们来说说怎么重写 `vant-ui` 样式。由于 `vant-ui` 的样式我们是在全局引入的，所以你想在某个页面里面覆盖它的样式就不能
加 `scoped`，但你又想只覆盖这个页面的 `vant` 样式，你就可在它的父级加一个 `class`，用命名空间来解决问题。

```css
.about-container {
  /* 你的命名空间 */
  .van-button {
    /* vant-ui 元素*/
    margin-right: 0px;
  }
}
```

#### 父组件改变子组件样式 深度选择器

当你子组件使用了 `scoped` 但在父组件又想修改子组件的样式可以 通过 `>>>` 来实现：

```css
<style scoped>
.a >>> .b { /* ... */ }
</style>
```

#### 全局变量

`vue.config.js` 配置使用 `css.loaderOptions` 选项,注入 `sass` 的 `mixin` `variables` 到全局，不需要手动引入 ,配
置`$cdn`通过变量形式引入 cdn 地址,这样向所有 Sass/Less 样式传入共享的全局变量：
设置 js 中可以访问 `$cdn`,`.vue` 文件中使用`this.$cdn`访问

```javascript
// 引入全局样式
import '@/assets/css/index.scss'

// 设置 js中可以访问 $cdn
// 引入cdn
import { $cdn } from '@/config'
Vue.prototype.$cdn = $cdn
```

在 css 和 js 使用

```html
<script>
  console.log(this.$cdn)
</script>
<style lang="scss" scoped>
  .logo {
    width: 120px;
    height: 120px;
    background: url($cdn + '/weapp/logo.png') center / contain no-repeat;
  }
</style>
```

###  Vuex 状态管理

目录结构

```bash
├── store
│   ├── modules
│   │   └── app.js
│   ├── index.js
│   ├── getters.js
```

### Vue-router 

本工程采用 `hash` 模式，开发者根据需求修改 `mode` `base`

注意：如果你使用了 `history` 模式，`vue.config.js` 中的 `publicPath` 要做对应的修改

### Axios 封装及接口管理

`utils/request.js` 封装 axios ,开发者需要根据后台接口做修改。

- `service.interceptors.request.use` 里可以设置请求头，比如设置 `token`
- `config.hideloading` 是在 api 文件夹下的接口参数里设置，下文会讲
- `service.interceptors.response.use` 里可以对接口返回数据处理，比如 401 删除本地信息，重新登录

#### 接口管理

在`src/api` 文件夹下统一管理接口

- 你可以建立多个模块对接接口, 比如 `home.js` 里是首页的接口这里讲解 `user.js`
- `url` 接口地址，请求的时候会拼接上 `config` 下的 `baseApi`
- `method` 请求方法
- `data` 请求参数 `qs.stringify(params)` 是对数据系列化操作
- `hideloading` 默认 `false`,设置为 `true` 后，不显示 loading ui 交互中有些接口不需要让用户感知

#### 如何调用

```javascript
// 请求接口
import { getUserInfo } from '@/api/user.js'

const params = { user: 'sunnie' }
getUserInfo(params)
  .then(() => {})
  .catch(() => {})
```

### table 组件
默认使用 vxe-table
https://gitee.com/xuliangzhan_admin/vxe-table/


###  Webpack 4 vue.config.js 基础配置

如果你的 `Vue Router` 模式是 hash

```javascript
publicPath: './',
```

如果你的 `Vue Router` 模式是 history 这里的 publicPath 和你的 `Vue Router` `base` **保持一直**

```javascript
publicPath: '/app/',
```


###  配置 alias 别名 

```javascript
const path = require('path')
const resolve = dir => path.join(__dirname, dir)
const IS_PROD = ['production', 'prod'].includes(process.env.NODE_ENV)

module.exports = {
  chainWebpack: config => {
    // 添加别名
    config.resolve.alias
      .set('@', resolve('src'))
      .set('assets', resolve('src/assets'))
      .set('api', resolve('src/api'))
      .set('views', resolve('src/views'))
      .set('components', resolve('src/components'))
  }
}
```

###   配置 proxy 跨域 

```javascript
module.exports = {
  devServer: {
    // ....
    proxy: {
      //配置跨域
      '/api': {
        target: 'https://test.xxx.com', // 接口的域名
        // ws: true, // 是否启用websockets
        changOrigin: true, // 开启代理，在本地创建一个虚拟服务端
        pathRewrite: {
          '^/api': '/'
        }
      }
    }
  }
}
```

使用 例如: `src/api/home.js`

```javascript
export function getUserInfo(params) {
  return request({
    url: '/api/userinfo',
    method: 'post',
    data: qs.stringify(params)
  })
}
```

### 配置了打包分析 
### 可配置 externals 引入 cdn 资源 这里注释掉了
### 去掉了正式环境 console.log 保留了测试环境和本地环境的 `console.log`
### splitChunks 单独打包第三方模块
```javascript
module.exports = {
  chainWebpack: config => {
    config.when(IS_PROD, config => {
      config
        .plugin('ScriptExtHtmlWebpackPlugin')
        .after('html')
        .use('script-ext-html-webpack-plugin', [
          {
            // 将 runtime 作为内联引入不单独存在
            inline: /runtime\..*\.js$/
          }
        ])
        .end()
      config.optimization.splitChunks({
        chunks: 'all',
        cacheGroups: {
          // cacheGroups 下可以可以配置多个组，每个组根据test设置条件，符合test条件的模块
          commons: {
            name: 'chunk-commons',
            test: resolve('src/components'),
            minChunks: 3, //  被至少用三次以上打包分离
            priority: 5, // 优先级
            reuseExistingChunk: true // 表示是否使用已有的 chunk，如果为 true 则表示如果当前的 chunk 包含的模块已经被抽取出去了，那么将不会重新生成新的。
          },
          node_vendors: {
            name: 'chunk-libs',
            chunks: 'initial', // 只打包初始时依赖的第三方
            test: /[\\/]node_modules[\\/]/,
            priority: 10
          },
          vantUI: {
            name: 'chunk-vantUI', // 单独将 vantUI 拆包
            priority: 20, // 数字大权重到，满足多个 cacheGroups 的条件时候分到权重高的
            test: /[\\/]node_modules[\\/]_?vant(.*)/
          }
        }
      })
      config.optimization.runtimeChunk('single')
    })
  }
}
```


###  添加 IE 兼容