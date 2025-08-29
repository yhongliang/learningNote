###Vue面试随记

#### MVVM模型

MVVM：是`Model-View-ViewModel`的简写，本质是MVC的升级版。其中`Model`代表的是数据模型，`View`代表的是看到的页面，`ViewModel`是`Model`和`View`之间的桥梁，数据会绑定到ViewModel层并自动将数据渲染到页面中，视图变化的时候会通知`ViewModel`更新数据，以前是操作DOM来更新视图，现在是**数据驱动视图**

#### Vue的生命周期

在Vue 2中，生命周期包括了一系列的钩子函数，它们在Vue实例的不同阶段被调用，允许我们执行自定义的逻辑。以下是Vue 2的生命周期钩子函数及其触发时机的详细描述：

1. beforeCreate：
   - 时机：在Vue实例创建之前触发。
   - 描述：在该钩子函数中，Vue实例的初始化尚未开始，无法访问到数据和组件实例。

2. created：
   - 时机：在Vue实例创建完成后触发。
   - 描述：在该钩子函数中，Vue实例已经完成了数据观测和事件配置，但尚未挂载到DOM上。

3. beforeMount：
   - 时机：在Vue实例挂载到DOM之前触发。
   - 描述：在该钩子函数中，Vue实例已经编译了模板，并且将要把编译好的模板挂载到DOM上。

4. mounted：
   - 时机：在Vue实例挂载到DOM后触发。
   - 描述：在该钩子函数中，Vue实例已经挂载到了DOM上，可以进行DOM操作，访问DOM元素，以及与第三方库进行集成。

5. beforeUpdate：
   - 时机：在Vue实例更新之前触发。
   - 描述：在该钩子函数中，Vue实例的数据发生变化，但尚未重新渲染DOM。

6. updated：
   - 时机：在Vue实例更新完成后触发。
   - 描述：在该钩子函数中，Vue实例已经完成了数据更新和重新渲染，并且DOM也已经更新。

7. beforeDestroy：
   - 时机：在Vue实例销毁之前触发。
   - 描述：在该钩子函数中，Vue实例仍然完全可用，可以执行清理工作、解绑事件监听器、取消定时器等。

8. destroyed：
   - 时机：在Vue实例销毁后触发。
   - 描述：在该钩子函数中，Vue实例已经完全销毁，所有的事件监听器和定时器已被移除，可以进行最后的清理操作。

此外，Vue 2还提供了一些其他的生命周期钩子函数，如`activated`和`deactivated`，用于处理组件的`keep-alive`缓存时的激活和停用状态。在具体使用时，可以根据实际需求选择合适的钩子函数来执行自定义的操作和处理。

#### Vue的实例挂载过程中发生了什么

当Vue实例进行挂载过程时，会经历以下的主要步骤和过程：

1. 初始化Vue实例：在这一步，Vue会对传入的选项进行初始化，包括数据的观测、事件的初始化、生命周期的初始化等。

2. 编译模板：Vue会将模板编译成渲染函数。如果使用的是单文件组件，Vue会在构建过程中将其转换为渲染函数。

3. 创建虚拟DOM：在这一步，Vue会使用渲染函数生成虚拟DOM（Virtual DOM）树，该树表示了组件的结构和状态。

4. 执行挂载：Vue将虚拟DOM挂载到真实的DOM上，即将组件渲染到页面上的指定位置。

   - 创建实例的根DOM节点。
   - 将虚拟DOM渲染到根节点，并生成真实的DOM元素。
   - 将真实的DOM元素插入到页面的指定位置，完成组件的挂载。

5. 数据响应式：一旦实例挂载完成，Vue会建立响应式的数据绑定，当数据发生变化时，会自动更新相应的DOM。

6. 生命周期钩子函数：在挂载过程中，Vue会触发一系列的生命周期钩子函数，包括`beforeMount`和`mounted`。

   - `beforeMount`：在挂载之前调用，此时虚拟DOM已经生成，但尚未渲染到真实DOM中。
   - `mounted`：在挂载完成后调用，此时虚拟DOM已经渲染到真实DOM中，可以访问到DOM元素。

