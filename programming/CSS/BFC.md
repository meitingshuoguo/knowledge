# 块格式化上下文（Block Formatting Context，BFC）

是页面中的一块渲染区域（二维的），具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC元素具有普通容器所没有的一些特性。BFC区域只包含其直接子元素。


# BFC特性：
- BFC元素的子元素会在垂直方向上一个接一个的放置。（默认行为）
- 属于同一个BFC的两个相邻元素的margin会发生折叠（取最大的那个margin），不同BFC不会发生折叠。（解决margin折叠问题）
- BFC的区域不会与float的元素区域重叠（可以和浮动元素产生边界，实现左右布局）
- 计算BFC的高度时，浮动子元素也参与计算。（解决浮动塌陷问题）


# 创建一个BFC
- `display`的值是`inline-block`、`flow-root`、`flex`或者`inline-flex`、`grid`或者`inline-grid`、`table-cell`、`table-caption`、
-  根元素（`<html>）`
-  浮动元素（ [`float的值`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float) 不是 `none`）
-  绝对定位元素（ [`position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position) 为 `absolute` 或 `fixed`）
-  [`overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow) 值不为 `visible` 的块元素
-  [`contain`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain) 值为 `layout`、`content` 或 `paint` 的元素

# 用处
1. 包含塌陷
	当父子关系的盒子，给子元素添加margin-top，有可能会把父元素一起带跑。这时给父元素设置BFC就可以解决这个问题。
2. 垂直塌陷
	两个相邻的BFC子元素的margin在垂直方向会折叠，解决办法就是分别放到两个BFC元素中去。
3. 浮动塌陷
	可以包住子元素，解决子元素浮动后带来的父元素高度塌陷的问题。
	子元素浮动后脱离父元素，给父元素设置BFC即可包住子元素。
4. 和浮动元素产生边界。
	实现两栏布局，左边元素浮动，右边设置BFC，就不会让左边元素浮动在右边元素上面。