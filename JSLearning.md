### 面试

#### 1.JS的基本数据类型

- 基本数据类型(值类型)：
- 复杂数据类型(引用类型)：

#### 2.null和undefined的区别

- `null`表示对象被定义了，但是值是‘空值’：用法
  - 作为函数的参数，表示函数的参数不是对象
  - **作为对象原型链的终点**
- `undefined`表示不存在这个值，就是此处应该有个值，但是还没定义，尝试读取就会返回undefined，用法：
  - 函数没有返回值时，默认返回undefined
  - 变量已声明，但没赋值，会返回undefined
  - 对象中没有赋值的属性，该属性的值为undefined
  - 调用函数时，没有提供必需的参数，参数等于undefined

####3.判断JS的数据类型

1. typeof:可以判断null以外的其他基本数据类型，以及从对象类型中识别出函数(function)
2. instanceof：一般用来判断引用数据类型，但不能正确判断基本数据类型，根据在原型链中查找判断当前的原型对象是否存在返回布尔值
3. Object.prototype.toString：可正确判断所有类型，并显示所属类型
4. `Array.isArray`用于判断是否是数值
5. `.Constructor`

#### 4.==和===的区别

#### 5.如何遍历对象的属性

1. Object.keys()
2. Object.getOwnPropertyNames()
3. for...in

#### 6.如何判断两个对象是否相等

#### 7.原型和原型链

js是面向对象的，每个对象都有一个proto属性，该属性指向它的原型对象。该实例的构造函数有一个prototype原型属性，与实例的proto指向同一个对象。同时，原型对象的constructor指向构造函数本身

```mermaid
graph TD;
f1=newFoo-->|__proto__|Foo.prototype;
Function的Foo-->|prototype|Foo.prototype;
Foo.prototype-->|constructor|Function的Foo;
```

当一个对象查找某个属性时，若自身没有就会根据__proto__属性去向上去原型查找，若还是没有，就向原型的原型去查找，直至查到`Object.prototype.__proto__`也就是`null`，这样就形成了`原型链`。

#### 8.闭包

#### 9.new操作符的实现机制

#### 10.this的理解

- 下面是整理好的 **Markdown 形式内容**：

  ---

  ## 📌 各种绑定方式详细说明

  ### 1. 显式绑定（call / apply / bind）

  ```js
  function greet() {
    console.log(this.name);
  }
  
  const person = { name: 'Alice' };
  
  greet.call(person);   // Alice
  greet.apply(person);  // Alice
  const bound = greet.bind(person);
  bound();              // Alice
  ```

  ------

  ### 2. 构造函数绑定（new）

  ```js
  function Person(name) {
    this.name = name;
  }
  
  const p = new Person('Tom');
  console.log(p.name); // Tom
  ```

  ------

  ### 3. 隐式绑定（作为对象方法调用）

  ```js
  const obj = {
    name: 'Bob',
    sayHi() {
      console.log(this.name);
    }
  };
  
  obj.sayHi(); // Bob
  ```

  #### ✅ 判定方法：

  - 如果函数是通过 `obj.fn()` 的形式调用的，`this` 就指向 `obj`。
  - 口诀：**谁点的我，我就是谁**

  ------

  ### 4. 默认绑定（普通函数直接调用）

  ```js
  function test() {
    console.log(this);
  }
  
  test(); // 非严格模式: window；严格模式: undefined
  ```

  ------

  ### 5. 隐式丢失（退化为默认绑定）

  ```js
  const obj = {
    name: 'Charlie',
    greet() {
      console.log(this.name);
    }
  };
  
  const fn = obj.greet;
  fn(); // undefined（严格模式下）
  ```

  #### 原因：

  - 赋值时函数脱离了对象上下文，不再是 `obj.fn()` 的形式，因此退化为默认绑定。

  ------

  ## 🧠 小结口诀

  > 🧾 “谁点的我，我就是谁。”（隐式绑定）
  >  🧾 “丢了对象就是默认绑。”（隐式丢失）
  >  🧾 “call、apply、bind 是显式。”
  >  🧾 “new 就指向新对象。”
  >  🧾 “箭头函数不绑定 this。”

  ------

  ## ⚠️ 流程图常见错误说明

  许多版本的 `this` 决策图中，有如下错误：

  > ❌ “Is the function called as a method?” → Yes → Default Binding

  这是 **错误的**。
   **正确应为：Yes → 隐式绑定（this = 对象）**

  ------

  ## ✅ 结语

  掌握 `this` 的绑定规则，核心是理解调用方式，而不是定义位置。

  建议多做实践题，如：

  - `setTimeout` 回调中的 `this`
  - 对象方法赋值后调用
  - 箭头函数与普通函数混合场景

  也可以绘制一张自己的流程图加强记忆。

  ```
  ---
  
  如果你还需要添加图示、用英文版本，或做成教学笔记等形式，我也可以帮你继续整理！
  ```