7. 实例更新：一旦实例挂载完成，当数据发生变化时，Vue会自动执行更新过程，重新渲染虚拟DOM，并将变更应用到真实DOM上。

以上是Vue实例挂载过程中的主要步骤和过程。通过这个过程，Vue能够将组件渲染到页面上，并建立起响应式的数据绑定，使数据和视图保持同步。同时，生命周期钩子函数提供了在不同阶段执行自定义逻辑的机会，使开发者可以在挂载过程中进行额外的操作和处理。

#### Vue的模板编译原理

Vue的模板编译原理是将Vue的模板语法转化为渲染函数，该渲染函数可以生成虚拟DOM，并最终渲染为真实的DOM。

以下是Vue模板编译的基本流程：

1. 解析模板：Vue使用解析器将模板字符串解析成抽象语法树（AST）。AST是一个以JSON形式表示的树状结构，它将模板的各个部分抽象为节点，并描述了节点之间的关系。

2. 优化静态内容：遍历AST，对静态内容进行优化。静态内容指的是不依赖于数据的部分，如纯文本节点。优化静态内容可以在编译时进行静态标记，减少运行时的检查和处理。

3. 生成渲染函数：通过AST，Vue将其转化为可执行的渲染函数。渲染函数是一个函数，它接受数据作为参数，并返回虚拟DOM（VNode）。

4. 生成静态渲染函数：对于只包含静态内容的模板片段，Vue会生成静态渲染函数，以提高渲染性能。

5. 渲染和更新：使用渲染函数生成虚拟DOM，并将其渲染为真实的DOM。在数据更新时，重新调用渲染函数生成新的虚拟DOM，并通过比对新旧虚拟DOM的差异，最小化对真实DOM的操作，提高性能。

需要注意的是，Vue的模板编译过程通常是在构建时进行，而不是在运行时。这意味着在构建阶段，Vue将模板编译为渲染函数，并将其包含在最终的构建输出中，以便在运行时直接使用渲染函数进行渲染。

通过将模板编译为渲染函数，Vue能够将模板的声明式语法转化为高效的、可重用的渲染逻辑，提高渲染性能并实现组件的动态更新。模板编译是Vue框架核心的一部分，它为Vue提供了强大的模板能力。

#### Vue的响应式原理

Vue的响应式原理是指Vue如何实现数据和视图之间的双向绑定，即当数据发生变化时，视图自动更新，同时当用户操作视图时，数据也会相应地发生改变。

Vue的响应式原理基于以下几个关键概念：

1. 响应式对象：在Vue中，通过将一个普通的JavaScript对象传入Vue实例的`data`选项中，该对象就会变成响应式对象。Vue会遍历该对象的属性，并使用`Object.defineProperty`方法对每个属性进行劫持，使其成为响应式的。

2. 数据劫持：Vue通过数据劫持来实现响应式。在劫持过程中，Vue会为对象的每个属性创建一个依赖追踪器（Dep），用于收集依赖和触发更新。当访问属性时，会将依赖追踪器添加到当前的依赖收集器（Watcher）中，以建立属性和依赖之间的关联。

3. 依赖收集：Vue使用依赖收集的机制来跟踪数据属性的依赖关系。当模板中使用到响应式数据时，Vue会收集对应属性的依赖，将其添加到依赖追踪器中。这样，在数据发生变化时，Vue就能够知道哪些视图依赖于该数据，从而触发视图的更新。

4. 视图更新：当响应式数据发生变化时，Vue会通知依赖追踪器，依赖追踪器会遍历所有相关的依赖（Watcher）并触发它们的更新函数。更新函数会重新渲染受影响的视图，并将变化应用到DOM上，从而完成视图的更新。

5. 异步更新：为了提高性能，Vue对视图更新进行了优化。Vue会将多次数据变更的操作合并为一次，并在下一个事件循环中异步地执行更新操作。这样可以避免频繁的DOM操作，提升性能并避免不必要的重复渲染。

