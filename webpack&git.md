### webpack

#### 对webpack的理解

webpack是一个应用于现代JS应用程序的**静态模块打包工具**，可以使用webpack管理模块，在webpack中所有的资源都是模块，通过分析模块间的依赖关系，在其内部构建出依赖图，最终编译输出模块为html，javascript，css以及各种静态文件，让开发更加高效

##### webpack的主要作用：

1. 模块打包：
2. 编译兼容：
3. 能力扩展

##### webpack构建流程：

#####webpack热更新原理：

`模块热替换(HMR - hot module replacement)`，又叫做`热更新`，在不需要刷新整个页面的同时更新模块，能够提升开发的效率和体验。热更新时只会局部刷新页面上发生了变化的模块，同时可以保留当前页面的状态，比如复选框的选中状态等。

热更新的核心是客户端从服务端拉取更新后的文件，准确的说是chunk diff（chunk需要更新的部分），实际上`webpack-dev-server`与浏览器之间维护了一个`websocket`，当本地资源发生变化时，`webpack-dev-server`会向浏览器推送更新，并带上构建时的`hash`，让客户端与上一次资源进行对比。客户端对比出差异后会向`webpack-dev-server`发起 Ajax 请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向`webpack-dev-server`发起 jsonp 请求获取该`chunk`的增量更新。

##### webpack是怎么打包的？babel是干什么的？

1. 打包和不打包的区别
   1. 运行效率
   2. 对基础的支持不够
2. 如何打包，babel做什么动作
   1. 把css，js，image看做一个模块，用require/import引入，找到入口文件，通过入口文件找到关联的依赖文件，把他们打包到一起，把bundle文件，拆分成多个小的文件，异步按需加载所需要的文件，如果一个文件被多次引用，打包时只会生成一个文件，如果引入的文件或者变量和方法没有被调用，就不打包。对于多个入口文件，引入了相同的代码，可以使用差价将其抽离成公共的代码
   2. babel是将高级版本的语言转化成低级版本的

### Git

####git clone,git pull, git fetch

- git clone:不需要链接远程仓库，直接将代码下载到本地

- git pull：需要链接远程仓库，获取最新的内容并合并，git pull = git fetch + git merge,可能会产生冲突，需手动解决

- git fetch:将远程仓库的内容拉取到本地，在用户检查了之后再决定是否合并git merge

#### git cheery-pick

用于将其他分支的某一个或几个分支合并到当前分支

#### git merge,git rebase

- git merge:将其他分支合并到当前分支并产生新的提交记录
- git rebase:将其他分支合并到当前分支，但不会产生新的提交记录，而是在原本的提交历史记录后面线性添加提交记录，这样review的时候更直观

