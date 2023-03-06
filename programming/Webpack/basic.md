# 除了Webpack外你还用过哪些构建工具？   
	`Vite`：基于原生ESM，底层用了`esbuild`（只做打包和压缩文件的工作）

1.  Webpack Dev Server 在启动时，需要先打包—遍，然后启动开发服务器，这一过程是需要耗费很多时间的。而vite是直接启动Server，并不会先编译所有的代码文件
2.  在进行热更新时，Webpack 修改某个文件过后，会自动以这个文件为入口重写 build—次，所有的涉及到的依赖也都会被加载一遍，所以反应速度会慢很多。而Vite 只需要立即编译当前所修改的文件即可，所以 响应速度非常快      
3.  Webpack 工具的做法是将所有模块提前编译，不管模块是否会被执行，都要被编译和打包到 bundle 里。随着项目越来越大打包后的 bundle 也越来越大，打包的速度自然也就越来越慢。而Vite 利用现代浏览器原生支持 ESM 特性，省略了对模块的打包。也就意味着不需要分析模块的依赖、不需要编译，只有具体去请求某个模块时才会编译这个文件，实现真正的按需编译！


# 基本概念
- `Entry`(入口)：入口起点_告诉 webpack _从哪里开始_，并根据依赖关系图确定**需要打包的文件内容**
- `Loader`(加载器)：webpack 将所有的资源（css, js, image 等）都看做模块，但是 webpack 能处理的只是 JavaScript，因此，需要存在一个能将其他资源转换为模块，让 webpack 能将其加入依赖树中的东西，它就是 loader。loader用于对模块的源代码进行转换。loader是一个导出为function的node模块。可以将匹配到的文件进行一次转换，同时loader可以链式传递。
- `Plugins`(插件)：loader 只能针对某种特定类型的文件进行处理，而 plugin 的功能则更为强大。在 plugin 中能够介入到整个 webpack 编译的生命周期，Plugins用于解决 loader 无法实现的其他事情，也就是说loader是预处理文件，那plugin 就是后处理文件。
		-   对loader打包后的模块文件（bundle.js）进行二次优化处理，例如：代码压缩从而减小文件体积
		-   提供辅助开发的作用：例如：热更新（浏览器实时显示）
- `Module`（模块）：在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。
- `Chunk`（代码块）：一个 Chunk **由多个模块组合而成**，用于代码合并与分割。
- `output`（输出）：告诉webpack在哪里输出以及如何命名它所创建的bundles。在配置中通过配置output属性指定



# Webpack的构建流程是什么  
![](Pasted%20image%2020220518121836.png)
	1.  初始化参数：从配置文件和 Shell 语句中读取与合并参数,得出最终的参数。
	2.  开始编译：用上一步得到的参数初始化 Compiler 对象,加载所有配置的插件,执行对象的 run 方法开始执行编译。
	3.  确定入口：根据配置中的 entry 找出所有的入口文件。
	4.  编译模块：从入口文件出发,调用所有配置的 Loader 对模块进行翻译,再找出该模块依赖的模块,再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理。
	5.  完成模块编译：在经过第 4 步使用 Loader 翻译完所有模块后,得到了每个模块被翻译后的最终内容以及它们之间的依赖关系。
	6.  输出资源：根据入口和模块之间的依赖关系,组装成一个个包含多个模块的 Chunk,再把每个 Chunk 转换成一个单独的文件加入到输出列表,这步是可以修改输出内容的最后机会。
	7.  输出完成：在确定好输出内容后,根据配置确定输出的路径和文件名,把文件内容写入到文件系统。


# 有哪些常见的Loader？他们是解决什么问题的？  
- 处理样式：style-loader（把 `CSS` 插入到 `DOM` 中）、css-loader（对 `@import` 和 `url()` 进行处理,并将结果作为一个js模块返回）、less-loader(将 Less 编译为 CSS)、sass-loader。postcss-loader: 用postcss来处理CSS等
- 处理文件：raw-loader（原文件所有的内容作为一个字符串导出）、file-loader（修改打包后图片的储存路径，再根据配置修改我们引用的路径。返回的是文件的路径） 、url-loader（会将引入的文件进行编码，生成 DataURL，返回的是文件的base64编码。1.  文件大小小于limit参数，url-loader将会把文件转为DataURL；2.  文件大小大于limit，url-loader会调用file-loader进行处理，参数也会直接传给file-loader。）
- 编译：babel-loader（将Es6+ 语法转换为Es5语法。）、ts-loader等
- 校验测试：eslint-loader等