通过以上的响应式原理，Vue能够实现数据和视图之间的双向绑定，让开发者可以专注于数据的变化和交互逻辑，而无需手动更新视图。同时，响应式的特性也使得Vue具有高效的渲染性能和可维护性，为开发者提供了更好的开发体验。

#### 虚拟DOM

概念：
 虚拟DOM，顾名思义就是虚拟的DOM对象，它本身就是一个JS对象，只不过是通过不同的属性去描述一个视图结构。

虚拟DOM的好处：
 (1) 性能提升
 直接操作DOM是有限制的，一个真实元素上有很多属性，如果直接对其进行操作，同时会对很多额外的属性内容进行了操作，这是没有必要的。如果将这些操作转移到JS对象上，就会简单很多。另外，操作DOM的代价是比较昂贵的，频繁的操作DOM容易引起页面的重绘和回流。如果通过抽象VNode进行中间处理，可以有效减少直接操作DOM次数，从而减少页面的重绘和回流。
 (2) 方便跨平台实现
 同一VNode节点可以渲染成不同平台上对应的内容，比如：渲染在浏览器是DOM元素节点，渲染在Native（iOS、Android）变为对应的控件。Vue 3 中允许开发者基于VNode实现自定义渲染器（renderer），以便于针对不同平台进行渲染。

结构：
 没有统一的标准，一般包括`tag`、`props`、`children`三项。
 `tag`：必选。就是标签，也可以是组件，或者函数。
 `props`：非必选。就是这个标签上的属性和方法。
 `children`：非必选。就是这个标签的内容或者子节点。如果是文本节点就是字符串；如果有子节点就是数组。换句话说，如果判断`children`是字符串的话，就表示一定是文本节点，这个节点肯定没有子元素。

#### Vue中key的作用

**key主要是为了更加高效的更新虚拟DOM**

Vue判断两个节点是否相同时，主要判断两者是否有相同的`key`和`元素类型tag`，如果不设置key，默认就是undefined，则认为永远是两个相同的节点，只能做更新操作，将造成大量的DOM更新操作

#### 为什么组件中的data是一个函数

New Vue()中可以是函数也可以是对象，因为根实例只有一个，不会产生数据污染。

组件中，data必须为函数，目的是为了防止多个组件实例之间共用一个data，产生数据污染。而采用函数的形式，initData时会将其作为工厂函数返回全新的data对象

#### Vue中组件间的通信方式

在Vue中，组件间的通信方式有以下几种：

1. 父子组件通信：
   - Props / `$emit`：父组件通过props向子组件传递数据，子组件可以通过事件`$emit`向父组件发送消息。
   - `$refs`：父组件可以通过`ref`属性获取子组件的实例，从而直接访问子组件的属性和方法。

2. 子组件向父组件通信：
   - `$emit`：子组件通过触发自定义事件并使用`$emit`向父组件发送消息。
   - `$attrs` / `$listeners`：子组件可以通过`$attrs`访问父组件传递的属性，通过`$listeners`访问父组件传递的事件监听器。

3. 兄弟组件通信：
   - EventBus：通过创建一个全局的事件总线（EventBus），兄弟组件可以通过事件的发布和订阅来进行通信。
   - Vuex：Vuex是Vue的状态管理库，允许多个组件共享状态，从而实现兄弟组件间的通信。

4. 跨级组件通信：
   - Provide / Inject：父组件通过`provide`提供数据，子孙组件通过`inject`注入数据，从而实现跨级组件的通信。
   - `$attrs` / `$listeners`：通过`$attrs`和`$listeners`的传递，可以在跨级组件间传递属性和事件。

5. 使用第三方库：
   - PubSub.js：使用第三方库PubSub.js实现组件间的发布-订阅模式的通信。
   - Event Bus库：使用类似Event Bus的库，如mitt、tiny-emitter等，实现组件间的事件通信。

根据不同的场景和需求，可以选择适合的通信方式来进行组件间的通信。在实际开发中，根据组件之间的关系和通信复杂度，选择合适的通信方式可以提高代码的可读性和可维护性。

#### v-show和v-if的区别

