## webpack 使用手册



#### 1 初始化包管理配置文件

`npm init -y `

------



#### 2 安装jQuery

`npm install jquery -S`

------



#### 3 在项目中安装webpack

安装webpack ： `npm install webpack`

安装 webpack-cli ：`npm install webpack-cli --save-dev`

------



#### 4 在项目中配置webpack

① 在项目根目录中，创建名为 webpack.config.js 的 webpack 配置文件，并初始化如下的基本配置：

```javascript
module.exports = {
    mode: 'development' // mode 用来指定构建模式。可选值有 development 和 production
}
```

② 在 package.json 的 scripts 节点下，新增 dev 脚本如下：

```javascript
"script":{
    "dev": "webpack" // script 节点下的脚本，可以通过 npm run 执行。例如 npm run dev
}
```

③ 在终端中运行 `npm run dev` 命令，启动 webpack 进行项目的打包构建

------



#### 5 配置 webpack 打包入口以及输出入口

在 webpack 4.x 和 5.x 的版本中，有如下的默认约定：

① 默认的打包入口文件为 src -> index.js

② 默认的输出文件路径为 dist -> main.js

在 webpack.config.js 配置文件中，通过 entry 节点指定打包的入口。通过 output 节点指定打包的出口。示例代码如下：

```javascript
const path = require('path') // 导入 node.js 中专门操作路径的模块

module.exports = {
    entry: path.join(__dirname,'./src/index.js'), // 打包入口文件的路径
    output: {
        path: path.join(__dirname,'./dist'), // 输出文件的存放路径
        filename: 'bundle.js' // 输出文件的名称
    }
}
```

------



#### 6 webpack 插件的作用

① webpack-dev-server

- 类似于 node.js 阶段用到的 nodemon 工具
- 每当修改了源代码，webpack 会自动进行项目的打包和构建

安装插件：`npm install webpack-dev-server -D`

配置 webpack-dev-server ：

1. 修改 package.json -> srcripts 中的 dev 命令如下：

   ```javascript
   "script":{
   	"dev": "webpack serve", // script 节点下的脚本，可以通过 npm run 执行
   }
   ```

   在 index.html 中，将 `<script src="../dist/bundle.js"></script>` 替换为 `<script src="/bundle.js"></script>` ，（因为是webpack_dev_server部署服务器(使用内存空间)地址在localhost:8080,所以可以直接使用"/"来访问，所以“/bundle.js”是从内存导入的）

2. 再次运行 `npm run dev` 命令，重新进行项目的打包

3. 在浏览器中访问 http://localhost:8080 地址，查看自动打包效果

② html-webpack-plugin

- webpack 中的 HTML 插件（类似于一个模板引擎插件）
- 可以通过此插件自定制 index.html 页面的内容

安装插件：`npm install html-webpack-plugin -D`

配置 html-webpack-plugin ：

```javascript
// 1. 导入 HTML 插件，得到一个构建函数
const HtmlPlugin = require('html-webpack-plugin')

// 2. 创建 HTML 插件的实例对象
const htmlPlugin = new HtmlPlugin({
    template: './src/index.html', // 指定源文件的存放路径
    filname: './index.html', // 指定生成的文件的存放路径
})

module.exports = {
	mode: 'development',
    plugins: [htmlPlugin], // 3. 通过 plugins 节点，使 htmlPlugin 插件生效
}
```

> HTML 插件在生成的 index.html 页面，自动注入了打包的 bundle.js 文件

------



#### 7 devServer 节点

在 webpack.config.js 配置文件中，可以通过 devServer 节点对 webpack-dev-server 插件进行更多的配置示例代码如下：

```javascript
devServer: {
    open: true, // 初次打包完成后，自动打开浏览器
    host: '127.0.0.1', // 实时打包所使用的主机地址
    port: 80, // 实时打包所使用的端口号
}
```

注意：凡是修改了 webpack.config.js 配置文件，或修改了 package.json 配置文件，必须重启实时打包的服务器，否则最新的配置文件无法生效！

