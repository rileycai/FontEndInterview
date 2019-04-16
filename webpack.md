## webpack相关面试题
参考：[webpack面试题汇总](https://www.jianshu.com/p/bb1e76edc71e)

[前端面试之webpack面试常见问题](https://segmentfault.com/a/1190000014148611)

### 什么是webpack和grunt和gulp有什么不同
Webpack是一个模块打包器，他可以递归的打包项目中的所有模块，最终生成几个打包后的文件。他和其他的工具最大的不同在于他支持code-splitting、模块化(AMD，ESM，CommonJs)、全局分析。

### 什么是bundle,什么是chunk，什么是module?
bundle是由webpack打包出来的文件，chunk是指webpack在进行模块的依赖分析的时候，代码分割出来的代码块。module是开发中的单个模块。

### 什么是Loader?什么是Plugin?
1）Loaders是用来告诉webpack如何转化处理某一类型的文件，并且引入到打包出的文件中
2）Plugin是用来自定义webpack打包过程的方式，一个插件是含有apply方法的一个对象，通过这个方法可以参与到整个webpack打包的各个流程(生命周期)。

### 配置问题:如何可以自动生成webpack配置？
webpack-cli /vue-cli /etc ...脚手架工具

### webpack-dev-server和http服务器如nginx有什么区别?
webpack-dev-server使用内存来存储webpack开发环境下的打包文件，并且可以使用模块热更新，他比传统的http服务对开发更加简单高效。

### 什么是模块热更新？
模块热更新是webpack的一个功能，他可以使得代码修改过后不用刷新浏览器就可以更新，是高级版的自动刷新浏览器。

### 什么是长缓存？在webpack中如何做到长缓存优化？
浏览器在用户访问页面的时候，为了加快加载速度，会对用户访问的静态资源进行存储，但是每一次代码升级或是更新，都需要浏览器去下载新的代码，最方便和简单的更新方式就是引入新的文件名称。在webpack中可以在output纵输出的文件指定chunkhash,并且分离经常更新的代码和框架代码。通过NameModulesPlugin或是HashedModuleIdsPlugin使再次打包文件名不变。

### 什么是webpack
webpack是一个打包模块化javascript的工具，在webpack里一切文件皆模块，通过loader转换文件，通过plugin注入钩子，最后输出由多个模块组合成的文件，webpack专注构建模块化项目。
WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

### 有哪些常见的Loader？他们是解决什么问题的？
+ file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
+ url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
+ source-map-loader：加载额外的 Source Map 文件，以方便断点调试
+ image-loader：加载并且压缩图片文件
+ babel-loader：把 ES6 转换成 ES5
+ css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
+ style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
+ eslint-loader：通过 ESLint 检查 JavaScript 代码

### webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全
Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：
+ 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
+ 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
+ 确定入口：根据配置中的 entry 找出所有的入口文件；
+ 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
+ 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
+ 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
+ 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

### 如何利用webpack来优化前端性能？
+ 压缩代码。删除多余的代码、注释、简化代码的写法等等方式。可以利用webpack的UglifyJsPlugin和ParallelUglifyPlugin来压缩JS文件， 利用cssnano（css-loader?minimize）来压缩css
+ 利用CDN加速。在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于output参数和各loader的publicPath参数来修改资源路径
+ 删除死代码（Tree Shaking）。将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数--optimize-minimize来实现
+ 提取公共代码。

### AMD和CMD的区别？
+ 最明显的区别就是在模块定义时对依赖的处理不同
+ AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块
+ CMD推崇就近依赖，只有在用到某个模块的时候再去require

参考：[前端模块化，AMD与CMD的区别](https://www.cnblogs.com/futai/p/5258349.html)
