背景：常规函数只会返回一个单一值（或者不返回任何值）。
# 生成（yield）值的函数
- `function`关键字后跟 `*`。内部有关键字 `yield`
- 被调用时，它不会运行其代码。而是返回一个被称为 “generator object” 的特殊对象，来管理执行流程。
- 该对象有`next`方法，执行后函数恢复运行，执行直到最近的`yield`，并将`yield`右边的值返回到外部，然后又暂停函数执行。
- 该对象还有`throw`方法，`return`方法
- `next`方法始终返回一个对象，内有`value`和`done`这两个属性。
- `next`方法第一次执行不接受参数，会被忽略。之后执行接收到的参数会称为`yield`的结果（换句话说这个传入的东西就是`yield`左边的东西）

## Generator 组合（composition）
是将一个 generator 流插入到另一个 generator 流的自然的方式。它不需要使用额外的内存来存储中间结果。
```js
function* generate(start, end) {
	for (let i = start; i <= end; i++) yield i;
}

function* generatePwd() {
	// 0-9
	yield* generate(48, 57);
	// 可以替换成 for (let i = 48; i <= 57; i++) yield i;
	// A-Z
	yield* generate(65, 90);
	// a-z
	yield* generate(97, 122);
}

let str = "";

for (let code of generatePwd()) {
	console.log(code);
	str += String.fromCharCode(code);
}
console.log(str);
```

>`yield*` 指令将执行 **委托** 给另一个 generator。这个术语意味着 `yield* gen` 在 generator `gen` 上进行迭代，并将其产出（yield）的值透明地（transparently）转发到外部。就好像这些值就是由外部的 generator yield 的一样。

 `yield` 是一条双向路（two-way street）：
 1. 可以向外返回结果。
 2. 还可以将外部的值传递到 generator 内。
	调用 `generator.next(arg)`，我们就能将参数 `arg` 传递到 generator 内部。这个 `arg` 参数会变成 `yield` 的结果。