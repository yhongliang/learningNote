### 一、Vue3基础

#### ref函数

对于基本数据类型是进行Object.defineproperty数据劫持，从而设置get和set方法，实现响应式数据，和vue2是一样的。但是对于对象来说是使用proxy进行代理实现响应式，vue3中将使用proxy的操作封装在一个名为reactive的函数中。vue2中原来的响应式是修改了但是vue没发现，也就是没监听到，比如新增和修改属性或者数组下标修改数据 . 

基本的 数据类型都用`ref`，对象类的就用`reactive`，当然对象类的也可以用ref，但是不建议，对象类的数据用ref的时候，ref会偷偷的求助于reactive函数，所以还不如直接用reactive函数去包住对象，reactive处理对象是深层次的遍历，使其变成响应式。同理，数组是特殊的对象，所以数组也用reactive来处理变成响应式。内部都是基于es6的proxy实现，通过代理对象操作源数据对象进行操作。

```vue
setup(props,context){
	// 这里context是上下文，上下文的可以理解为此时此刻正好需要一些东西，正好都整理好了，集合成context
}
```

**将panjiachen的技术文档中的内容提炼为自己项目的技术文档**

将小程序发布一下，记录下流程。

#### 响应式

- 数组中的方法，能触发响应式的是push，pop，shift，unshift，sort，splice，reverse，这几种之所以能触发响应式，是因为他们会改变数组本身，vue能监测到数组自身变化。而其他的不改变数组自身的方法，例如，slice，filter，concat等就不能触发响应式，因为他们不会改变数组自身，他们会生成一个新数组，使用时需要将它重新赋值

  ```vue
  // 不能改变数组自身的这样写才可以触发响应式
  items.value = items.value.filter((item) => item.message.match(/Foo/))
  ```

- 因为上述提到的七种方法会改变原数组，所以在某些情况下使用时需要先将其复制一份，例如在对数组进行排序时

- 在需要页面对数据变化做出相应变化时，就要让vue能监测到数据变化，那么对应的数据就要能让vue监测到其变化。

  - ref或reactive声明基本数据类型和引用数据类型

  - 声明引用数据类型时需要注意，若对象深层的某个属性或者数组的某一项中某个属性对数据有所依赖，就不能单纯的用ref或reactive声明它，例如setup中声明了之后，它就是静态的。vue只能检测到其本身的变化，内部变化无法检测。此时可以使用computed构建整个数组或对象。若还需要ref或reactive，就用watch监听内部需要依赖的数据，然后对整个数据进行更新。

  - ```vue
    // 此处为案例
    state: () => ({
        economicIndicatorData: <EconomicIndicator>{}, // 项目统计对象
        areaList: <AreaInfo[]>[], // 区域列表
        projctStatusData: <ProjctStatusInfo>{}, // 右侧区域统计
    })
    actions: {
        GetAreaReportData() {
          return GetAreaReportApi().then((res: any) => {
            if (res.code === 200) {
              this.economicIndicatorData = res.data.economicIndicatorData;
              this.areaList = res.data.projectList.area_list;
              this.projctStatusData = res.data.projctStatusData;
    }. 这是我在pinia中的调取接口并且存储数据的方法
    
    <script lang="ts" setup>
    import useProjectStatisticStore from "@/store/projectStatistic";
    const useProjectStatistic = useProjectStatisticStore();
    const {
      projctStatusData,
    } = storeToRefs(useProjectStatistic);
    const { GetAreaReportData } = useProjectStatistic;
    onMounted(async () => {
      GetAreaReportData()
    });
    const statisticList = ref([
      { title: "在建项目", value: projctStatusData.value.underConstructionNum, color: "#1279FF", type: "underConstruction" },
      { title: "完工项目", value: projctStatusData.value.finishedNum, color: "#26C779", type: "finished" },
      { title: "收尾已结算", value: projctStatusData.value.end_checked, color: "#F2840F", type: "endChecked" },
      { title: "未结算", value: projctStatusData.value.end_uncheck, color: "#F2840F", type: "endUncheck" },
      { title: "暂停项目", value: projctStatusData.value.suspendNum, color: "#F02933", type: "suspended" },
      { title: "未开始", value: projctStatusData.value.notStartedNum, color: "#9255DE", type: "notStarted" },
      { title: "长期停工", value: projctStatusData.value.longTermShutdownNum, color: "#7B828C", type: "longTermShutdown" }
    ]);
    <script>
    <template>
    <div
                      class="project-statistic"
                      v-for="(item, index) in statisticList"
                      :key="index"
                      :data-type="item.type"
                    >
                      <StatisticsBlock
                        :title="item.title"
                        :value="item.value"
                        :unit="'个'"
                        :color="item.color"
                        :unit-text-size="'18px'"
                      />
                    </div>
    <template>
    这是在vue组件中调用方法，并将数据绑定在结构中，刷新页面后数据就不显示了
    ```

    此处为针对上述问题gpt的详细说明

    这是因为 **JavaScript 中的引用类型（对象或数组）和 Vue 3 的 reactivity 机制** 之间的关系导致的，主要有以下几个关键点：  

    ---

    ### **1. `ref` 创建的是一个静态引用**
    当你使用 `ref([])` 或 `ref({})` 声明一个响应式对象时，Vue 只会对 `ref` **本身的 `.value` 变化进行追踪**，但不会深度追踪它内部的属性变化。  

    #### **示例：静态 `ref` 的问题**
    ```ts
    const statisticList = ref([
      { title: "在建项目", value: projctStatusData.value.underConstructionNum }
    ]);
    ```
    这里 `statisticList` **初始化时就存入了 `projctStatusData.value.underConstructionNum` 的值**，它是一个 **数值（原始类型）**，之后 `projctStatusData.value` 变了，但 `statisticList.value` **不会自动更新**，因为 `ref` 只追踪 `statisticList.value` 本身，而不会深度追踪其内部的对象。  

    💡 **总结：**
    - `ref` **不会自动响应其子属性的变化**，只有 `statisticList.value` 本身的引用发生变化时，Vue 才会检测到更新。

    ---

    ### **2. `computed` 让数据始终依赖最新的 `projctStatusData`**
    我们改用 `computed` 解决这个问题：
    ```ts
    const statisticList = computed(() => [
      { title: "在建项目", value: projctStatusData.value?.underConstructionNum || 0 },
      { title: "完工项目", value: projctStatusData.value?.finishedNum || 0 }
    ]);
    ```
    #### **为什么 `computed` 能解决问题？**
    - `computed` **依赖 `projctStatusData.value`，当 `projctStatusData` 变化时，它会自动重新计算 `statisticList`**。
    - `computed` **是懒计算的**，只有 `projctStatusData.value` 发生变化时，Vue 才会重新计算 `statisticList`，避免不必要的性能损耗。

    💡 **总结：**
    - `computed` 适用于**依赖响应式数据**并需要**自动更新**的情况。

    ---

    ### **3. `watch` 监听 `projctStatusData` 并手动更新 `ref`**
    如果你仍然希望 `statisticList` 用 `ref([])`，你可以使用 `watch` 监听 `projctStatusData` 的变化，然后手动更新它：
    ```ts
    const statisticList = ref([]);
    
    watch(projctStatusData, (newVal) => {
      statisticList.value = [
        { title: "在建项目", value: newVal?.underConstructionNum || 0 },
        { title: "完工项目", value: newVal?.finishedNum || 0 }
      ];
    }, { immediate: true });
    ```
    #### **为什么 `watch` 能起作用？**
    - `watch` **监听 `projctStatusData` 的变化**，当数据从 API 更新后，它会**手动更新 `statisticList.value`**。
    - `immediate: true` **确保初次执行时也会赋值**，否则只有数据发生变化时才会触发。

    💡 **总结：**
    - `watch` 适用于**手动处理数据变化**的情况，比如在 `projctStatusData` 变化后**执行额外逻辑**。

    ---

    ## **结论**
    | 方案                | 是否自动更新       | 适用场景                       |
    | ------------------- | ------------------ | ------------------------------ |
    | `ref([])`           | ❌ **不会自动更新** | 静态数据（不会依赖响应式数据） |
    | `computed`          | ✅ **自动更新**     | 依赖**响应式数据**的场景       |
    | `watch` + `ref([])` | ✅ **手动更新**     | 需要执行**额外逻辑**的情况     |

    **推荐方案**：  
    ✔ **如果 `statisticList` 只是展示 `projctStatusData` 的数据，建议用 `computed`。**  
    ✔ **如果你需要手动控制更新逻辑，可以用 `watch` + `ref([])`。**  

    你现在的情况 `computed` 是最适合的，因为它让 `statisticList` **始终保持最新的 `projctStatusData` 数据**，不需要额外的 `watch` 逻辑。 🚀


