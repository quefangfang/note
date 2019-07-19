## taro构建微信小程序

### 安装

先全局安装`@tarojs/cli`
```
# 使用 npm 安装 CLI
$ npm install -g @tarojs/cli
# OR 使用 yarn 安装 CLI
$ yarn global add @tarojs/cli
# OR 安装了 cnpm，使用 cnpm 安装 CLI
$ cnpm install -g @tarojs/cli
```
初始化项目

```javascript
$ taro init myApp
```

根据提示 输入配置，创建完项目后 安装依赖

```
# 使用 yarn 安装依赖
$ yarn
# OR 使用 cnpm 安装依赖
$ cnpm install
# OR 使用 npm 安装依赖
$ npm install
```

### 开发

微信小程序编译预览及打包（去掉 --watch 将不会监听文件修改，并会对代码进行压缩打包）

```
# yarn
$ yarn dev:weapp
$ yarn build:weapp
# npm script
$ npm run dev:weapp
$ npm run build:weapp
# 仅限全局安装
$ taro build --type weapp --watch
$ taro build --type weapp
```

#### 生命周期

| 生命周期方法       | 作用                                                         | 说明                                                     |
| ------------------ | ------------------------------------------------------------ | -------------------------------------------------------- |
| componentWillMount | 组件加载时触发，一个组件只会调用一次，此时组件 DOM 尚未准备好，还不能和视图层进行交互 | 对应微信小程序`onLaunch`                                 |
| componentDidMount  | 组件初次渲染完成时触发，一个组件只会调用一次，代表组件已经准备妥当，可以和视图层进行交互 | 对应微信小程序`onLaunch`，在`componentWillMount`之后执行 |
| componentDidShow   | 页面显示/切入前台时触发                                      | 对应微信小程序`onShow`                                   |
| componentDidHide   | 页面隐藏/切入后台时触发， 如 navigateTo 或底部 tab 切换到其他页面，小程序切入后台等 | 对应微信小程序`onHide`                                   |

不过当然也包含`componentWillUnmout`和`componentWillReceiveProps`等react原始生命周期函数，用来编写自定义组件。



#### 路由

在 Taro 中，路由功能是默认自带的，不需要开发者进行额外的路由配置。

```javascript
// 跳转到目的页面，打开新页面
Taro.navigateTo({
  url: '/pages/page/path/name'
})

// 跳转到目的页面，在当前页面打开
Taro.redirectTo({
  url: '/pages/page/path/name'
})
```

#### 路由传参

```javascript
// 传入参数 id=2&type=test
Taro.navigateTo({
  url: '/pages/page/path/name?id=2&type=test'
})
```

在跳转成功的目标页的**生命周期**方法里就能通过 `this.$router.params` 获取到传入的参数，例如上述跳转，在目标页的 `componentWillMount` 生命周期里获取入参

```js
class C extends Taro.Component {
  componentWillMount () {
    console.log(this.$router.params) // 输出 { id: 2, type: 'test' }
  }
}
```

#### 组件

Taro 以 微信小程序组件库 为标准，结合 jsx 语法规范，定制了一套自己的组件库规范。这部分可以自行去看文档。**值得注意的是，小程序中的写法bind\*这种事件都要改成以on开头。**

#### 引入Redux/Mobox

-  [引入Redux](http://taro-docs.jd.com/taro/docs/redux.html)

- [引入Mobox](http://taro-docs.jd.com/taro/docs/mobx.html)



#### 异步编程

Taro 支持使用 `async functions` 来让开发者获得不错的异步编程体验，开启 `async functions` 支持需要安装包 `@tarojs/async-await`

```
$ yarn add @tarojs/async-await
# 或者使用 npm
$ npm install --save @tarojs/async-await
```

随后在项目入口文件 `app.js` 中直接 `import` ，就可以开始使用 `async functions` 功能了

```javascript
// src/app.js
import '@tarojs/async-await'
```



### 官方文档

- [taro官方文档](https://taro.aotu.io/)
- [taro-ui 官方文档](https://taro-ui.aotu.io/)

  