#### 11.call,apply,bind的区别以及手写实现

- call:
- apply:
- bind:
- **call,apply都是立即执行的，bind不会立即执行，而是生成一个修改this之后的函数**
- **call和bind的第二个参数都是列表形式，apply的第二个参数是数组形式**

#### 12.箭头函数

- 是一种定义函数的新的方式，比普通函数更简单和方便
- 捕获上下文的this

- 使用call，apply，bind并不会改变this的指向

#### 13.浅拷贝和深拷贝的实现

- 深拷贝

在 JavaScript 中实现深拷贝（Deep Copy）可以使用多种方式，下面给出两种常见的实现方式：

1. 使用 JSON 序列化和反序列化
```javascript
function deepCopy(obj) {
  return JSON.parse(JSON.stringify(obj));
}
```

上述代码通过将对象转换为字符串，再将字符串转换为新的对象实现深拷贝。但是需要注意的是，使用 JSON.stringify 和 JSON.parse 的方式有一些限制：
- 无法拷贝函数、正则表达式、日期等特殊对象。
- 会忽略 undefined 属性。
- 会忽略对象的原型链。

2. 递归遍历对象属性
```javascript
function deepCopy(obj) {
  if (typeof obj !== 'object' || obj === null) {
    return obj;
  }

  let copy;
  if (Array.isArray(obj)) {
    copy = [];
    for (let i = 0; i < obj.length; i++) {
      copy[i] = deepCopy(obj[i]);
    }
  } else {
    copy = {};
    for (let key in obj) {
      // 这里是判断对象属性是否是自身的，并且使用call绑定在obj上，确保不会被原型链上的属性影响到
      if (Object.prototype.hasOwnProperty.call(obj, key)) {
        copy[key] = deepCopy(obj[key]);
      }
    }
  }

  return copy;
}
```

上述代码通过递归遍历对象属性，创建新的对象并复制属性值，实现了深拷贝。它能够处理包括数组、对象在内的各种情况。

需要注意的是，上述代码仅适用于处理普通对象和数组，对于包含函数、正则表达式等特殊对象的深拷贝，需要进一步的处理。

另外，深拷贝在处理嵌套层级较深或循环引用的对象时需要格外小心，避免进入无限递归的循环。

3. 使用第三方库 深拷贝的实现涉及到递归遍历对象的属性，并处理各种数据类型和特殊情况。为了简化开发过程，可以使用一些成熟的第三方库，如 lodash 的 cloneDeep 方法或 jQuery 的 extend 方法等。这些库提供了方便的深拷贝功能，可以处理多种数据类型和边界情况。

- 浅拷贝

在 JavaScript 中实现浅拷贝（Shallow Copy）可以使用多种方式，下面给出两种常见的实现方式：

1. 使用 Object.assign()
```javascript
function shallowCopy(obj) {
  return Object.assign({}, obj);
}
```

上述代码使用 `Object.assign()` 方法，将原始对象的属性复制到一个新的空对象中，实现了浅拷贝。但是需要注意的是，`Object.assign()` 只能复制对象的可枚举属性，并且只能进行浅拷贝。

2. 使用扩展运算符
```javascript
function shallowCopy(obj) {
  return { ...obj };
}
```