------



#### 8 loader 概述

在实际开发过程中，webpack 默认智能打包处理以 .js 后缀名结尾的模块。其他非 .js 后缀名结尾的模块，webpack 默认处理不了，需要调用 loader 加载器才可以正常打包，否则会报错！

loader 加载器的作用：协助 webpack 打包处理特定的文件模块。比如：

- css-loader 可以打包处理 .css 相关的文件
- less-loader 可以打包处理 .less 相关的文件
- babel-loader 可以打包处理 webpack 无法处理的高级 JS 语法

① 打包处理 css 文件

1. 运行 `npm i style-loader css-loader -D` 命令，安装处理 css 文件的 loader

2. 在webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：

   ```javascript
   module: { // 所有第三方文件模块的匹配规则
       rules: [ // 文件后缀名的匹配规则
           { test: /\.css$/, use: ['style-loader', 'css-loader'] }
       ]
   }
   ```

   其中，test 表示匹配的文件类型， use 表示对应要调用的 loader

   注意：

   - use 数组中指定的 loader 顺序是固定的
   - 多个 loader 的调用顺序是：从后往前调用 

> 在 index.js 中引入 css 文件，`import './css/index.css'`

② 打包处理 less 文件

1. 运行 `npm i less-loader less -D` 命令

2. 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：

   ```javascript
   module: {
       rules: [
           { test: /\.less$/, use: ['style-loader', 'css-loader' , 'less-loader'] },
       ]
   }
   ```

   > 在 index.js 中引入 less 文件，`import './css/index.less'`

③ 打包处理样式表中与 url 路径相关的文件

1. 运行 `npm i url-loader file-loader -D` 命令

2. 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：

   ```javascript
   module: {
       rules: [
           { test: /\.jpg|png|gif$/, use: 'url-loader?limit=22229' },
       ]
   }
   ```

   其中 ？ 之后的是 loader 的参数项：

   - limit 用来指定图片的大小，单位是字节（byte）
   - 只有 ≤ limit 大小的图片，才会被转为 base64 格式的图片
   
   使用示例：
   
   ```html
   <!-- html -->
   <img src="" alt="" class="box">
   ```
   
   ```javascript
   // js
   // 1. 导入图片，得到图片文件
   import logo from './images/logo.png'
   // 2. 给 img 标签的 src 动态赋值
   $('.box').attr('src',logo)
   ```

④ 打包处理 js 文件中的高级语法

webpack 只能打包处理一部分高级的 JavaScript 语法。对于那些 webpack 无法处理的高级 js 语法，需要借助于 babel-loader 进行打包处理。例如 webpack 无法处理下面的 JavaScript 代码：

```javascript
// 1. 定义了名为 info 的在装饰器
function info(target){
    // 2. 为目标添加静态属性 info
    target.info = 'Person info'
}

// 3. 为 Person 类应用 info 装饰器
@info
class Person {}

// 4. 打印 Person 的静态属性 info
console.log(Person.info)
```

1. 运行如下的命令安装对应的依赖包：` npm i babel-loader @babel/core @babel/plugin-proposal-decorators -D`

2. 在 webpack.config.js 的 module->rules 数组中，添加 loader 规则如下：

   ```javascript
   // 注意： 必须使用 exclude 指定排除项，因为 node_modules 目录下的第三方包不需要被打包
   { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
   ```

3. 在项目根目录下，创建名为 babel.config.js 的配置文件，定义 Babel 的配置项如下：

   ```javascript
   module.exports = {
       // 声明 babel 可用的插件
   	// 将来，webpack 在调用 babel-loader 的时候，会先加载 plugins 中的插件来使用
       plugins: [ [ '@babel/plugin-proposal-decorators', { legacy: true } ] ]
   }
   ```

------



#### 9 配置 webpack 的打包发布

① 在 package.json 文件的 scripts 节点下，新增 build 命令如下：

```javascript
"scripts": {
    "build": "webpack --mode production" // 项目发布时，运行 build 命令
}
```

