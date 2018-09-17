## webpack 的配置详解

 webpack 的使用步骤：
 1 安装： npm i -D webpack webpack-cli
 2 webpack 使用的两种方式：
  2.1 命令行的使用方式（知道）
  2.2 配置文件

  ### 1.webpack的四个核心概念
  1.入口:entry
  2.出口:打包后输出的内容
  3.loaders 加载器：对于非js的静态资源：如css，图片等
  4.plugins 插件
  ## 2.命令行的形式使用
```
    演示命令行的使用方式：
 webpack 入口文件 出口文件路径
 最基本的打包: webpack ./src/main.js
 注意:使用 webpack 的时候应该提供mode, 可以是: production 或者 development
 production 表示: 生产模式    -- 生产环境(也就是给用户使用的)
 development 表示: 开发模式   -- 开发环境(也就是给开发人员开发使用的)

 指定模式: .\node_modules\.bin\webpack ./src/main.js --mode development
 指定为生产模式: .\node_modules\.bin\webpack ./src/main.js --mode production

 演示指定出口文件路径:
  .\node_modules\.bin\webpack ./src/main.js -o ./dist/a.js --mode production
```
  - 去掉.\node_modules\.bin\
   1.在package.json 的scripts中添加一个build脚本
   2.将webket命令作为build脚本值
     "build": "webpack ./src/main.js --mode development",
   3.在终端中执行命令:npm run build 来运行上面创建好的脚本


## 3. webpack 打包处理的过程：
   如果引入jquery使用 import $ from "jquery"
    在浏览器或者Nodejs中都无法直接识别import语法，
    但是经过webpack处理之后就可以识别了
  - 打包过程
  ```
  1.运行了webpack的打包命令以后：webpack./src/main.js --mode development
  2.webpack会找到我们指定的入口文件 main.js
  3.webpack就会分析main.js中的代码，当遇到import $ ...语法的时候，webpack就知道我们要使用jquery这个模块啦
  4.webpack 就会将jquery的代码拿过来
  5.然后就会继续分析，如果有遇到import语法就会继续加载这个模块
  6.直到分析完整个js文件后，将main.js中的所有用到的代码与我们自己的代码打包生成一个js文件，也就是dist/main.js
  ```

## 4.使用配置文件的方式
在根目录新建一个js文件，webpack.config.js
### webpack-dev-server

- 安装：`npm i -D webpack-dev-server`
- 作用：配合webpack，创建开发环境（启动服务器、监视文件变化、自动编译、刷新浏览器等），提高开发效率
- 注意：无法直接在终端中执行 `webpack-dev-server`，需要在 `package.json` 配置 `scripts` 后使用

#### 使用说明

- 注意：`webpack-dev-server`将打包好的文件存储在内存中，提高编译和加载速度，效率更高
- 注意：输出的文件被放到项目根目录中
  - 命令行中的提示：`webpack output is served from /`
  - 在`index.html`页面中直接通过 `/bundle.js` 来引入内存中的文件

#### 配置说明 - CLI配置

- `--contentBase` ：告诉服务器在哪个目录中提供服务（可以理解为：打开哪个目录中的index.html）
  - `--contentBase ./`：当前工作目录
  - `--contentBase ./src`：当前目录下的src文件夹
- `--open` ：自动打开浏览器
- `--port` ：端口号
- `--hot` ：热更新，只加载修改的文件(按需加载修改的内容)，而非全部加载

```js
/* package.json */
/* 运行命令：npm run dev */

{
  "scripts": {
    "dev": "webpack-dev-server --contentBase ./src --open --port 8888 --hot"
  }
}
```
## html-webpack-plugin
作用
- 根据指定模板页面（index.html）在内存中产生一个新的页面，并且浏览器打开的就是这个新的页面
- 能够自动引入css/js等文件
 使用
 1.安装：npm i -D html-webpack-plugin
 2.在webpack.config.js中导入这个模块
 3.在plugins中配置