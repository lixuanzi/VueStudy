# Day01

## 引入

```html
// 开发版本
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

// 生产版本
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14"></script>

// 本地 Vue.js
https://v2.cn.vuejs.org/v2/guide/installation.html
```

## API

**v-text**：更新元素的 `textContent`。如果要更新部分的 `textContent`，需要使用 `{{ Mustache }}` 插值。

```html
// 示例
<span v-text="msg"></span>
<!-- 和下面的一样 -->
<span>{{msg}}</span>
```

**v-html**：更新元素的 `innerHTML`。**注意：内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译**。如果试图使用 `v-html` 组合模板，可以重新考虑是否通过使用组件来替代。

```html
<div v-html="msg"></div>

<script>
    const app = new Vue({
        el: '#app',
        data:{
            msg:`<h1>大家好</h1>`
       }
    })
</script>
```

**v-show**：根据表达式之真假值，切换元素的 `display` CSS property。当条件变化时该指令触发过渡效果。

**v-if 和 v-show**

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

**v-for**：基于源数据多次渲染元素或模板块。此指令之值，必须使用特定语法 `alias in expression`，为当前遍历的元素提供别名：

```html
<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>

根据数组索引指定别名（或者用于对象的键）
<div v-for="(item, index) in items"></div>
<div v-for="(val, key) in object"></div>
<div v-for="(val, name, index) in object"></div>
```

**v-on**：

- `.stop` - 调用 `event.stopPropagation()`。
- `.prevent` - 调用 `event.preventDefault()`。
- `.capture` - 添加事件侦听器时使用 capture 模式。
- `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
- `.native` - 监听组件根元素的原生事件。
- `.once` - 只触发一次回调。
- `.left` - (2.2.0) 只当点击鼠标左键时触发。
- `.right` - (2.2.0) 只当点击鼠标右键时触发。
- `.middle` - (2.2.0) 只当点击鼠标中键时触发。
- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

```html
<!-- 方法处理器 -->
<button v-on:click="doThis"></button>

<!-- 动态事件 (2.6.0+) -->
<button v-on:[event]="doThis"></button>

<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 缩写 -->
<button @click="doThis"></button>

<!-- 动态事件缩写 (2.6.0+) -->
<button @[event]="doThis"></button>

<!-- 停止冒泡 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认行为 -->
<button @click.prevent="doThis"></button>

<!-- 阻止默认行为，没有表达式 -->
<form @submit.prevent></form>

<!--  串联修饰符 -->
<button @click.stop.prevent="doThis"></button>

<!-- 键修饰符，键别名 -->
<input @keyup.enter="onEnter">

<!-- 键修饰符，键代码 -->
<input @keyup.13="onEnter">

<!-- 点击回调只会触发一次 -->
<button v-on:click.once="doThis"></button>

