是一种用于按行或按列布局元素的一维布局方法 。元素可以膨胀以填充额外的空间, 收缩以适应更小的空间。

1. 将一个元素先指定为`flex`容器
	- 块级元素 `display: flex`
	- 内联元素 `display: inline-flex`

2. flex容器上可以指定的属性
	- `flex-direction` 主轴的方向。
	- `flex-wrap` 默认全部排列在一行，定义换行方式。
	- `flex-flow` 是`flex-direction` `flex-wrap` 的简写形式
	- `justify-content` flex项在主轴上的对齐方式。
	- `align-items` flex项在交叉轴上的对齐方式。
	- `align-content` 指如果多于一行，行与行之间的对齐方式。（在交叉轴上）

3. flex项上可以指定的属性
	- `order` 定义flex项的排列顺序。数值越小，排列越靠前主轴起点。默认为0。
	- `flex-grow` 设定flex项的扩大比例，默认为0，即不扩大。
		当存在剩余空间（flex容器的宽度大于flex项宽度的总和，也可能是高度，取决于`flex-direction`的值）的时候，将剩余空间的值除以总的`flex-grow`的合得到的值即为1份`flex-grow`的值，然后看每个flex项的`flex-grow`值是多少，就为其再加上几份宽度或者高度。
	- `flex-shrink` 设定flex项的缩小比例，默认为1。设为0即不缩小。
		当flex项加起来的大小超出flex容器后该如何改变其大小（或者说叫flex项如何分担超出flex容器的那部分空间）。默认是1，即等比缩小，每个flex项承担超出空间的1/n（n指flex项的个数）。如果为0，则该flex项不承担分配任务，保持原本大小。
	- `flex-basis` 定义flex项在主轴方向的大小。如果主轴是`row`则是设定其宽度。如果是`column`则是其高度）默认值为`auto`。在设置其他值后原来的`width`或`height`设置将失效。
	- `flex` 是`flex-grow` `flex-shrink` `flex-basis`的简写形式。
	- `align-self` 允许单个flex项与其他flex项有不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性。

[详见](https://www.youtube.com/watch?v=5vrpaxLTsTo)