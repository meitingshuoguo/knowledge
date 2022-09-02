# 修改浏览器滚动条

```css
/* webkit内核浏览器的滚动样式 */
/* 滚动条整体 */
::-webkit-scrollbar {
  width: 4px;
  height: 4px;
  background-color: transparent;
}
/* 滚动条的轨道 */
::-webkit-scrollbar-track {
  border-radius: 15px;
  background-color: transparent;
}
/* 滚动条可以滚动的小方条的样式 */
::-webkit-scrollbar-thumb {
  border-radius: 15px;
  background-color: rgba(189, 189, 189, 0.5);
}

/* 火狐浏览器的滚动条样式 */
* {
  scrollbar-width: thin;
  scrollbar-color: rgba(189, 189, 189, 0.5) rgba(0, 0, 0, 0);
}

```

也可以单独修改某一个可滚动元素的滚动条样式

```css
div::-webkit-scrollbar-thumb{
  border-radius: 15px;
  background-color: transparent;
}
div{
  scrollbar-width: thin;
  scrollbar-color: rgba(189, 189, 189, 0.5) rgba(0, 0, 0, 0);
}
```