1. 编译时机不同：
   - `v-show`：在编译时一直存在于DOM中，只是通过css的display来控制其显示或隐藏
   - `v-if`：编译时会根据条件进行判断，如果条件为真，才会将元素添加到DOM中，否则不会渲染该元素
2. 渲染性能:
   - `v-show`：元素一开始就渲染到DOM中，通过CSS控制显示与隐藏。因此，切换显示与隐藏时的性能开销较小，但在初始渲染时会有一定的性能开销
   - `v-if`：元素根据条件进行渲染，条件为假，就不渲染。因此初始渲染时性能开销较小，但在切换显示与隐藏时会有一定的性能开销
3. 条件切换频率：
   - `v-show`：适用于频繁切换显示与隐藏的元素，因为一直存在于DOM中。切换时不会触发重复渲染和销毁的过程
   - `v-if`：适用于条件不经常改变的元素，因为在条件变化时，都会触发重新渲染和销毁的过程
4. 初始渲染开销：
   - `v-show`：初始渲染时，元素会被渲染到DOM中，即使条件为假。因此，如果在初始状态下元素需要隐藏，会有一定的初始性能开销。
   - `v-if`：初始渲染时，根据条件进行判断，条件为假，就不渲染。因此在初始状态下元素如果需要隐藏，不会有初始性能开销
5. 触发生命周期不同。`v-show`由 false 变为 true 的时候不会触发组件的生命周期；`v-if`由 false 变为 true 的时候，触发组件的`beforeCreate`、`created`、`beforeMount`、`mounted`钩子，由 true 变为 false 的时候触发组件的`beforeDestory`、`destoryed`钩子。
6. 使用场景：
   如果需要非常频繁地切换，则使用`v-show`较好，如：手风琴菜单，tab 页签等； 如果在运行时条件很少改变，则使用`v-if`较好，如：用户登录之后，根据权限不同来显示不同的内容。

综上所述，`v-show`适合频繁切换显示与隐藏的元素，而`v-if`适合条件不经常改变的元素。根据具体的场景和需求，选择合适的指令可以提高渲染性能和优化用户体验。

#### computed和watch的区别

- computed计算属性，依赖其他属性值，内部任一个依赖项的变化都会重新执行该函数，计算属性有缓存，多次重复使用计算属性时会从缓存中获取返回值，计算属性必须要有return关键词
- watch侦听到某一数据变化从而触发函数。当数据为对象类型时，对象中的属性值变化需要使用深度侦听`deep`属性,也可以在页面第一次加载时使用立即侦听`immediate`属性

运用场景：计算属性一般用在渲染模版中，某个值依赖其他响应对象甚至是计算属性而来；而侦听属性适用于观测某个值的变化从而去实现一段复杂的业务逻辑

- computed中使用get和set可以实现计算属性的双向绑定

- ```vue
  <template>
    <div>
      <div style="margin-bottom:15px;">
        Your roles: {{ roles }}
      </div>
      Switch roles:
      <el-radio-group v-model="switchRoles">
        <el-radio-button label="editor" />
        <el-radio-button label="admin" />
      </el-radio-group>
    </div>
  </template>
  
  <script>
  export default {
    computed: {
      roles() {
        return this.$store.getters.roles
      },
      switchRoles: {
        get() {
          return this.roles[0]
        },
        set(val) {
          this.$store.dispatch('user/changeRoles', val).then(() => {
            this.$emit('change')
          })
        }
      }
    }
  }
  </script>
  ```

  

#### v-if和v-for为什么不能放在一起使用

#### data为什么是一个函数

防止组件在被复用时，产生数据的关联关系，从而导致数据干扰，本质是一个工厂函数

####vue-router

1. vue-router原理：通过对url地址的监听，实现对不同组件的渲染