上述代码使用扩展运算符 `...`，将原始对象的属性复制到一个新的对象中，也实现了浅拷贝。扩展运算符会展开对象的可枚举属性，将它们复制到新对象中。

3. slice（适用于数组）
4. concat（适用于数组）

需要注意的是，浅拷贝只复制对象的第一层属性，如果对象的属性值是引用类型（如数组、对象等），则仍然是浅拷贝，即复制的是引用而非值。修改原始对象或新对象的引用类型属性会相互影响。

总结起来，浅拷贝可以使用 `Object.assign()` 方法或扩展运算符来实现，但需要注意其限制和对引用类型的处理。如果需要实现深拷贝，可以参考之前提到的深拷贝实现方式。

#### 14.js内存泄漏的几种情况

####15.防抖和节流的区别，以及手写实现

#### 16.event loop事件循环

#### 17.Promise

- ```javascript
  // 这是类似于promise中allSettled的解决方法,所有任务执行完再输出。可控制并发任务数量，且每个任务按顺序执行
  function runTasksInOrder(tasks, limit) {
    const results = new Array(tasks.length);
    let activeCount = 0;
    let currentIndex = 0;
  
    return new Promise((resolve, reject) => {
      const next = () => {
        if (currentIndex >= tasks.length && activeCount === 0) {
          // 全部完成
          results.forEach(r => console.log(r)); // 顺序输出
          return resolve(results);
        }
  
        while (activeCount < limit && currentIndex < tasks.length) {
          const taskIndex = currentIndex++;
          const task = tasks[taskIndex];
          activeCount++;
  
          task()
            .then(res => {
              results[taskIndex] = res;
            })
            .catch(reject)
            .finally(() => {
              activeCount--;
              next();
            });
        }
      };
  
      next();
    });
  }
  
  // 模拟任务（不同耗时）
  const tasks = [
    () => new Promise(res => setTimeout(() => res('T1 done'), 300)),
    () => new Promise(res => setTimeout(() => res('T2 done'), 100)),
    () => new Promise(res => setTimeout(() => res('T3 done'), 150)),
    () => new Promise(res => setTimeout(() => res('T4 done'), 50)),
  ];
  
  runTasksInOrder(tasks, 2);
  
  // 第二种，将多个任务按照并发任务的限制 尽可能在最短时间内完成所有任务，并且按顺序**及时**输出结果（不是最后一起输出）
  function runTasksInOrderRealtime(tasks, limit) {
    const results = new Array(tasks.length);  // 只存结果
    let activeCount = 0;
    let currentIndex = 0;
    let nextPrintIndex = 0; // 下一个要输出的任务索引
  
    const tryPrint = () => {
      while (results[nextPrintIndex] !== undefined) {
        console.log(results[nextPrintIndex]);
        nextPrintIndex++;
      }
    };
  
    return new Promise((resolve, reject) => {
      const next = () => {
        if (currentIndex >= tasks.length && activeCount === 0) {
          return resolve(results);
        }
  
        while (activeCount < limit && currentIndex < tasks.length) {
          const taskIndex = currentIndex++;
          activeCount++;
  
          tasks[taskIndex]()
            .then(res => {
              results[taskIndex] = res;
              tryPrint();
            })
            .catch(reject)
            .finally(() => {
              activeCount--;
              next();
            });
        }
      };
  
      next();
    });
  }
  
  // 模拟不同耗时的任务
  const tasks = [
    () => new Promise(res => setTimeout(() => res('T1 done'), 300)),
    () => new Promise(res => setTimeout(() => res('T2 done'), 100)),
    () => new Promise(res => setTimeout(() => res('T3 done'), 150)),
    () => new Promise(res => setTimeout(() => res('T4 done'), 50)),
  ];
  
  runTasksInOrderRealtime(tasks, 2).then(() => {
    console.log('All tasks completed');
  });
  
  ```

  ------
  
  # 📚 Promise 知识点整理
  
  ## 一、基本概念
  
  1. **定义**
     - `Promise` 是一个代表异步操作最终结果的对象。
     - 三种状态：
       - `pending`（进行中）
       - `fulfilled`（已成功）
       - `rejected`（已失败）
     - 状态一旦变更不可逆。
  2. **核心方法**
     - `new Promise((resolve, reject) => {...})`
     - `then(onFulfilled, onRejected)`
     - `catch(onRejected)`
     - `finally(onFinally)`
  
  ------
  
  ## 二、执行机制
  
  1. **立即执行**
     - `Promise` 构造函数内的代码会立即执行。
  2. **微任务机制**
     - `.then/.catch/.finally` 的回调会被放入 **微任务队列**（microtask queue）。
     - 微任务总是优先于下一个宏任务执行。
  3. **链式调用**
     - `then` 返回一个新的 `Promise`，支持链式调用。
     - 返回值规则：
       - 返回普通值 → 自动包装成 `Promise.resolve(值)`
       - 返回 `Promise` → 等待该 Promise 结果
       - 抛出错误 → 进入下一个 `catch`
  
  ------
  
  ## 三、常用静态方法
  
  1. `Promise.resolve(value)`
     - 将值包装成已完成的 Promise。
  2. `Promise.reject(reason)`
     - 返回一个已拒绝的 Promise。
  3. `Promise.all([p1, p2, ...])`
     - 并发执行，所有成功才成功；有一个失败则立即失败。
  4. `Promise.race([p1, p2, ...])`
     - 返回第一个完成的结果（无论成功或失败）。
  5. `Promise.allSettled([p1, p2, ...])`
     - 等待所有完成，返回每个结果的状态和数据。
  6. `Promise.any([p1, p2, ...])`
     - 只要有一个成功就返回；全失败则返回 `AggregateError`。
  
  ------
  
  ## 四、手写实现要点（Promise/A+）
  
  1. **状态机**：pending → fulfilled / rejected
  2. **then 链式调用**：每个 then 返回新 Promise
  3. **回调存储队列**：pending 时存起来，等状态变化再调用
  4. **异步执行**：回调必须放入微任务队列执行
  
  ------
  
  ## 五、与 async/await 的关系
  
  1. `async function` 默认返回一个 `Promise`。
  
  2. `await expr` 相当于：
  
     ```js
     Promise.resolve(expr).then(value => {
       // 继续执行
     });
     ```
  
  3. `await` 会暂停 async 函数，释放主线程，等 Promise resolve 后继续。
  
  ------
  
  ## 六、实战应用
  
  1. **封装异步请求**（接口调用）
  2. **并发控制**（限流器、任务队列）
  3. **重试机制**（失败自动重试）
  4. **超时控制**（请求超过时间自动失败）
  5. **顺序执行任务**（按顺序输出结果）
  
  ------
  
  ## 七、常见考点 / 易错点
  
  1. `Promise` 构造函数中 `resolve/reject` 立即执行
  2. `then` 总是异步（微任务），即使 `resolve` 是同步的
  3. 链式调用中异常会冒泡到最近的 `catch`
  4. `Promise.all` 失败一个就立刻失败
  5. `async/await` 语法糖，底层还是基于 Promise
  
  ------
  
  ✅ 这样你可以快速掌握：**概念 → 机制 → API → 手写 → 应用 → 面试考点**
  
  要不要我帮你再整理一份 **「Promise + async/await 高频面试题清单」**？这样可以拿来直接刷题练习。

