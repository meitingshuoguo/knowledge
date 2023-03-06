由 `new Promise` 构造器返回的 `promise` 对象具有以下内部属性：
-   `state` —— 最初是 `"pending"`，然后在 `resolve` 被调用时变为 `"fulfilled"`，或者在 `reject` 被调用时变为 `"rejected"`。
-   `result` —— 最初是 `undefined`，然后在 `resolve(value)` 被调用时变为 `value`，或者在 `reject(error)` 被调用时变为 `error`。
- `resolve/reject` 只需要一个参数（或不包含任何参数），并且将忽略额外的参数。

所以，executor 最终将 `promise` 移至以下状态之一：

![Pasted image 20220411203845](/Users/hefkang/Desktop/my-sync/knowledge/programming/JavaScript/assets/Pasted image 20220411203845.png)

# `.then` 
- 第一个参数是一个函数，该函数将在 promise resolved 后运行并接收结果。
- 第二个参数也是一个函数，该函数将在 promise rejected 后运行并接收 error。
# `.catch`
-   `.catch` 处理 promise 中的各种 error：在 `reject()` 调用中的，或者在处理程序（handler）中抛出的（thrown）error。
- 如果我们只对 error 感兴趣，那么我们可以使用 `null` 作为第一个参数：`.then(null, errorHandlingFunction)`。或者我们也可以使用 `.catch(errorHandlingFunction)`，其实是一样的：
- promise对象的 `.catch(f)` 调用是 `.then(null, f)` 的完全的模拟，它只是一个简写形式。

# 错误处理
```js
window.addEventListener('unhandledrejection', function(event) { 
	// 这个事件对象有两个特殊的属性： 
	alert(event.promise); // [object Promise] - 生成该全局 error 的 promise 
	alert(event.reason); // Error: Whoops! - 未处理的 error 对象 
});
new Promise(function() {
	throw new Error("Whoops!"); 
}); // 没有用来处理 error 的 catch
```
如果出现了一个 error，并且在这儿没有 `.catch`，那么 `unhandledrejection` 处理程序（handler）就会被触发，并获取具有 error 相关信息的 `event` 对象，所以我们就能做一些后续处理了。
通常此类 error 是无法恢复的，所以我们最好的解决方案是将问题告知用户，并且可以将事件报告给服务器。

# 方法
1. `.all`
	- 当所有给定的 `promise` 都 `resolve` 时，新的 `promise` 才会 `resolve`，并且返回每个结果组成的数组。结果数组中元素的顺序与其在源 `promise` 中的顺序相同。
	- 如果任意一个 `promise` 被 `reject`，由 `Promise.all` 返回的 `promise` 就会立即 `reject`，完全忽略列表中其他的 `promise` 和它们的结果。并且最终返回的就是这个 `error`。
	- 通常，`Promise.all(...)` 接受含有 `promise` 项的可迭代对象（大多数情况下是数组）作为参数。但是，如果这些对象中的任何一个不是 `promise`，那么它将被“按原样”传递给结果数组。
2. `.allSettled`
	- 等待所有的 `promise` 都被 `settle`，无论结果如何，最终对于每个 `promise` 都得到了其`status`和 `value/reason`。
	- 返回一个包含`status`和 `value/reason`的结果数组。
3. `.race`
	- 只等待第一个 `settled` 的 `promise` 并获取其结果（或 `error`）。
4. `.any`
	- 只等待第一个 `resolve` 的 `promise`，并将这个 `resolve` 的 `promise` 返回。如果给出的 `promise` 都 `rejected`，那么则返回 `rejected` 的 `promise` 和 [`AggregateError`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/AggregateError) 错误类型的 `error` 实例—— 一个特殊的 `error` 对象，在其 `errors` 属性中存储着所有 `promise error`。
5. `.resolve` 
	- 用结果 `value` 创建一个 `resolved` 的 `promise`。
6. `.reject`
	- 用 `error` 创建一个 `rejected` 的 `promise`。


# async
1.  让这个函数总是返回一个 promise。函数的返回值将自动被包装在一个 resolved 的 promise 中
2.  允许在该函数内使用 `await`。

# await
- 只在 async 函数内工作，放在一个promise前面。
- `await` 实际上会暂停函数的执行，直到 promise 状态变为 settled，然后返回 promise 的结果。
- 这个行为不会耗费任何 CPU 资源，因为 JavaScript 引擎可以同时处理其他任务：执行其他脚本，处理事件等。
- 相比于 `promise.then`，它只是获取 promise 的结果的一个更优雅的语法。并且也更易于读写。