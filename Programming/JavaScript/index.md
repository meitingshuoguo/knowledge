# 关于对象

- 使用构造函数声明 ` let user = new Object()`
- 使用字面量声明  `let user = {}`
- 对象的key只能是字符串类型或者Symbol类型
- 对象的原始值转换只有数值转换（对象相减或应用数学函数时）和字符串转换（类似alert()这种）

**为了进行转换，JavaScript 尝试查找并调用三个对象方法：**（hint是指对象到什么类型的转换）

1. 调用 `obj[Symbol.toPrimitive](hint)` —— 带有 symbol 键 `Symbol.toPrimitive`（系统 symbol）的方法，如果这个方法存在的话，

2. 否则，如果 hint 是 `"string"` —— 尝试 `obj.toString()` 和 `obj.valueOf()`，无论哪个存在。

3. 否则，如果 hint 是 `"number"` 或 `"default"` —— 尝试 `obj.valueOf()` 和 `obj.toString()`，无论哪个存在。

   ```javascript
   
   let user = {
     name: "John",
     money: 1000,
     [Symbol.toPrimitive](hint) {
       console.log(hint);
       return hint === "string" ? `{name:${this.name}}` : this.money;
     },
   };
   alert(user);
   alert(+user);
   alert(user + 500);
   ```

   

# 关于原型链

##### 概念

- JS中的对象有一个特殊的隐藏属性`[[Prototype]]`，如果对象A的这个特殊属性的值是另一 个对象B，那我们就可以说对象B是对象A的**原型**。而当我们从对象A中读取一个缺失的属性时，JS会自动从对象A的原型（就是上文的对象B）中获取该属性（**原型继承**），如果在B中也没有找到，那就去B的原型C去找，一直找到这个**原型链**中某个对象的原型对象为null时停止，因为根据定义null没有原型。
   - 原型继承不能形成闭环，也就是相互引用。这样JS会抛出错误
   - `__proto__` 的值可以是对象，也可以是 `null`。而其他的类型都会被忽略。
   - 只能有一个 `[[Prototype]]`。一个对象不能从其他两个对象获得继承。
   - 原型仅用于**读取**属性
   - `this` 根本不受原型的影响。**无论在哪里找到方法：在一个对象还是在原型中。在一个方法调用中，`this` 始终是点符号 `.` 前面的对象。**
   - 当继承的对象运行继承的方法时，它们将仅修改自己的状态，而不会修改大对象的状态。（方法是共享的，但对象状态不是）
   - `for..in` 循环也会迭代继承的属性。所以可以用`obj.hasOwnProperty(prop)`来检测



- 属性 `[[Prototype]]` 是内部的而且是隐藏的，但是有很多设置它的方式：

  	- 使用特殊的名字 `__proto__`（非标准但许多浏览器实现了）
  	- ES6开始可以通过 [`Object.getPrototypeOf()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/GetPrototypeOf) 和 [`Object.setPrototypeOf()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf) 这两个方法来访问

  > `__proto__` 与内部的 `[[Prototype]]` **不一样**。`__proto__` 是 `[[Prototype]]` 的 getter/setter。`__proto__` 属性有点过时了。它的存在是出于历史的原因。

  ```javascript
  const A = {
    name:'jack'
  }
  const B = {
    age:12
  }
  
  A.__proto__ = B
  console.log(A.age,A.__proto__,Object.getPrototypeOf(A))
  ```

题目

1. 我们有从 `animal` 中继承的 `rabbit`。

   如果我们调用 `rabbit.eat()`，哪一个对象会接收到 `full` 属性：`animal` 还是 `rabbit`？

   ```javascript
   let animal = {
     eat() {
       this.full = true;
     }
   };
   
   let rabbit = {
     __proto__: animal
   };
   
   rabbit.eat();
   ```

   <details>
     答案：`rabbit`。
     这是因为 `this` 是点符号前面的这个对象，因此 `rabbit.eat()` 修改了 `rabbit`。
     属性查找和执行是两回事儿。首先在原型中找到 `rabbit.eat` 方法，然后在 `this=rabbit` 的情况下执行。
   </details>



# 关于this的问题

1. this的值是在调用的时候才被计算出来的。

   - 作为点符号被作为方法调用，指向的就是点前面的对象。
   - 作为函数被调用，指向的就是当前函数的this。

   ```javascript
   let user = {
     name: "John",
     say() {
       return this.name;
     },
   };
   console.log(user.say());
   
   function makeUser() {
     return {
       name: "John",
       ref: this,
     };
   }
   let user = makeUser();
   console.log(user.ref.name);
   ```

   

# 关于Symbol

- 表示唯一的标识符

- 相同的描述（Symbol名）也是不同的值

- 有方法`toString`和属性`description`

- 在`for in`中会被跳过，`Object.keys()`也会忽略，但`Object.assign()`会同时复制字符串和symbol属性。

- 

  ```javascript
  // 对象字面量中使用
  let age = Symbol()
  let user = {
    name:'Tom',
    [age]: 23
  }
  console.log(user[age])
  
  // 从全局注册表中读取
  let id = Symbol.for("id"); // 如果该 Symbol 不存在，则创建它
  
  // 再次读取（可能是在代码中的另一个位置）
  let idAgain = Symbol.for("id");
  
  // 相同的 Symbol
  alert( id === idAgain ); // true
  
  // 通过 name 获取 Symbol
  let sym = Symbol.for("name");
  let sym2 = Symbol.for("id");
  
  // 通过 Symbol 获取 name
  alert( Symbol.keyFor(sym) ); // name
  alert( Symbol.keyFor(sym2) ); // id
  
  ```



## 

**[总结](https://zh.javascript.info/symbol#zong-jie)**

`Symbol` 是唯一标识符的基本类型

Symbol 是使用带有可选描述（name）的 `Symbol()` 调用创建的。

Symbol 总是不同的值，即使它们有相同的名字。如果我们希望同名的 Symbol 相等，那么我们应该使用全局注册表：`Symbol.for(key)` 返回（如果需要的话则创建）一个以 `key` 作为名字的全局 Symbol。使用 `Symbol.for` 多次调用 `key` 相同的 Symbol 时，返回的就是同一个 Symbol。

Symbol 有两个主要的使用场景：

1. “隐藏” 对象属性。 如果我们想要向“属于”另一个脚本或者库的对象添加一个属性，我们可以创建一个 Symbol 并使用它作为属性的键。Symbol 属性不会出现在 `for..in` 中，因此它不会意外地被与其他属性一起处理。并且，它不会被直接访问，因为另一个脚本没有我们的 symbol。因此，该属性将受到保护，防止被意外使用或重写。

   因此我们可以使用 Symbol 属性“秘密地”将一些东西隐藏到我们需要的对象中，但其他地方看不到它。

2. JavaScript 使用了许多系统 Symbol，这些 Symbol 可以作为 `Symbol.*` 访问。我们可以使用它们来改变一些内置行为。例如，在本教程的后面部分，我们将使用 `Symbol.iterator` 来进行 [迭代](https://zh.javascript.info/iterable) 操作，使用 `Symbol.toPrimitive` 来设置 [对象原始值的转换](https://zh.javascript.info/object-toprimitive) 等等。

从技术上说，Symbol 不是 100% 隐藏的。有一个内置方法 [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) 允许我们获取所有的 Symbol。还有一个名为 [Reflect.ownKeys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) 的方法可以返回一个对象的 **所有** 键，包括 Symbol。所以它们并不是真正的隐藏。但是大多数库、内置方法和语法结构都没有使用这些方法。

