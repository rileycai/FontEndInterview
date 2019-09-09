## webpack相关面试题
参考：[webpack面试题汇总](https://www.jianshu.com/p/bb1e76edc71e)

[前端面试之webpack面试常见问题](https://segmentfault.com/a/1190000014148611)

### 1. 什么是webpack
webpack是一个打包模块化javascript的工具，在webpack里一切文件皆模块，通过loader转换文件，通过plugin注入钩子，最后输出由多个模块组合成的文件，webpack专注构建模块化项目。
WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

### 2. webpack与grunt、gulp的不同？
+ **Grunt、Gulp是基于任务运行的工具：** 它们会自动执行指定的任务，就像流水线，把资源放上去然后通过不同插件进行加工，它们包含活跃的社区，丰富的插件，能方便的打造各种工作流。
+ **Webpack是基于模块化打包的工具:** 自动化处理模块,webpack把一切当成模块，当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。
+ 因此这是完全不同的两类工具,而现在主流的方式是用npm script代替Grunt、Gulp,npm script同样可以打造任务流。

### 3.什么是bundle,什么是chunk，什么是module?
+ bundle：是由webpack打包出来的文件
+ chunk：代码块，一个chunk由多个模块组合而成，用于代码的合并和分割
+ module：是开发中的单个模块，在webpack的世界，一切皆模块，一个模块对应一个文件，webpack会从配置的entry中递归开始找出所有依赖的模块

### 4.什么是Loader?什么是Plugin?
1）Loaders是用来告诉webpack如何转化处理某一类型的文件，并且引入到打包出的文件中
2）Plugin是用来自定义webpack打包过程的方式，一个插件是含有apply方法的一个对象，通过这个方法可以参与到整个webpack打包的各个流程(生命周期)。

### 5. 有哪些常见的Loader？他们是解决什么问题的？
+ file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
+ url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
+ source-map-loader：加载额外的 Source Map 文件，以方便断点调试
+ image-loader：加载并且压缩图片文件
+ babel-loader：把 ES6 转换成 ES5
+ css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
+ style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
+ eslint-loader：通过 ESLint 检查 JavaScript 代码

### 6. 有哪些常见的Plugin？
+ define-plugin：定义环境变量
+ html-webpack-plugin：简化html文件创建
+ uglifyjs-webpack-plugin：通过UglifyES压缩ES6代码
+ webpack-parallel-uglify-plugin: 多核压缩,提高压缩速度
+ webpack-bundle-analyzer: 可视化webpack输出文件的体积
+ mini-css-extract-plugin: CSS提取到单独的文件中,支持按需加载

### 7. Loader和Plugin的不同？
+ Loader直译为"加载器"。Webpack将一切文件视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析_非JavaScript文件_的能力。
+ Plugin直译为"插件"。Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
+ Loader在module.rules中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）
+ Plugin在plugins中单独配置。 类型为数组，每一项是一个plugin的实例，参数都通过构造函数传入。

### 8. 配置问题:如何可以自动生成webpack配置？
webpack-cli /vue-cli /etc ...脚手架工具

### 9. webpack-dev-server和http服务器如nginx有什么区别?
webpack-dev-server使用内存来存储webpack开发环境下的打包文件，并且可以使用模块热更新，他比传统的http服务对开发更加简单高效。

### 10. 什么是模块热更新？
模块热更新是webpack的一个功能，他可以使得代码修改过后不用刷新浏览器就可以更新，是高级版的自动刷新浏览器。

### 11. 什么是长缓存？在webpack中如何做到长缓存优化？
浏览器在用户访问页面的时候，为了加快加载速度，会对用户访问的静态资源进行存储，但是每一次代码升级或是更新，都需要浏览器去下载新的代码，最方便和简单的更新方式就是引入新的文件名称。在webpack中可以在output纵输出的文件指定chunkhash,并且分离经常更新的代码和框架代码。通过NameModulesPlugin或是HashedModuleIdsPlugin使再次打包文件名不变。

### 12.webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全
Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：
+ 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
+ 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
+ 确定入口：根据配置中的 entry 找出所有的入口文件；
+ 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
+ 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
+ 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
+ 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

### 13. 是否写过Loader和Plugin？描述一下编写loader或plugin的思路？
+ Loader像一个"翻译官"把读到的源文件内容转义成新的文件内容，并且每个Loader通过链式操作，将源文件一步步翻译成想要的样子。
+ 编写Loader时要遵循单一原则，每个Loader只做一种"转义"工作。 每个Loader的拿到的是源文件内容（source），可以通过返回值的方式将处理后的内容输出，也可以调用this.callback()方法，将内容返回给webpack。 还可以通过 this.async()生成一个callback函数，再用这个callback将处理后的内容输出出去。 此外webpack还为开发者准备了开发loader的工具函数集——loader-utils。
+ 相对于Loader而言，Plugin的编写就灵活了许多。webpack在运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

### 14.如何利用webpack来优化前端性能？
+ 压缩代码。删除多余的代码、注释、简化代码的写法等等方式。可以利用webpack的UglifyJsPlugin和ParallelUglifyPlugin来压缩JS文件， 利用cssnano（css-loader?minimize）来压缩css
+ 利用CDN加速。在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于output参数和各loader的publicPath参数来修改资源路径
+ 删除死代码（Tree Shaking）。将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数--optimize-minimize来实现
+ 提取公共代码。

### 15. webpack、rollup、parcel优劣？
+ webpack适用于大型复杂的前端站点构建: webpack有强大的loader和插件生态,打包后的文件实际上就是一个立即执行函数，这个立即执行函数接收一个参数，这个参数是模块对象，键为各个模块的路径，值为模块内容。立即执行函数内部则处理模块之间的引用，执行模块等,这种情况更适合文件依赖复杂的应用开发.
+ rollup适用于基础库的打包，如vue、d3等: Rollup 就是将各个模块打包进一个文件中，并且通过 Tree-shaking 来删除无用的代码,可以最大程度上降低代码体积,但是rollup没有webpack如此多的的如代码分割、按需加载等高级功能，其更聚焦于库的打包，因此更适合库的开发.
+ parcel适用于简单的实验性项目: 他可以满足低门槛的快速看到效果,但是生态差、报错信息不够全面都是他的硬伤，除了一些玩具项目或者实验项目不建议使用

### 16. 如何提高webpack的打包速度?
+ happypack: 利用进程并行编译loader,利用缓存来使得 rebuild 更快,遗憾的是作者表示已经不会继续开发此项目,类似的替代者是thread-loader
+ 外部扩展(externals): 将不怎么需要更新的第三方库脱离webpack打包，不被打入bundle中，从而减少打包时间,比如jQuery用script标签引入。
+ dll: 采用webpack的 DllPlugin 和 DllReferencePlugin 引入dll，让一些基本不会改动的代码先打包成静态资源,避免反复编译浪费时间
+ 利用缓存: webpack.cache、babel-loader.cacheDirectory、HappyPack.cache都可以利用缓存提高rebuild效率
+ 缩小文件搜索范围: 比如babel-loader插件,如果你的文件仅存在于src中,那么可以include: path.resolve(__dirname, 'src'),当然绝大多数情况下这种操作的提升有限,除非不小心build了node_modules文件

### 17. 如何提高webpack的构建速度？
1. 多入口情况下，使用CommonsChunkPlugin来提取公共代码
2. 通过externals配置来提取常用库
3. 利用DllPlugin和DllReferencePlugin预编译资源模块 通过DllPlugin来对那些我们引用但是绝对不会修改的npm包来进行预编译，再通过4. DllReferencePlugin将预编译的模块加载进来。
4. 使用Happypack 实现多线程加速编译
5. 使用webpack-uglify-parallel来提升uglifyPlugin的压缩速度。 原理上webpack-uglify-parallel采用了多核并行压缩来提升压缩速度
6. 使用Tree-shaking和Scope Hoisting来剔除多余代码

### 18. AMD和CMD的区别？
+ 最明显的区别就是在模块定义时对依赖的处理不同
+ AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块
+ CMD推崇就近依赖，只有在用到某个模块的时候再去require

参考：[前端模块化，AMD与CMD的区别](https://www.cnblogs.com/futai/p/5258349.html)

### 19. 介绍模块化发展历程
+ 模块化主要是用来抽离公共代码，隔离作用域，避免变量冲突等。
1. IIFE： 使用自执行函数来编写模块化，特点：在一个单独的函数作用域中执行代码，避免变量冲突。
2. AMD： 使用requireJS 来编写模块化，特点：依赖必须提前声明好。
3. CMD： 使用seaJS 来编写模块化，特点：支持动态引入依赖文件。
4. CommonJS： nodejs 中自带的模块化。
5. UMD：兼容AMD，CommonJS 模块化语法。
6. webpack(require.ensure)：webpack 2.x 版本中的代码分割。
7. ES Modules： ES6 引入的模块化，支持import 来引入另一个 js 。