#### 18.事件委托

- # JavaScript 中的事件委托与事件冒泡机制总结

  ---

  ## 📌 一、什么是事件冒泡（Event Bubbling）

  事件冒泡是 DOM 事件传播的**第三阶段**，指的是：

  > 当某个元素触发事件时，该事件会从目标元素开始，沿着 DOM 树**逐层向上传播**到 `document`。

  ### 🧠 事件传播的三个阶段：

  1. **捕获阶段**：从 `window` → `document` → 最外层 DOM → ... → 目标元素
  2. **目标阶段**：事件到达真正触发的目标元素
  3. **冒泡阶段**：从目标元素开始，向上传播到其父级、祖先、直到 `document`

  📌 **默认事件是在冒泡阶段被触发的。**

  ---

  ## ✅ 示例：事件冒泡过程

  HTML 结构：
  ```html
  <div id="parent">
    <button id="child">Click Me</button>
  </div>
  ```

  JavaScript：

  ```js
  document.getElementById('child').addEventListener('click', () => {
    console.log('child clicked');
  });
  document.getElementById('parent').addEventListener('click', () => {
    console.log('parent clicked');
  });
  ```

  点击按钮后输出：

  ```
  child clicked
  parent clicked
  ```

  ------

  ## 🧠 二、事件冒泡的设计原理

  ### 为什么浏览器要设计“事件冒泡”机制？

  1. **模拟现实世界中的传播模型**
     - 比如：你拍水面，水波会向外扩散，冒泡模拟这种“层层传递”。
  2. **提高性能**
     - 避免每个子节点都绑定监听器。
  3. **增强灵活性**
     - 父组件可以统一管理所有子组件的事件。
  4. **早期 IE 先实现了冒泡，W3C 标准保留了它**

  ------

  ## 💡 三、什么是事件委托（Event Delegation）

  事件委托是一种编程模式，指的是：

  > 将某类子元素的事件绑定，**统一委托给它们的公共祖先元素**处理，利用事件冒泡机制。

  ### ✅ 场景：

  - 列表中有大量 `<li>` 项目，每个都可点击
  - 而你只在 `<ul>` 上绑定监听器

  ```js
  document.getElementById('list').addEventListener('click', function (e) {
    if (e.target.tagName === 'LI') {
      console.log('点击了：', e.target.textContent);
    }
  });
  ```

  ------

  ## 🔁 四、事件委托的原理关系图

  ```
  事件触发 (child 元素点击)
     ↓
  事件冒泡到父元素
     ↓
  父元素监听器触发
     ↓
  根据 e.target 判断事件源
     ↓
  执行对应逻辑（委托处理）
  ```

  ------

  ## 📈 五、事件委托的优点

  | 优点           | 说明                                             |
  | -------------- | ------------------------------------------------ |
  | 减少内存消耗   | 不需要为每个子元素都绑定监听器                   |
  | 动态元素支持   | 新增的 DOM 元素也能自动响应事件，无需重新绑定    |
  | 更方便管理     | 所有逻辑集中在父元素一个监听器中实现，代码更可控 |
  | 适用于高频操作 | 比如：分页表格、动态列表、按钮池等频繁更新场景   |

  ------

  ## ❗ 六、事件委托的注意事项

  1. **依赖事件冒泡机制**，如果事件不能冒泡（如 `blur`、`focus`），就不能用事件委托；
  2. **`e.target` 可能不是你期望的元素**，需要加条件判断；
  3. 委托层级不宜太高，否则判断逻辑会变复杂；
  4. 嵌套结构中注意 `stopPropagation()` 会中断冒泡链。

  ------

  ## 🔍 七、事件冒泡与事件委托的区别与联系

  | 项目         | 事件冒泡                   | 事件委托                              |
  | ------------ | -------------------------- | ------------------------------------- |
  | 本质         | 浏览器事件传播机制的一部分 | 一种利用事件冒泡的编码技巧            |
  | 由谁决定     | 浏览器内部机制             | 开发者手动实现                        |
  | 是否自动发生 | 自动发生                   | 需要开发者手动委托                    |
  | 是否依赖冒泡 | 本身就是冒泡               | ✅ 完全依赖冒泡机制                    |
  | 应用目的     | 定义事件传播路径           | 提高性能、简化代码、支持动态 DOM 元素 |

  ------

  ## ✅ 小结口诀

  > “**事件冒泡**是浏览器行为，事件从里向外传；
  >  **事件委托**是开发技巧，借助冒泡来少绑。”

  ------