<!-- 对象语法 (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

**v-bind**：动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。

在绑定 `class` 或 `style` attribute 时，支持其它类型的值，如数组或对象。可以通过下面的教程链接查看详情。

在绑定 prop 时，prop 必须在子组件中声明。可以用修饰符指定不同的绑定类型。

没有参数时，可以绑定到一个包含键值对的对象。注意此时 `class` 和 `style` 绑定不支持数组和对象。

```html
<!-- 绑定一个 attribute -->
<img v-bind:src="imageSrc">

<!-- 动态 attribute 名 (2.6.0+) -->
<button v-bind:[key]="value"></button>

<!-- 缩写 -->
<img :src="imageSrc">

<!-- 动态 attribute 名缩写 (2.6.0+) -->
<button :[key]="value"></button>

<!-- 内联字符串拼接 -->
<img :src="'/path/to/images/' + fileName">

<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]"></div>

<!-- style 绑定 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>

<!-- 绑定一个全是 attribute 的对象 -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- 通过 prop 修饰符绑定 DOM attribute -->
<div v-bind:text-content.prop="text"></div>

<!-- prop 绑定。“prop”必须在 my-component 中声明。-->
<my-component :prop="someThing"></my-component>

<!-- 通过 $props 将父组件的 props 一起传给子组件 -->
<child-component v-bind="$props"></child-component>

<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```

**v-model**：在表单控件或者组件上创建双向绑定。细节请看下面的教程链接。

- `.number` - 输入字符串转为有效的数字
- `.trim` - 输入首尾空格过滤

**一些数组的相关操作**

```js
// filter 根据条件保留满足条件的对应项，同时返回一个新数组
// 过滤(需要过滤的内容 => 条件) 通常用于删除操作
this.bookList=this.bookList.filter(item => item.id!==id)


// 数组某一项求和使用reduce
// reduce((阶段性结果, 每一项) => 求和逻辑, 起始值)
let totalValue = this.list.reduce((sum, item) => sum+item.num, 0)

// 截取字符串 slice
this.firstName=this.tmpName.slice(0,1)
this.lastName=this.tmpName.slice(1)
```

# Day02

## 选项/数据

**computed**：计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。

注意如果你为一个计算属性使用了箭头函数，则 `this` 不会指向这个组件的实例，不过你仍然可以将其实例作为函数的第一个参数来访问。

```js
var vm = new Vue({
  data: { a: 1 },
  computed: {
    // 仅读取
    aDouble: function () {
      return this.a * 2
    },
    // 读取和设置
    aPlus: {
      get: function () {
        return this.a + 1
      },
      set: function (v) {
        this.a = v - 1
      }
    }
  }
})
vm.aPlus   // => 2
vm.aPlus = 3
vm.a       // => 2
vm.aDouble // => 4
```

**watch**：一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 `$watch()`，遍历 watch 对象的每一个属性。

```js
var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: {
      f: {
        g: 5
      }
    }
  },
  watch: {
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // 方法名
    b: 'someMethod',
    // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    },
    // 该回调将会在侦听开始之后被立即调用
    d: {
      handler: 'someMethod',
      immediate: true
    },
    // 你可以传入回调数组，它们会被逐一调用
    e: [
      'handle1',
      function handle2 (val, oldVal) { /* ... */ },
      {
        handler: function handle3 (val, oldVal) { /* ... */ },
        /* ... */
      }
    ],
    // watch vm.e.f's value: {g: 5}
    'e.f': function (val, oldVal) { /* ... */ }
  }
})
vm.a = 2 // => new: 2, old: 1
```

**ajax**：

```js
watch: {
        obj: {
      deep: true, // 监视深度
      immediate: true, // 即可执行
      handler(newValue) {
        clearTimeout(this.timer)
        this.timer = setTimeout(async () => {
           const res = await axios({
             url: 'https://applet-base-api-t.itheima.net/api/translate',
             params: newValue
            })
         this.result = res.data.data
         console.log(res.data.data)
        }, 300)
       }
      }
```

## Day03

## 生命周期函数

**创建阶段（响应式数据）**：

beforeCreate:

created（发送初始化请求）:

**挂载阶段（渲染模板）**：

beforeMount:

mounted（操作Dom）:

**更新阶段（修改数据，更新视图）**：

boforeUpdate:

update:

**销毁阶段（销毁实例）**：

boforeDestory（释放Vue以外的资源 延时器、定时器）:

destoryed:

## Echarts可视化图表

官方地址：https://echarts.apache.org/examples/zh/editor.html?c=pie-simple

官方文档：https://echarts.apache.org/handbook/zh/how-to/data/dynamic-data

### 创建Vue项目

- 安装node：https://nodejs.org/en/download
- 配置环境变量及相关操作：https://blog.csdn.net/zimeng303/article/details/112167688
- 具体创建步骤

1. 全局安装（一次）：yarn global add @vue/cli 或 npm i @vue/cli -g
2. 查看 Vue 版本：vue --version
3. 创建架子项目：vue create peoject-name (项目名-不能用中文)
4. 启动项目：yarn serve 或 npm run serve (找到package.json)

### 目录结构

![image-20230822220016263](https://img.ixuanzi.com/images/typora/image-20230822220016263.png)
