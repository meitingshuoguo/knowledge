是一个用于web的二维布局系统。利用网格，你可以把内容按照行与列的格式进行排版。

1. 将一个元素指定为grid容器
	- 块级元素 `display: grid`
	- 内联元素 `display: inline-grid`
	
	>设为网格布局以后，容器子元素（项目）的 `float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。

2. grid容器上可以指定的[属性](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)
	- `grid-template-columns` 定义每一列的列宽
	- `grid-template-rows` 定义每一行的行高
	- `row-gap` 属性设置行与行的间隔（行间距）
	- `column-gap` 属性设置列与列的间隔（列间距）
	- `gap` 是`column-gap`和`row-gap`的合并简写形式
	- `grid-template-areas` 属性用于定义区域。
	- `grid-auto-flow` 默认值是`row`，即"先行后列"。也可以将它设成`column`，变成"先列后行"。
	- `justify-items` 属性设置单元格内容的水平位置（左中右）
	- `align-items` 属性设置单元格内容的垂直位置（上中下）。
	- `place-items` 属性是`align-items` 属性和`justify-items` 属性的合并简写形式。
	- `justify-content` 属性是整个内容区域在容器里面的水平位置（左中右）
	- `align-content` 属性是整个内容区域的垂直位置（上中下）。
	- `place-content` 属性是`align-content` 属性和`justify-content` 属性的合并简写形式。
	- `grid-auto-columns`属性和`grid-auto-rows`属性用来设置浏览器自动创建的多余网格的列宽和行高。它们的写法与`grid-template-columns`和`grid-template-rows`完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。
	
3. grid项上可以指定的属性
	- `grid-column-start` grid项左边框所在的垂直网格线
	- `grid-column-end` grid项右边框所在的垂直网格线
	- `grid-row-start` grid项上边框所在的水平网格线
	- `grid-row-end` grid项下边框所在的水平网格线
	- `grid-column` 是 `grid-column-start`和`grid-column-end`的合并简写形式
	- `grid-row` 是 `grid-row-start` 和 `grid-row-end` 的合并简写形式
	- `grid-area` 指定项目放在哪一个区域
	- `justify-self` 设置单元格内容的水平位置（左中右），跟`justify-items`属性的用法完全一致，但只作用于单个项目。
	- `align-self` 设置单元格内容的垂直位置（上中下），跟`align-items`属性的用法完全一致，也是只作用于单个项目。


[详情](https://www.youtube.com/watch?v=6weNTVC1oik)