1. Echarts渲染问题，DOM和数据的优先级
	1. Q: 如果数据还没到位，就渲染echarts的dom，会有问题吗
		A: 会的。
		- 第一种情况，调用 `setOption(undefined)` 或 `null`
```
echarts.js:xxxx Uncaught TypeError: Cannot read properties of undefined
```
		- 图表不会渲染，甚至整个图表崩溃
		- 原因：setOption希望接收一个合法的option==对象==，不能是undefined
		- 解决办法：可以先判断option是否为空，再去渲染

		 - 第二种情况，接收的是一个空对象，直接渲染一个空画布
		 - 第三种情况，数据还没到位，手动触发了resize方法，原因是内部无数据或页面重排。