- 以下是事件委托的相关内容，需要掌握

  ## 🧩 1. `e.target` vs `e.currentTarget`

  这两个属性常在事件委托中用来判断事件来源和处理者。

  | 属性              | 说明                                               |
  | ----------------- | -------------------------------------------------- |
  | `e.target`        | 实际**触发事件的元素**（比如你点了哪个具体的按钮） |
  | `e.currentTarget` | 当前绑定事件监听器的**元素本身**                   |

  ### 示例：

  ```js
  document.getElementById('parent').addEventListener('click', function (e) {
    console.log('target:', e.target);
    console.log('currentTarget:', e.currentTarget);
  });
  <div id="parent">
    <button id="child">Click Me</button>
  </div>
  ```

  如果点击的是按钮：

  ```
  target: <button>        // 实际被点击的元素
  currentTarget: <div>    // 当前绑定事件处理器的元素
  ```

  ### 💡 在事件委托中：

  - 你通常会用 `e.target` 来判断是哪一个子元素触发了事件；
  - 而 `e.currentTarget` 是你绑定事件的父级容器。

  ------

  ## 🛑 2. `stopPropagation()`

  ### 作用：

  > 阻止事件继续冒泡到更高层的祖先元素。

  ```js
  child.addEventListener('click', function (e) {
    e.stopPropagation();
    console.log('child clicked');
  });
  ```

  - 事件会在这里停止，不会继续冒泡到父元素。

  ### ⚠️ 注意：

  - **不会阻止当前元素上的其他监听器执行**；
  - **不影响默认行为（如跳转）**，要阻止默认行为请用 `preventDefault()`。

  ------

  ## ❌ 3. `preventDefault()`

  ### 作用：

  > 阻止事件的默认行为（但事件本身仍会继续传播）。

  常见用途：

  - 阻止 `<a>` 标签跳转
  - 阻止 `<form>` 自动提交
  - 阻止右键菜单出现（`contextmenu`）

  ```js
  document.querySelector('a').addEventListener('click', function (e) {
    e.preventDefault();
    console.log('链接被点击但没有跳转');
  });
  ```

  ------

  ## ⚙️ 4. `addEventListener` 的第三个参数 `{ capture: true }`

  这是用来控制事件监听器在哪个阶段被触发的。

  ```js
  element.addEventListener('click', handler, true); // 捕获阶段触发
  element.addEventListener('click', handler, false); // 冒泡阶段（默认）
  ```

  ### 说明：

  - `{ capture: true }` → 在**捕获阶段触发**
  - `{ capture: false }` 或省略 → 在**冒泡阶段触发**

  ### 🧠 什么时候用？

  - 少数情况下你想要“**抢先处理事件**”，可以在捕获阶段处理。
  - 大多数时候使用默认的冒泡阶段即可。

  ------

  ## 🧭 5. 哪些事件可以冒泡，哪些不可以？

  ### ✅ 会冒泡的常见事件：

  | 类别     | 事件                                                       |
  | -------- | ---------------------------------------------------------- |
  | 鼠标事件 | `click`, `dblclick`, `mousedown`, `mouseup`, `contextmenu` |
  | 表单事件 | `change`, `input`, `submit`                                |
  | 键盘事件 | `keydown`, `keyup`, `keypress`                             |
  | 其他     | `focusin`, `focusout`, `error`（部分冒泡）                 |

  ### ❌ 不会冒泡的事件：

  | 类别     | 事件                     | 说明                         |
  | -------- | ------------------------ | ---------------------------- |
  | 表单事件 | `focus`, `blur`          | 改用 `focusin` 和 `focusout` |
  | 媒体事件 | `play`, `pause`, `ended` | 多为视频/音频相关            |
  | 动画事件 | `transitionend`（部分）  | 要注意兼容性                 |

  ------

  ## ✅ 小结口诀

  | 项目                | 建议记住的点                        |
  | ------------------- | ----------------------------------- |
  | `e.target`          | 谁触发事件                          |
  | `e.currentTarget`   | 谁在监听处理                        |
  | `stopPropagation()` | 阻止事件继续往上冒泡                |
  | `preventDefault()`  | 阻止默认浏览器行为（如跳转）        |
  | `{ capture: true }` | 在捕获阶段触发监听器                |
  | 冒泡事件            | click/input/change 等多数交互型事件 |
  | 不冒泡事件          | focus/blur/play 等特殊事件          |