2. 导航钩子：

   1. beforeEach,afterEach:全局路由钩子,有三个参数(to, from, next)，next是必须要执行的函数，若不传参数那么就执行下一个钩子函数，若传入false，就终止跳转，若传入的是一个路径，则跳转到相应的路由，如果传入 error ，则导航终止，error 传入错误的监听函数。next函数的使用[next使用的正确姿势](https://blog.csdn.net/qq_41912398/article/details/109231418)
   2. beforeEnter:单个路由独享钩子函数,在路由配置上直接定义
   3. beforeRouteEnter,beforeRouteUpdate,beforeRouteLeave:组件内的路由钩子，在路由内部直接定义

3. Vue-router的两种模式：

   1. hash：原理是onhashchanged事件，可以在window上监听此事件，

      ```javascript
      window.onhashchanged = function(event){
        console.log(event.oldURL, event.newURL)
        let hash = location.hash.slice(1)
      }
      ```

      

   2. history:利用了H5 history interface中新增的pushState和replaceState的方法，需后台支持，若刷新时，服务器没有响应的资源，就会报404

4. vue通过全局路由守卫beforeEach进行页面的权限控制，可以在路由跳转前，判断用户的权限，(利用cookie，token)，若不能跳转，则重定向或提示错误，后台管理系统中经常能遇到

5. $router 和 $route

   1. $router是路由实例对象，包含了路由的跳转方法和钩子函数
   2. $route是路由信息对象，包括path,params,name,query,hash,fullPath,matched等路由信息参数

####指令

1. v-model：等同于v-bind:value和v-on:input="e => value = e.target.value"

   1. v-bind绑定数据，props传入子组件中

   2. $emit('input')触发事件，将新的数据传给父组件

   3. v-model并不仅限于Form表单, 目前还支持更改事件名称和变量名称

      ```javascript
      model: {
        prop:'checked',
        event:'change'
      }
      ```

2. 自定义指令判断权限时，可能不生效，此时可以使用v-if进行权限判断，

   ```vue
   例如：v-permission="["admin","editor"]"在动态渲染DOM时，可能不生效，
   可以用v-if="checkPermission(['admin', 'editor'])"，确保权限判断生效
   ```

   在动态渲染DOM的情况下，`v-permission`指令的判断时机取决于指令的具体实现方式和所绑定的元素的生命周期。

   一般情况下，`v-permission`指令的判断会在元素被创建并插入到DOM中时进行，即在`inserted`钩子函数中执行权限判断逻辑。这意味着在元素被动态添加到DOM之后，`v-permission`指令会立即执行权限判断。

   然而，需要注意的是，如果动态渲染的元素在初始渲染时并不存在于DOM中，而是在后续的操作中被动态添加到DOM中，那么`v-permission`指令的判断可能无法生效，因为判断时机已经错过了。

   为了确保`v-permission`指令的判断能够生效，你可以考虑以下几点：

   1. 在动态渲染元素之前，先进行权限判断，然后再根据权限结果决定是否添加元素到DOM中。
   2. 使用条件渲染指令`v-if`来控制元素的显示和隐藏，将权限判断放在`v-if`指令中进行。
   3. 在元素被动态添加到DOM后，手动触发`v-permission`指令的判断逻辑，可以使用`Vue.nextTick`或其他方式来延迟执行权限判断。

   通过以上方法，你可以确保在动态渲染DOM时，`v-permission`指令的判断能够正确生效，根据权限结果决定元素的显示和隐藏。

####nextTick实现原理

**在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM**

nextTick是Vue提供的一个全局API，由于Vue的异步更新策略，导致我们修改数据后不会直接体现在DOM上，此时如果想要立即获取更新后的状态，就需要用此方法

Vue在更新DOM时是异步的，当数据变化时，Vue将开启一个异步更新队列，并缓冲在同一事件循环中发生的所有数据变更。如果是同一个watcher被多次触发，则只会被推入队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和DOM操作是十分重要的，nextTick会在队列中加入一个回调函数，确保该函数在前面的DOM操作完毕后再调用

**使用场景**

1. 在数据更新后立即得到更新后的DOM结构，可以使用nextTick
2. 在created生命周期中使用，进行DOM操作

####Vue的模板编译原理

Vue 中有个独特的编译器模块，称为`compiler`，它的主要作用是将用户编写的`template`编译为js中可执行的`render`函数。
 在Vue 中，编译器会先对`template`进行解析，这一步称为`parse`，结束之后得到一个JS对象，称之为`抽象语法树AST`；然后是对`AST`进行深加工的转换过程，这一步称为`transform`，最后将前面得到的`AST`生成JS代码，也就是`render`函数。

####mixins	

[Vue2官方文档中的说明](https://v2.cn.vuejs.org/v2/guide/mixins.html)

- 数据合并：以组件数组为准。
- 对象合并：`methods,components,directives`当键值对冲突时以组件的对象为准
- 生命周期(created,mounted)：mixins和组件中的会组成一个数组，先执行mixins的再执行组件中的











#### Vue项目随记

##### table表格的列表标题也可使用v-for渲染

- 需先将列表标题的内容放在一个对象中

- 然后使用v-for遍历

- 最后动态绑定到table的各个属性上

  ```html
  //此处以el-table为例
  <el-table :data="tableData" style="width: 100%">
    <el-table-column v-for="(key, val) in tableLabel" :key="key" :prop="val" :label="key" />
  </el-table>
  
  data() {
  	return {
  		tableLabel: {
          name: '品牌',
          todayBuy: '日销量',
          monthBuy: '月销量',
          totalBuy: '总销量'
      }
  	}
  }
  ```

  

##### Vue中改变UI框架的默认样式

1. 使用全局样式覆盖:创建global.css在main.js中引入
2. 使用自定义主题：Element UI提供了自定义主题的功能，允许您修改默认样式的各个方面，如颜色、字体大小、边框等。您可以使用Element UI提供的主题生成工具，通过覆盖默认的Sass变量来定制主题。生成的主题文件可以在项目中引入，从而应用自定义的样式。
3. 局部样式覆盖：组件内部添加自定义的class和style属性，来覆盖默认的样式。注意设置scoped来限制样式仅在当前组件生效。注意可能需要deep来进行样式穿透

##### this.$el和this.$refs

在Vue中，`this.$el`是一个指向当前组件实例所关联的DOM元素的引用。

当Vue实例被创建并且挂载到DOM上时，Vue会将组件的模板编译为真实的DOM，并将其插入到指定的目标元素中。`this.$el`就是指向这个目标元素的引用。

例如，考虑以下Vue组件：

```vue
<template>
  <div>
    <h1>{{ message }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello Vue!'
    }
  },
  mounted() {
    console.log(this.$el); // 输出组件的根DOM元素
  }
}
</script>
```

在上面的代码中，`this.$el`将在组件挂载完成后被打印出来。它将引用`<div>`元素，该元素包含一个`<h1>`子元素，显示"Hello Vue!"。

请注意，`this.$el`在组件的生命周期钩子函数`mounted`之后才可用。在`mounted`之前，它将为`undefined`。

在Vue中，`this.$refs`是一个对象，用于访问组件或DOM元素的引用。它提供了一种在Vue组件中直接引用其他组件或DOM元素的方式。

当在模板中给组件或DOM元素添加`ref`属性时，Vue会自动在`this.$refs`对象中创建一个对应的属性，并将引用的组件或DOM元素赋值给该属性。

例如，考虑以下的Vue组件：

```vue
<template>
  <div>
    <h1 ref="titleRef">Hello Vue!</h1>
    <child-component ref="childRef"></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  mounted() {
    console.log(this.$refs.titleRef); // 输出<h1>元素的引用
    console.log(this.$refs.childRef); // 输出子组件ChildComponent的引用
  }
}
</script>
```

在上面的代码中，`<h1>`元素和`<child-component>`组件都添加了`ref`属性，并分别设置为"titleRef"和"childRef"。在`mounted`钩子函数中，我们可以通过`this.$refs`来访问这些引用。

`this.$refs.titleRef`将引用组件的DOM元素`<h1>`，而`this.$refs.childRef`将引用子组件`ChildComponent`的实例。

请注意，`this.$refs`的值在组件挂载之后才会被填充，因此在`mounted`钩子函数或之后的生命周期钩子函数中使用`this.$refs`才能获取到正确的引用。