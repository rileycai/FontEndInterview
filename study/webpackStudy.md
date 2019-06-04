# webpack学习笔记

## loader

### postcss-loader
+ PostCSS是一个用 JavaScript 工具和插件转换 CSS 代码的工具，它有很多插件来转换css，可以大胆的使用未来语法。
+ 我们用的最多的插件就是autoprefixer：自动添加浏览器前缀，添加css兼容性写法。

 ```
 npm install --save-dev postcss-loader autoprefixer

 //代码示例
 {
     loader:'postcss-loader',
     options:{
         plugins:[
             autoprefixer({
                 browsers:[
                     'Android >= 4.3'
                 ]
             })
         ]
     }
 }
 ```

 ###  less-loader
 + 将 css-loader、style-loader 和 less-loader 链式调用，可以把所有样式立即应用于 DOM。
 ```
{
    loader: "style-loader" // creates style nodes from JS strings
}, {
    loader: "css-loader" // translates CSS into CommonJS
}, {
    loader: "less-loader" // compiles Less to CSS
}
 ```
###  file-loader and url-loader
+ file-loader 和 url-loader 可以接收并加载任何文件，然后将其输出到构建目录。
+ 当你 **import MyImage from './my-image.png'**，该图像将被处理并添加到 output 目录，_并且_ MyImage 变量将包含该图像在处理后的最终 url。当使用 css-loader 时，如上所示，你的 CSS 中的 **url('./my-image.png')** 会使用类似的过程去处理。loader 会识别这是一个本地文件，并将 './my-image.png' 路径，替换为输出目录中图像的最终路径。html-loader 以相同的方式处理 <img src="./my-image.png" />。

### csv-loader 和 xml-loader
+ 可以加载的有用资源还有数据，如 JSON 文件，CSV、TSV 和 XML。类似于 NodeJS，JSON 支持实际上是内置的，也就是说 import Data from './data.json' 默认将正常运行。要导入 CSV、TSV 和 XML，你可以使用 **csv-loader** 和 **xml-loader**。
+ 在使用 d3 等工具来实现某些数据可视化时，预加载数据会非常有用。我们可以不用再发送 ajax 请求，然后于运行时解析数据，而是在构建过程中将其提前载入并打包到模块中，以便浏览器加载模块后，可以立即从模块中解析数据。


## plugins

### html-webpack-plugin
[html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin)
+ 该插件将为您生成一个HTML5文件，其中包含使用脚本标记的正文中的所有webpack包。

### clean-webpack-plugin
+ 由于过去的指南和代码示例遗留下来，导致我们的 /dist 文件夹相当杂乱。webpack 会生成文件，然后将这些文件放置在 /dist 文件夹中，但是 webpack 无法追踪到哪些文件是实际在项目中用到的。通常，在每次构建前清理 /dist 文件夹，是比较推荐的做法，因此只会生成用到的文件。