# 有哪些常见的Plugin？他们是解决什么问题的？  
- HtmlWebpackPlugin（在打包结束后，⾃动生成⼀个 `html` ⽂文件，并把打包生成的`js` 模块引⼊到该 `html` 中）
- clean-webpack-plugin（删除（清理）构建目录）
- mini-css-extract-plugin（提取 `CSS` 到一个单独的文件中）
- [HotModuleReplacementPlugin](https://yesifang.com/zh/Webpack%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B/54896/#HotModuleReplacementPlugin)配置`webpack-dev-server`实现HMR（模块的热替换）
- [SplitChunksPlugin](https://yesifang.com/zh/Webpack%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B/54896/#SplitChunksPlugin)分割代码chunk为一个个独立的文件，从而提升客户端访问时加载的性能。


# Loader 和 Plugin 有什么差别  
- loader 是文件加载器，能够加载资源文件，并对这些文件进行一些处理，诸如编译、压缩等，最终一起打包到指定的文件中
- plugin 赋予了 webpack 各种灵活的功能，例如打包优化、资源管理、环境变量注入等，目的是解决 loader 无法实现的其他事
- loader 运行在打包文件之前
- plugins 在整个编译周期都起作用

# compiler与complilation有什么区别？
- `Compiler` 对象包含了当前运行`Webpack`的配置，包括`entry、output、loaders`等配置，这个对象在启动`Webpack`时被实例化，而且是全局唯一的。`Plugin`可以通过该对象获取到Webpack的配置信息进行处理。
- `Compilation`对象代表了一次资源版本构建。当运行 `webpack` 开发环境中间件时，每当检测到一个文件变化，就会创建一个新的 `compilation`，从而生成一组新的编译资源。一个 `Compilation` 对象表现了当前的模块资源、编译生成资源、变化的文件、以及被跟踪依赖的状态信息，简单来讲就是把本次打包编译的内容存到内存里。`Compilation` 对象也提供了插件需要自定义功能的回调，以供插件做自定义处理时选择使用拓展。
	简单来说,`Compilation`的职责就是构建模块和Chunk，并利用插件优化构建过程。


# 有哪些代码分离的方法  
-   **入口起点**：使用 `entry` 配置手动地分离代码。
-   **防止重复**：使用 `Entry dependencies` 或者 `SplitChunksPlugin` 去重和分离`chunk`。
-   **动态导入**：通过模块的内联函数 `import` 调用来分离代码。


# 什么是 Tree Shaking  
 `Tree Shaking` 是一个术语，在计算机中表示消除死代码，依赖于`ES Module`的静态语法分析（不执行任何的代码，可以明确知道模块的依赖关系）
在`webpack`实现`Trss shaking`有两种不同的方案：
-   usedExports：通过标记某些函数是否被使用，之后通过Terser来进行优化的
-   sideEffects：跳过整个模块/文件，直接查看该文件是否有副作用


# 如何利用Webpack来优化前端性能  
- JS代码压缩
- CSS代码压缩
- Html文件代码压缩
- 文件大小压缩
- 图片压缩
- Tree Shaking
- 代码分离
- 内联 chunk

# 如何提高Webpack的构建速度  
- 优化 loader 配置
- 合理使用 resolve.extensions
- 优化 resolve.modules
- 优化 resolve.alias
- 使用 DLLPlugin 插件
- 使用 cache-loader
- terser 启动多线程
- 合理使用 sourceMap

优化搜索时间、缩小文件搜索范围、减少不必要的编译等方面入手

# 打包文件大怎么解决  
- `prod`环境下关闭`source-map`。
- 剥离css文件，单独打包
- 提取公共依赖`CommonsChunkPlugin`
- 开启gzip压缩`compression-webpack-plugin`
- cache-loader，也能开启缓存，用法非常简单，在开销较大的loader前使用即可