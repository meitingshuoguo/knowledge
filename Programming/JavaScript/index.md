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



