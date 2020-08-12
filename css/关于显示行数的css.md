老是要忘记，写下来记住

```javascript
// CSS 设置文字只显示一行，多余显示省略号
  /**
	思路：
	1.设置inline-block属相
	2.强制不换行
	3.固定高度
	4.隐藏超出部分
	5.显示“……”
  */
  display: inline-block;
  white-space: nowrap; 
  width: 100%; 
  overflow: hidden;
  text-overflow:ellipsis;
```



```javascript
// 显示两行
/**
思路：
	1.超出的文本隐藏
	2.溢出用省略号显示
	3.溢出不换行
	4.将对象作为弹性伸缩盒子模型显示
	5.从上到下垂直排列子元素（设置伸缩盒子的子元素排列方式）
	6.这个属性不是css的规范属性，需要组合上面两个属性，表示显示的行数
  */
  .text2{
	width:200px;
	word-break:break-all;
	display:-webkit-box;
	-webkit-line-clamp:2;
	-webkit-box-orient:vertical;
	overflow:hidden;
}
```

