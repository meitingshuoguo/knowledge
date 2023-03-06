# HTML页面的生命周期
1. `DOMContentLoaded` —— 浏览器已完全加载 HTML，并构建了 DOM 树，但像 `<img>` 和样式表之类的外部资源可能尚未加载完成。
	Firefox，Chrome 和 Opera 都会在 `DOMContentLoaded` 中自动填充表单。
	
	```js
	document.addEventListener("DOMContentLoaded", () => { 
		alert("DOM ready!"); 
	});
	```

2. `load` —— 浏览器不仅加载完成了 HTML，还加载完成了所有外部资源：图片，样式等。`window` 对象上的 `load` 事件。

	```js
	window.onload = function() { 
		
	};
	//或者
	window.addEventListener('load', (event) => {
	
	}
	```

3. `beforeunload/unload` —— 都在`window` 对象上。
	1. `beforeunload` 当用户想要离开页面时。如果我们取消这个事件，浏览器就会询问我们是否真的要离开（例如，我们有未保存的更改）。
	2. `unload`当用户最终离开时。
		在处理程序中，我们只能执行不涉及延迟或询问用户的简单操作。正是由于这个限制，它很少被使用。我们可以使用 `navigator.sendBeacon` 来发送网络请求。


# 文档加载属性
`document.readyState` 属性可以为我们提供当前加载状态的信息。
1. `loading` —— 文档正在被加载。
2. `interactive` —— 文档被全部读取。
3. `complete` —— 文档被全部读取，并且所有资源（例如图片等）都已加载完成。


 `readystatechange` 事件，会在状态发生改变时触发。


# DOM 变动观察器
`MutationObserver` 是一个内建对象，它观察 DOM 元素，并在检测到更改时触发回调。可以对 DOM 的变化作出反应 —— 特性（attribute），文本内容，添加/删除元素。