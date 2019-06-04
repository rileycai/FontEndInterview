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
###  file-loader
+ 假想，现在我们正在下载 CSS，但是我们的背景和图标这些图片，要如何处理呢？使用 file-loader，我们可以轻松地将这些内容混合到 CSS 中