#### 19.对象扁平化与还原

- ## 题目一：对象扁平化（flatten）

  ### 要求

  实现一个函数 `flatten(obj)`，将多层嵌套的对象转为一层对象，键名用路径表示。

  ### 示例

  ```js
  const input = {
    a: { b: { c: 1 }, d: 2 },
    e: 3
  };
  
  flatten(input);
  // => { "a.b.c": 1, "a.d": 2, "e": 3 }
  ```

  ### 知识点

  1. **递归**：遇到对象继续展开，否则直接赋值。
  2. **键名拼接**：
     - 对象属性 → `parent.key`
     - 数组索引 → `parent[index]`
  3. **类型判断**：
     - `typeof value === 'object' && value !== null` → 继续递归
     - `Array.isArray(value)` 区分数组
  4. **属性遍历方式**：
     - `for...in` + `hasOwnProperty`（会包含原型链属性）
     - `Object.keys/values/entries`（只取自身属性，推荐）

  ------

  ## 题目二：对象还原（unflatten）

  ### 要求

  实现一个函数 `unflatten(obj)`，将扁平对象还原为嵌套对象。

  ### 示例

  ```js
  const flat = {
    "a.b.c": 1,
    "a.d": 2,
    "arr[0].name": "Tom",
    "arr[0].age": 18,
    "arr[1].name": "Jerry"
  };
  
  unflatten(flat);
  // => { a: { b: { c: 1 }, d: 2 }, arr: [ { name: "Tom", age: 18 }, { name: "Jerry" } ] }
  ```

  ### 知识点

  1. **路径解析**

     - 简单版：`"a.b.c".split('.') → ["a","b","c"]`

     - 高级版：解析 `arr[0].name` → `["arr","0","name"]`

       ```js
       function parsePath(path) {
         return path.replace(/\[(\w+)\]/g, '.$1').split('.');
       }
       ```

  2. **引用赋值**

     - `current` 指针在 `result` 内游走并修改，最终值写入 `result`。
     - 通过引用修改嵌套对象 → `result` 自动更新。

  3. **自动建层**

     - 如果下一层是数字 → 建 `[]`
     - 否则 → 建 `{}`
     - 最后一层直接赋值

  ------

  ## 🔹 对象遍历方式总结

  | 方法                   | 是否遍历原型链 | 是否返回数组 | 顺序是否固定  | 支持 Symbol 键 |
  | ---------------------- | -------------- | ------------ | ------------- | -------------- |
  | `for...in`             | ✅（需过滤）    | ❌            | 不完全保证    | ❌              |
  | `Object.keys(obj)`     | ❌              | ✅            | ✅（插入顺序） | ❌              |
  | `Object.values(obj)`   | ❌              | ✅            | ✅             | ❌              |
  | `Object.entries(obj)`  | ❌              | ✅            | ✅             | ❌              |
  | `Reflect.ownKeys(obj)` | ❌              | ✅            | ✅             | ✅              |

  👉 实际开发中，推荐 `Object.keys / entries`，避免原型链干扰。

  ------

  ## 🔹 flatten ↔ unflatten 双向验证

  ```js
  const obj = {
    a: { b: { c: 1 }, d: 2 },
    arr: [{ name: "Tom", age: 18 }, { name: "Jerry" }]
  };
  
  const flat = flatten(obj);
  console.log(flat);
  
  const restored = unflatten(flat);
  console.log(restored);
  
  // 验证：deepEqual(obj, restored) === true
  ```

  ------

  ```javascript
  // 这是进阶版本的扁平化和还原
  特点
  
  ✅ 支持 对象 / 数组
  
  ✅ 支持 arr[0].xxx 风格路径
  
  ✅ 支持 Symbol 键 / 不可枚举属性
  
  ✅ 支持 循环引用检测（返回 "__CIRCULAR__"）
  
  ✅ 支持 Set / Map / Date / RegExp 类型保留
  
  ✅ 可配置化（分隔符、是否用中括号、是否保留类型信息）
  // =====================
  // 🔹 Flatten
  // =====================
  function flatten(obj, parentKey = '', result = {}, options = {}) {
    const {
      arrayBrackets = true,     // 是否使用 arr[0] 风格（false = arr.0）
      separator = '.',          // 对象分隔符
      detectType = true,        // 是否保留类型信息 (Date, RegExp, etc.)
      seen = new WeakSet()      // 防止循环引用
    } = options;
  
    if (obj !== null && typeof obj === 'object') {
      if (seen.has(obj)) {
        // ⛔ 避免循环引用
        result[parentKey] = '__CIRCULAR__';
        return result;
      }
      seen.add(obj);
  
      if (Array.isArray(obj)) {
        obj.forEach((item, index) => {
          const newKey = parentKey
            ? arrayBrackets
              ? `${parentKey}[${index}]`
              : `${parentKey}${separator}${index}`
            : `${arrayBrackets ? `[${index}]` : index}`;
  
          flatten(item, newKey, result, options);
        });
      } else if (obj instanceof Date) {
        result[parentKey] = detectType ? { __type: 'Date', value: obj.toISOString() } : obj.toISOString();
      } else if (obj instanceof RegExp) {
        result[parentKey] = detectType ? { __type: 'RegExp', value: obj.toString() } : obj.toString();
      } else if (obj instanceof Set) {
        result[parentKey] = detectType ? { __type: 'Set', value: [...obj] } : [...obj];
      } else if (obj instanceof Map) {
        result[parentKey] = detectType ? { __type: 'Map', value: [...obj.entries()] } : [...obj.entries()];
      } else {
        Object.getOwnPropertyNames(obj).forEach(key => {
          const newKey = parentKey ? `${parentKey}${separator}${key}` : key;
          flatten(obj[key], newKey, result, options);
        });
        Object.getOwnPropertySymbols(obj).forEach(sym => {
          const symKey = parentKey ? `${parentKey}${separator}${sym.toString()}` : sym.toString();
          flatten(obj[sym], symKey, result, options);
        });
      }
    } else {
      result[parentKey] = obj;
    }
  
    return result;
  }
  
  
  // =====================
  // 🔹 Unflatten
  // =====================
  function unflatten(flatObj, options = {}) {
    const {
      separator = '.',
      arrayBrackets = true
    } = options;
  
    const result = {};
  
    const parsePath = (path) => {
      if (arrayBrackets) {
        // arr[0].name -> ["arr", "0", "name"]
        return path.replace(/\[(\w+)\]/g, `${separator}$1`).split(separator);
      } else {
        return path.split(separator);
      }
    };
  
    for (let flatKey in flatObj) {
      if (!flatObj.hasOwnProperty(flatKey)) continue;
      const keys = parsePath(flatKey);
      let current = result;
  
      keys.forEach((key, index) => {
        const isLast = index === keys.length - 1;
        const nextKey = keys[index + 1];
        const value = flatObj[flatKey];
  
        if (isLast) {
          // 类型还原
          if (value && typeof value === 'object' && value.__type) {
            switch (value.__type) {
              case 'Date': current[key] = new Date(value.value); break;
              case 'RegExp': current[key] = new RegExp(value.value.slice(1, -1)); break;
              case 'Set': current[key] = new Set(value.value); break;
              case 'Map': current[key] = new Map(value.value); break;
              default: current[key] = value.value;
            }
          } else {
            current[key] = value;
          }
        } else {
          if (!(key in current)) {
            const isArrayIndex = !isNaN(Number(nextKey));
            current[key] = isArrayIndex ? [] : {};
          }
          current = current[key];
        }
      });
    }
  
    return result;
  }
  
  
  ```

  

  ## 总结

  - **flatten** 考察递归、对象/数组遍历、键名拼接。
  - **unflatten** 考察字符串解析、动态建层、引用赋值。
  - **延伸知识点**：
    - 对象属性遍历方式差异
    - `current` 作为“游标”修改 `result` 的原理
    - 路径语法风格（`a.0.b` vs `a[0].b`）
