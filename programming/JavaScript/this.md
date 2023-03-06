如果要判断一个运行中函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后

就可以顺序应用下面这四条规则来判断 this 的绑定对象。

1. 由 new 调用？绑定到新创建的对象。

2. 由 call 或者 apply （或者 bind ）调用？绑定到指定的对象。

3. 由上下文对象调用？绑定到那个上下文对象。

4. 默认：在严格模式下绑定到 undefined ，否则绑定到全局对象。

一定要注意，有些调用可能在无意中使用默认绑定规则。如果想“更安全”地忽略 this 绑

定，你可以使用一个 DMZ 对象，比如 ø = Object.create(null) ，以保护全局对象。

ES6 中的箭头函数并不会使用四条标准的绑定规则，而是根据当前的词法作用域来决定

this ，具体来说，箭头函数会继承外层函数调用的 this 绑定（无论 this 绑定到什么）。这

其实和 ES6 之前代码中的 self = this 机制一样。
```js
function foo() {
	console.log(this.bar)
}
var bar = "global";
var obj1 = {
	bar: "obj1",
	foo: foo
}
var obj2 = {
	bar: "obj2"
}

foo(); // global
obj1.foo(); // "obj1"
foo.call(obj2); // "obj2"
new foo(); // undefined
```

关于如何设置 this 有 4 条规则，上述代码中的最后 4 行展示了这 4 条规则。

(1) 在非严格模式下， foo() 最后会将 this 设置为全局对象。在严格模式下，这是未定义的行为，在访问 bar 属性时会出错——因此 "global" 是为 this.bar 创建的值。

(2) obj1.foo() 将 this 设置为对象 obj1 。

(3) foo.call(obj2) 将 this 设置为对象 obj2 。

(4) new foo() 将 this 设置为一个全新的空对象。

底线：为了搞清楚 this 指向什么，你必须检查相关的函数是如何被调用的。调用方式会是以上 4 种之一，这也会回答“ this 是什么”这个问题。