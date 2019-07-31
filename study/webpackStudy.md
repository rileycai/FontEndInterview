# webpack学习笔记

## loader
+ loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。
+ 本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。
+ 在更高层面，在 webpack 的配置中 loader 有两个目标：
1. **test** 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
2. **use** 属性，表示进行转换时，应该使用哪个 loader。

### loader特性
+ loader 支持 **链式传递**。能够对资源使用流水线(pipeline)。一组链式的 loader 将按照相反的顺序执行(从右到左)。loader 链中的第一个 loader 返回值给下一个loader。在最后一个 loader，返回 webpack 所预期的 JavaScript。
+ loader 可以是同步的，也可以是异步的。
+ loader 运行在 Node.js 中，并且能够执行任何可能的操作。
+ loader 接收查询参数。用于对 loader 传递配置。
+ loader 也能够使用 options 对象进行配置。
+ 除了使用 package.json 常见的 main 属性，还可以将普通的 npm 模块导出为 loader，做法是在 package.json 里定义一个 loader 字段。
+ 插件(plugin)可以为 loader 带来更多特性。
+ loader 能够产生额外的任意文件。


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

### bundle-loader and promise-loader
+ bundle-loader: 用于分离代码和延迟加载生成的 bundle。
+ promise-loader: 类似于 bundle-loader ，但是使用的是 promises。


## plugins
+ loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

### html-webpack-plugin
[html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin)
+ 该插件将为您生成一个HTML5文件，其中包含使用脚本标记的正文中的所有webpack包。
+ 它主要有两个作用：
1. 为html文件中引入的外部资源如script、link动态添加每次compile后的hash，防止引用缓存的外部文件问题
2. 可以生成创建html入口文件，比如单页面可以生成一个html文件入口，配置N个html-webpack-plugin可以生成N个页面入口

### clean-webpack-plugin
+ 由于过去的指南和代码示例遗留下来，导致我们的 /dist 文件夹相当杂乱。webpack 会生成文件，然后将这些文件放置在 /dist 文件夹中，但是 webpack 无法追踪到哪些文件是实际在项目中用到的。通常，在每次构建前清理 /dist 文件夹，是比较推荐的做法，因此只会生成用到的文件。

### ExtractTextPlugin
+ ExtractTextPlugin: 用于将 CSS 从主应用程序中分离

### CommonsChunkPlugin
+ **CommonsChunkPlugin** 插件可以将公共的依赖模块提取到已有的入口 chunk 中，或者提取到一个新生成的 chunk。

### webpack-dev-server
webpack-dev-server 为你提供了一个简单的 web 服务器，并且能够实时重新加载(live reloading)。
``` json
    devServer: {
        host: '0.0.0.0',
        contentBase: './src',
        inline: true,
        hot: true,
        port: 22222,
        proxy: {
            'home/page': {
                target: 'http://10.3.23.77:20086/',
                changeOrigin: true,
            },
        },
        quiet: true,
        overlay: {
            errors: true,
        },
    },
```

## 开发
### 使用 source map
+ 当 webpack 打包源代码时，可能会很难追踪到错误和警告在源代码中的原始位置。例如，如果将三个源文件（a.js, b.js 和 c.js）打包到一个 bundle（bundle.js）中，而其中一个源文件包含一个错误，那么堆栈跟踪就会简单地指向到 bundle.js。这并通常没有太多帮助，因为你可能需要准确地知道错误来自于哪个源文件。
+ 为了更容易地追踪错误和警告，JavaScript 提供了 source map 功能，将编译后的代码映射回原始源代码。如果一个错误来自于 b.js，source map 就会明确的告诉你。
+ 我们鼓励你在生产环境中启用 source map，因为它们对调试源码(debug)和运行基准测试(benchmark tests)很有帮助。虽然有如此强大的功能，然而还是应该针对生成环境用途，选择一个构建快速的推荐配置。

### 模块热替换
+ 块热替换(HMR - Hot Module Replacement)功能会在应用程序运行过程中替换、添加或删除模块，而无需重新加载整个页面。主要是通过以下几种方式，来显著加快开发速度：
1. 保留在完全重新加载页面时丢失的应用程序状态。
2. 只更新变更内容，以节省宝贵的开发时间。
3. 调整样式更加快速 - 几乎相当于在浏览器调试器中更改样式。

### tree shaking
+ tree shaking 是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。它依赖于 ES2015 模块系统中的静态结构特性，例如 import 和 export。
+ 为了学会使用 tree shaking，你必须……
1. 使用 ES2015 模块语法（即 import 和 export）。
2. 在项目 package.json 文件中，添加一个 "sideEffects" 入口。
3. 引入一个能够删除未引用代码(dead code)的压缩工具(minifier)（例如 **UglifyJSPlugin**）。

### 生产环境配置
+ 开发环境(development)和生产环境(production)的构建目标差异很大。在开发环境中，我们需要具有强大的、具有实时重新加载(live reloading)或热模块替换(hot module replacement)能力的 source map 和 localhost server。而在生产环境中，我们的目标则转向于关注更小的 bundle，更轻量的 source map，以及更优化的资源，以改善加载时间。由于要遵循逻辑分离，我们通常建议为每个环境编写 **彼此独立的 webpack 配置**。
+ 虽然，以上我们将生产环境和开发环境做了略微区分，但是，请注意，我们还是会遵循不重复原则(Don't repeat yourself - DRY)，保留一个“通用”配置。为了将这些配置合并在一起，我们将使用一个名为 **webpack-merge** 的工具。通过“通用”配置，我们不必在环境特定(environment-specific)的配置中重复代码。

### 代码分离
+ 代码分离是 webpack 中最引人注目的特性之一。此特性能够把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间。
+ 有三种常用的代码分离方法：
1. 入口起点：使用 entry 配置手动地分离代码。
2. 防止重复：使用 CommonsChunkPlugin 去重和分离 chunk。
3. 动态导入：通过模块的内联函数调用来分离代码。

### 懒加载
+ 懒加载或者按需加载，是一种很好的优化网页或应用的方式。这种方式实际上是先把你的代码在一些逻辑断点处分离开，然后在一些代码块中完成某些操作后，立即引用或即将引用另外一些新的代码块。这样加快了应用的初始加载速度，减轻了它的总体体积，因为某些代码块可能永远不会被加载。

### 缓存
+ 我们使用 webpack 来打包我们的模块化后的应用程序，webpack 会生成一个可部署的 /dist 目录，然后把打包后的内容放置在此目录中。只要 /dist 目录中的内容部署到服务器上，客户端（通常是浏览器）就能够访问网站此服务器的网站及其资源。而最后一步获取资源是比较耗费时间的，这就是为什么浏览器使用一种名为 缓存 的技术。可以通过命中缓存，以降低网络流量，使网站加载速度更快，然而，如果我们在部署新版本时不更改资源的文件名，浏览器可能会认为它没有被更新，就会使用它的缓存版本。由于缓存的存在，当你需要获取新的代码时，就会显得很棘手。
+ 通过使用 output.filename 进行文件名替换，可以确保浏览器获取到修改后的文件。[hash] 替换可以用于在文件名中包含一个构建相关(build-specific)的 hash，但是更好的方式是使用 [chunkhash] 替换，在文件名中包含一个 chunk 相关(chunk-specific)的哈希。