### 二、Vue组件

#### 1.基本原则

- 组件要复用到很多地方，所以设计组件最核心的就是可扩展和贴合业务之间找到一个平衡点
- 尽量提供简便，满足大部分场景下零配置或极少配置可用，特殊场景也可以拓展自定义

#### 2.方案

1. 图片区域
   - 图片大小默认按设计图，不要props传入width和height，这会增加props的数量，可以给个默认的高度设计，然后走css调控，且css的设计层级短一点
   - 统一兜底图，留出兜底图自定义空间
   - 默认显示一张图片，可以使用插槽进行替换
   - 图片地址不单独传入，可以传入整体信息，例如商品信息，然后组件内容提取图片地址，也预留props传入url
2. 信息区域
   - 行数不固定，每行的内容如何传入
   - 一、整个由插槽插入，但是需独立编写html和css，可扩展性最强
   - 二、每个位置分别用插槽，想用哪个插槽就写哪个，可扩展性一般，不需要写布局了
   - 另外信息区域如果分别主信息区域和额外信息区域，主信息区域长度过长需要换行显示，**可以根据使用的插槽的数量来判断是否分行显示内容**
3. 按钮
   - 预留基础点击行为
   - 无特点的，直接插槽
   - 预留基础错误点击行为
4. 数据格式
   - 注意响应式，所谓数据驱动视图，通过设计合适的数据结构去通知组件，需要怎样的页面。数据的灵活变化让组件更灵活。

#### 3.总结

1. 步骤：先分析ui，再分析步骤
2. 尽量让使用者不用传入太多的props和插槽
3. 思考是否能贴合业务做的更简便，既满足最大的便利也满足最大的扩展
4. 一定要留拓展接口
5. 行为记得两件事，开关和回调
6. 注意数据使用过程中有修改再使用的地方，注意引用类型的数据。需要先拷贝一份，可以对象解构构造新对象，或深拷贝一份。尽量不破坏原数据的引用

#### 全局方法注册与使用

