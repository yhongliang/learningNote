###HTML

##### H5新特性有哪些？

- 绘图`canvas`
- 用于媒体回放的`video`和`audio`元素
- 本地离线存储`localStorage`和`sessionStorage`
- 语义化更好的内容元素: `header`,`article`,`nav`,`section`,`footer`等
- 表单控件:`calendar`,`date`,`time`,`email`,`url`,`search`等
- 新的技术:`webworker`,`websocket`
- 新的文档属性 `document.visibilityState`

##### HTML语义化

- 语义化，指对文本内容的结构化（内容语义化），选择合乎语义的标签（代码语义化）
- 语义化标签：`header`、`nav`、`main`、`article`、`section`、`aside`、`footer`等
- 优点：
  - 代码结构清晰，易于阅读，有利于维护
  - 方便其他设备解析（如：屏幕阅读器）
  - 有利于搜索引擎优化（SEO），搜索引擎爬虫会根据不同的标签来赋予不同的权重

##### script 标签中属性 async 和 defer 的区别？

- script会阻碍html解析，只有下载并执行完脚本才会继续解析html
- async：解析html时会异步下载脚本，下载完成后立即执行，因此可能会阻断html解析，且多个脚本的执行顺序也不能保证
- defer：也是异步下载脚本，但是不会阻断html解析，会按顺序执行script，不会破坏script之间的依赖关系

##### 回流(重排)和重绘

1. 概念

- 回流：DOM的变化影响了元素的几何信息，浏览器需要重新计算元素的几何属性，然后将其放在正确的位置，这个过程叫做回流或者重排。主要表现为重新生成布局，重新排列元素
- 重绘：一个元素的外观发生改变，重新把元素绘制出来的过程叫做重绘。表现为某些元素的外观发生改变，例如颜色等

2. 常见的引起重绘和重排的方法：

​	任何改变元素几何信息（元素的位置和尺寸大小）的操作都会引发回流

- 添加或删除可见的DOM元素
- 元素尺寸改变--边距，填充，宽度，高度
- 浏览器尺寸改变--resize事件发生时
- 计算offsetWidth和offsetHeight属性
- 设置style属性的值
- 修改网页默认字体

**回流一定触发重绘，重绘不一定会引发回流，回流所需的成本比重绘高得多**

3. 如何减少回流
   1. 使用transform替代top
   2. 不要把节点的属性值放在一个循环里，当成循环里的变量？？？
   3. 不要使用table布局，任何一个小的改动都会造成整个table的重新布局
   4. 把DOM离线后修改，如：使用documentFragment在内存里操作DOM
   5. 不要一条条修改样式，可以预定好class，然后修改DOM的className
   6. 使用absolute或fixed使元素脱离文档流

##### `localStorage,sessionStorage,Cookie`的区别

1. 共同点：都是保存在浏览器端，且同源
2. 区别：
   1. cookie始终在同源的http请求中携带（即使不需要），即cookie在客户端和服务器之间来回传递，而另外两者不会把数据发送到服务端，仅保存在本地。cookie还有路径(path)的概念，
   2. 存储大小限制不同。cookie只有4k，因为每次http请求都会携带cookie，所以只适合存储很小的数据。另外两种虽然也有存储限制，但是比cookie大得多，大概为5M或更大
   3. 数据有效期不同。`sessionStorage`仅在当前浏览器窗口未关闭之前有效；`localStorage`始终有效，窗口和浏览器关闭也会存储，因此适合持久化存储数据。cookie只在设置的过期时间之前有效
   4. 作用域不同。`sessionStorage`在不同的浏览器窗口不能共享，即使是同一个页面；`localStorage`和`cookie`在所有同源窗口中都是共享的

##### DOCTYPE 的作用是什么？

`<!DOCTYPE>`是文档类型声明，用于指定HTML文档的版本和类型。它位于HTML文档的最前面，放在`<html>`标签之前。

主要作用如下：

1. 指定文档类型：`<!DOCTYPE>`告诉浏览器使用哪个HTML版本解析文档。不同的HTML版本具有不同的规范和特性。通过指定文档类型，浏览器可以正确地解析和显示文档内容。

2. 浏览器模式切换：某些旧版本的浏览器在遇到没有指定文档类型的HTML文档时，会以混杂模式（quirks mode）进行解析。混杂模式下，浏览器可能会根据旧的渲染引擎规则进行解析，可能导致不一致的渲染结果。通过使用`<!DOCTYPE>`来明确指定文档类型，可以确保浏览器以标准模式（standards mode）进行解析和渲染，以获得更一致和可靠的渲染结果。

