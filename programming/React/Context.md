# what
Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。  
意思便是context能够不必显式地通过组件树的逐层传递 props，可以跨层级进行数据传递。
```js
	const MyContext = React.createContext(defaultValue);
	<MyContext.Provider value={/* 某个值 */}>
	static contextType = MyContext;
	<MyContext.Consumer>
	  {value => /* 基于 context 值进行渲染*/}
	</MyContext.Consumer>