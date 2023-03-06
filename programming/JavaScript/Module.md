# 模块
- [AMD](https://en.wikipedia.org/wiki/Asynchronous_module_definition) —— 最古老的模块系统之一，最初由 [require.js](http://requirejs.org/) 库实现。
- [CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1) —— 为 Node.js 服务器创建的模块系统。
- [UMD](https://github.com/umdjs/umd) —— 另外一个模块系统，建议作为通用的模块系统，它与 AMD 和 CommonJS 都兼容。
- 现代 JavaScript 模块（module）
	```html
	<script type="module"></script>
	```

# what
一个模块（module）就是一个文件。一个脚本就是一个模块。就这么简单。

模块可以相互加载，并可以使用特殊的指令 `export` 和 `import` 来交换功能，从另一个模块调用一个模块的函数：

- `export` 关键字标记了可以从当前模块外部访问的变量和函数。
- `import` 关键字允许从其他模块导入功能。

>模块只通过 HTTP(s) 工作，而非本地（file）


# 特点
1. 模块始终在严格模式下运行。`use strict`
2. 每个模块都有自己的顶级作用域（top-level scope）。
	换句话说，一个模块中的顶级作用域变量和函数在其他脚本中是不可见的。
3. [模块代码仅在第一次导入时被解析](https://zh.javascript.info/modules-intro#mo-kuai-dai-ma-jin-zai-di-yi-ci-dao-ru-shi-bei-jie-xi)
	如果同一个模块被导入到多个其他位置，那么它的代码只会执行一次，即在第一次被导入时。然后将其导出（export）的内容提供给进一步的导入（importer）。
4. `import.meta` 对象包含关于当前模块的信息。
	它的内容取决于其所在的环境。在浏览器环境中，它包含当前脚本的 URL，或者如果它是在 HTML 中的话，则包含当前页面的 URL。
5. 在一个模块中，顶级 `this` 是 undefined。
6. 模块脚本 **总是** 被延迟的
	与 `defer` 特性（在 [脚本：async，defer](https://zh.javascript.info/script-async-defer) 一章中描述的）对外部脚本和内联脚本（inline script）的影响相同。
	- 下载外部模块脚本 `<script type="module" src="...">` 不会阻塞 HTML 的处理，它们会与其他资源并行加载。
	- 模块脚本会等到 HTML 文档完全准备就绪（即使它们很小并且比 HTML 加载速度更快），然后才会运行。
	- 保持脚本的相对顺序：在文档中排在前面的脚本先执行。
7. 浏览器中不支持裸模块
	没有任何路径的模块被称为“裸（bare）”模块。
	在浏览器中，`import` 必须给出相对或绝对的 URL 路径。
	```js
	import {sayHi} from 'sayHi'; // Error，“裸”模块 
	// 模块必须有一个路径，例如 './sayHi.js' 或者其他任何路径
```
	>某些环境，像 Node.js 或者打包工具（bundle tool）允许没有任何路径的裸模块，因为它们有自己的查找模块的方法和钩子（hook）来对它们进行微调。但是浏览器尚不支持裸模块。
8. 兼容性问题
	```html
	<script nomodule></script>
```


## 具有 `type="module"` 的外部脚本
1. 具有相同 `src` 的外部脚本仅运行一次
2. 从另一个源（例如另一个网站）获取的外部脚本需要 [CORS](https://developer.mozilla.org/zh/docs/Web/HTTP/CORS) header。
	换句话说，如果一个模块脚本是从另一个源获取的，则远程服务器必须提供表示允许获取的 header `Access-Control-Allow-Origin`。

## 导入导出
`import` 命名的导出时需要花括号，而 `import` 默认的导出时不需要花括号。