3. 标准化验证：`<!DOCTYPE>`声明还有助于确保HTML文档符合规范。当浏览器遇到`<!DOCTYPE>`时，它会根据声明的文档类型来验证文档的结构和语法是否正确。如果文档存在结构错误或不兼容的语法，浏览器可能会发出警告或错误信息。

总结来说，`<!DOCTYPE>`的主要作用是指定HTML文档的版本和类型，确保浏览器以正确的模式解析文档，并验证文档的结构和语法的正确性。



###css

##### css3有哪些新特性

- rgba和透明度
- background-image、background-origin、background-size、background-repeat
- word-wrap: break-word（对长的不可分割的单词换行）
- 文字阴影`text-shadow`
- 盒阴影 box-shadow
- font-face属性，定义自己的字体
- border-radius
- 边框图片 border-image
- 媒体查询：定义多套 css，当浏览器尺寸发生变化时采用不同的属性

##### css选择器及优先级

1. 选择器

   - id选择器(#myid)

   - 类选择器(.myclass)

   - 属性选择器(a[rel="external"])

   - 伪类选择器(a:hover, li:nth-child)

   - 标签选择器(div, h1, p)

   - 伪元素选择器(p::first-line)

   - 相邻选择器（h1 + p）

   - 子选择器(ul > li)

   - 后代选择器(li a)

   - 通配符选择器(*)

2. 优先级

   - `!important`

   - 内联样式(1000)

   - id选择器（0100）

   - 类选择器/属性选择器/伪类选择器（0010）

   - 标签选择器/伪元素选择器（0001）

   - 关系选择器/通配符选择器 (0000)

     带 !important 标记的样式属性优先级最高；样式表的来源相同时：`!important > 行内样式> ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性`

##### rgba和opacity设置透明度的区别

两者都能实现透明效果，区别在于rgba只作用于其颜色和背景色，设置rgba透明的元素的子元素不会继承其透明效果，而opacity设置透明度会作用于元素及其元素内的所有内容

##### 浏览器如何解析css选择器

**从右向左解析**，若从左向右匹配，发现不符合规则，需要回溯，会损失很多性能。若从右向左，先找到所有的最后节点，对于每一个节点，向上查找其父节点直至根节点或满足条件的匹配规则，则结束这个分支的遍历。

css解析完毕，会将css规则树和DOM Tree一起组合分析建议render Tree，最终用来绘图

##### visibility:hidden和display:none的区别

1. `display:none`隐藏后不占用文档流，`visibility:hidden`隐藏后占用文档流
2. visibility具有继承性，给父元素设置 "visibility: hidden"，子元素也会继承该属性，但如果重新给子元素设置 "visibility: visible"，则子元素又会显示出来。
3. visibility: hidden 不会影响计数器的计数，虽然隐藏了，但计数器依然运行着。
4. 在 css3 中 transition 支持 visibility 属性，但不支持 display。因为 **transition 可以延迟执行**，因此配合 visibility 使用纯 css 延时显示效果可以提高用户体验。
5. `display:none`会引起回流和重绘，`visibility:hidden`只会引起重绘

##### line-height如何继承

1. 父元素的`line-height`如果是数值和比例，则子元素会继承数值和比例
2. 父元素的`line-height`如果是百分比,则子元素的`line-height`的值继承的是父元素的`font-size`*百分比的值

##### 如何让chrome支持10px的文字

1. font-size: 12px; -webkit-transform: scale(0.84);
2. font-size: 20px; -webkit-transform: scale(0.5);

##### position属性的值有哪些

1. static:默认定位，出现在正常的文档流中（忽略top，bottom，left，right 或 z-index声明）
2. relative:相对定位，定位的元素会出现在它的位置上，可以通过设置水平或垂直位置来使其相对起点位置移动。**无论是否移动，都会占用原来的空间，移动元素会导致覆盖其他元素**
3. absolute:绝对定位，元素的位置相对于最近的已定位的父元素，如果没有已定位的父元素，则相对于根元素(html)定位，**绝对定位的元素的不占据文档流，不占据空间，会与其他元素重叠**
4. fixed:固定定位，元素的位置相对于**浏览器窗口**是固定定位，即使窗口滚动也不会移动。**固定定位的元素不占据文档流，不占据空间，会与其他元素重叠**
5. sticky:粘性定位。可看作**相对定位和固定定位的混合**。元素在跨域特定阈值前是相对定位，之后是固定定位。top，right，buttom，left，必须指定这四个阈值中的一个，才可以使粘性定位生效，否则行为与其相对定位相同。
6. Inherit:应该从父元素继承position的值

##### css盒模型

1. 标准盒模型：width = content的宽度，总宽度 = width + border(左右) + padding(左右) + margin(左右)。高度同理
2. 怪异盒模型：width = content + border + padding, 总宽度 = content + margin.高度同理

​	box-sizing属性设置

​	 1.content-box:标准盒模型

​	 2.border-box：怪异盒模型

	 3. inherit,继承父元素的box-sizing值

##### BFC(块级格式化上下文)

1. 概念：是一个独立的渲染区域，规定了内部box如何布局，且这个区域内的子元素不会影响到外面的元素
2. 布局规则：
   - 内部的 box 会在垂直方向一个接一个的放置
   - box 垂直方向的距离由 margin 决定，同一个 BFC 相邻的 box 的 margin 会发生重叠
   - 每个 box 的 margin 左边，与包含块的左 border 相接触（对于从左往右的格式化，否则相反）
   - BFC 的区域不会与 float box 重叠
   - BFC 是一个独立容器，容器内的子元素不会影响到外面的元素
   - 计算 BFC 高度时，浮动元素也参与计算高度
3. **如何创建BFC**简而言之（OFPD）O:overflow,F:float,P:position,D:display
   - 根元素，即html元素
   - float != none
   - position == absolute || fixed
   - display == inline-block || table-cell || table-caption || flex
   - overflow != visible

4. 应用场景（解决问题）
   - 去除边距重叠问题
   - 清除浮动（让父元素的高度包含子浮动元素）
   - 阻止元素被浮动元素覆盖

##### 让一个元素水平/垂直居中

##### flex布局

##### 清除浮动

##### img的alt和title的异同，实现图片懒加载的原理？

##### 消除元素间的白色间隔

使用padding和margin无法将元素之间的白色间隔消除，可能有以下几个可能的原因：

1. 默认样式：某些元素在浏览器中有默认的样式，例如按钮、表单元素等。这些默认样式可能包含了padding和margin属性，导致无法完全消除元素之间的间隔。可以使用开发者工具检查元素的默认样式，尝试覆盖或重置相关样式。

2. 盒模型：在CSS中，元素的宽度和高度由内容区域、内边距（padding）、边框（border）和外边距（margin）组成，这被称为盒模型。如果元素的盒模型设置不当，例如宽度和高度没有计算进入盒模型中，就会导致元素之间的间隔无法消除。可以通过调整元素的盒模型设置，如使用`box-sizing: border-box`来确保元素的宽度和高度包括了padding和border。

3. 相邻元素之间的空白节点：在HTML中，相邻元素之间的换行、空格等字符会被解析为文本节点，可能导致元素之间产生间隔。可以使用CSS的`font-size: 0`和`line-height: 0`来消除这些空白节点之间的间隔。

4. 其他元素或样式的影响：元素之间的间隔可能受到其他元素或样式的影响，例如父元素的布局、浮动、定位等。需要综合考虑整个布局结构和相关样式，找到导致间隔的原因并进行调整。

如果以上方法都无法解决问题，建议使用浏览器的开发者工具检查元素的样式和布局情况，分析可能的原因并进行调试和修复。

##### link和@import的区别

`link`和`@import`是两种不同的方式用于在HTML文档中引入外部资源（如CSS文件）。它们之间有以下区别：

1. 位置：`link`标签是HTML标签，通常放置在`head`标签中的`head`部分，用于引入外部资源。而`@import`是CSS的一种规则，需要放置在CSS文件中，可以放在任何地方。

2. 加载时机：`link`标签在页面加载时同时加载外部资源，并行下载，不会阻塞页面的渲染。而`@import`规则会在CSS文件加载完成后再加载外部资源，即串行下载，会阻塞页面的渲染。

3. 兼容性：`link`标签具有更广泛的浏览器兼容性，适用于所有现代浏览器。而`@import`规则在旧版本的浏览器（如IE6、IE7）中可能不被支持。

4. 功能性：`link`标签除了可以引入CSS文件外，还可以用于引入其他资源，如网站图标（favicon）、JavaScript文件等。而`@import`规则仅用于引入CSS文件。

综上所述，如果只需引入CSS文件，优先推荐使用`link`标签。它具有更好的性能和兼容性。而`@import`规则在某些特定的情况下可能有其用途，例如在需要根据媒体查询条件加载不同的样式文件时，可以使用`@import`规则来实现动态加载。