--model 是一个参数项，用来指定 webpack 的运行模式。production 代表生产环境，会对打包生成的文件进行代码雅俗和性能优化。

注意：通过 --model 指定的参数项，会覆盖 webpack.config.js 中的 model 选项。

② 把 JavaScript 文件统一生产到 js 目录中

在 webpack.config.js 配置文件的 output 节点中，进行如下的配置：

```javascript
output: {
    path: path.join(__dirname,'dist'),
    // 明确告诉 webpack 把生成的 bundle.js 文件存放到 dist 目录下的 js 子目录中
    filename: 'js/bundle.js',
}
```

③ 把图片文件统一生成到 image 目录中

修改 webpack.config.js 中的 url-loader 配置项，新增 outputPath 选项即可指定图片文件的输出路径：

```javascript
{
    test: /\.jpg|png|gif$/,
    use: {
        loader: 'url-loader',
        options: {
            limit: 22228,
            // 明确指定把打包生成的图片文件，存储到 dist 目录下的 iamge 文件夹中
            outputPath: 'image',
        },
    },
}

// 或者
{ test: /\.jpg|png|gif$/, use: 'url-loader?limit=470&outputPath=images' }
```

> 在配置 url-loader 的时候，多个参数之间，使用 $ 符号进行分隔

④ 自动清理 dist 目录下的旧文件

为了在每次打包发布时自动清理掉 dist 目录中的旧文件，可以安装并配置 clean-webpack-plugin 插件：

1. 安装清理 dist 目录的 webpack 插件

   `npm install clean-webpack-plugin -D`

2. 按需导入插件，得到插件的构造函数之后，创建插件的实例对象

   ```javascript
   const { CleanWebpackPlugin } = require('clean-webpack-plugin') // 左侧的花括号是解构赋值
   const cleanPlugin = new CleanWebpackPlugin()
   ```

3. 把创建的 cleanPlugin 插件实力对象，挂载到 plugins 节点中

   ```javascript
   plugins: [cleanPlugin], // 挂载插件
   ```

------



#### 10 Source Map

Source Map 就是一个信息文件，里面储存着位置信息。也就是说，Source Map 文件中储存着压缩后的代码，所对应的转换前的位置。

有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码，能够极大的方便后期的调试。

开发环境下默认生成的 Source Map，记录的是生成后的代码的位置。会导致运行时报错的行数与源代码的行数不一致的问题。

开发环境下，推荐在 webpack.config.js 中添加如下的配置，即可保证运行时报错的行数与源代码的行数保持一致：

```javascript
module.exports = {
    mode: 'development',
    // eval-source-map 仅限在“开发模式”下使用，不建议在“生产模式”下使用。
    // 此选项生成的 Source Map 能够保证“运行时报错的行数”与“源代码的行数”保持一致
    devtool: 'eval-source-map',
    // 省略其它配置项...
}
```

在生产环境下，如果省略了 devtool 选项，则最终生成的文件不包含 Source Map。这能够防止原始代码通过 Source Map 的形式暴露给别有所图之人。

在生产环境下，如果只想定位报错的具体行数，且不想暴露源码。此时可以将 devtool 的值设置为 nosources-source-map 。

Source Map 的最佳实践：

① 开发环境下：

- 建议把 devtool 的值设置为 eval-source-map
- 好处：可以精准定位到具体的错误行

② 生产环境下：

- 建议关闭 Source Map 或将 devtool 的值设置为 nosources-source-map
- 好处：防止源码泄露，提高网站的安全性

------



#### 11 @

建议大家使用 @ 表示 src 源代码目录，从外往里查找；不要使用 ../ 从里往外查找

使用 @ 之前，需要在 webpack.config.js 中配置：

```javascript
module.exports = {
    // ...
    resolve: {
        alias: {
            // 告诉 webpack ，程序员写的代码中，@ 符号表示 src 这一层目录
            '@': path.join(__dirname,'./src/')
        },
    },
    // ...
}
```

