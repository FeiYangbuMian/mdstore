## 其他

### 项目构建

vite代替vue-cli



<script setup>+TS+Volar

vscode插件Volar：https://blog.csdn.net/qq_41800366/article/details/120363622



### 关键字说明

副作用/作用 effect （我理解的触发器后的处理操作handler

依赖 dependency

订阅者 subscriber

​	副作用作为订阅者

SFC 单文件组件

## 基础

### 组合式API

响应式API：`ref()`和`reactive()`，创建响应式状态、计算属性、侦听器

生命周期钩子：`onMounted()` 和 `onUnmounted()`

依赖注入：`provide()`和`inject()`



ref()一般声明简单数据类型；

reactive()一般申明引用数据类型

​	数组的话，用arr.length=0清空（赋值会丢失响应式）对应的ref是xx.value=[]

但底层实现都是通过Proxy

### 模板语法

提示：没有参数的 v-bind 会将一个对象的所有属性都作为 attribute 应用到目标元素上

#### 指令

```vue
<button v-on:submit.prevent="onSubmit">submit</button>
<!-- Name:Argument.Modifiers="Value"  -->
```

##### 动态参数

通过[]，限制：小写字符串

```vue
<a :[attributeName]="url"> ... </a>
<a @[eventName]="doSomething">
```

### 响应式基础

全局API`nextTick()`：等待一个状态变化后的DOM更新完成

`setup()` ：专门用于组合式 API 的特殊钩子函数，`return`出需要的

但在单文件组件SFC中，可使用`<script setup>`

响应式对象其实是 JavaScript Proxy

劫持属性访问方式：Proxy，getter/setters（ref）

#### reactive()

创建一个响应式对象或数组，返回对象的Proxy

局限：

1. 仅对对象类型有效（对象、数组和 `Map`、`Set`），对原始类型无效。

2. 必须始终保持对该响应式对象的相同引用（Vue 的响应式系统是通过属性访问进行追踪的）。不能随意地“替换”一个响应式对象，会导致对初始引用的响应性连接丢失。

   失去响应式：属性赋值；解构至本地变量；将属性传入一个函数

局限原因：JavaScript没有可以作用于所有值类型的“引用”机制。



#### ref()

创建可以使用任何值类型的响应式ref，返回带`.value`属性的ref对象

无reactive的局限性，但是各种情况的默认解包，有点繁琐，实践再看看吧

解包：顶级变量在模板内自动解包

劫持属性访问方式：getter/setters

### 计算属性

根据其他值派生一个值

`computed()` 方法期望接收一个 getter 函数，返回值为一个**计算属性 ref**

```javascript
import { reactive, computed} from 'vue'
const author = reactive({
	name:'xx',
	books:['x','xx','xxx']
})
// 一个计算属性 ref，可使用 booksMessage或booksMessage.value
const booksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
```

**计算属性值会基于其响应式依赖被缓存**：一个计算属性仅会在其响应式依赖更新时才重新计算

**Vue 的计算属性会自动追踪响应式依赖**

注意：

1. 计算属性只做计算，不要在计算函数中做异步请求或者更改 DOM
2. 拒绝直接修改计算属性值。返回值应是只读，应该更新其所依赖的源状态以触发新的计算

#### 类与样式绑定

##### Attributes继承

多根元素，绑定class时

```Vue
<!-- MyComponent 模板使用 $attrs 时 -->
<p :class="$attrs.class">Hi!</p>
<!-- <p v-bind="$attrs">Hi!</p> -->
<span>This is a child component</span>

<MyComponent class="baz" />

<p class="baz">Hi!</p>
<span>This is a child component</span>
```

`<script setup>` 中使用 `useAttrs()` API 来访问一个组件的所有透传 attribute

或者`attrs` 会作为 `setup()` 上下文对象的一个属性暴露

### 条件渲染

`v-if` 有更高的切换开销， `v-show` 有更高的初始渲染开销

不推荐`v-if` 和 `v-for` 同时使用。若，那就`v-if` 会先被执行

### 列表渲染

in of都可以，尽量提供:key

v-for遍历对象属性时，顺序是根据Object.keys()的返回值

```Vue

<li v-for="(value, key, index) in myObject">
  {{ index }}. {{ key }}: {{ value }}
</li>
```

v-for接受整数值遍历，从1开始

```vue
<span v-for="n in 10">{{ n }}</span>
```

#### 数组变化侦测

变更方法：push() pop() shift() unshift() splice() sort() reverse()

不变方法：filter() concat() slice()

计算属性：不方便直接返回数据时，可在组件数据中调用计算方法

以防改变原数组：[...arr]先创建原数组副本

### 事件处理

`event` DOM原生事件，组件中用特殊变量`$event`

#### 事件修饰符

可链式，但要注意调用顺序

- .stop 停止传递
- .prevent 阻止默认行为
- .self 
- .capture 捕获模式 
- .once 
- .passive

#### 按键修饰符

.enter .tab .delete .esc .space .up .down .left .right .ctrl .alt .shift .meta

.exact修饰符，用于组合键

#### 鼠标按键修饰符

.left .right .middle

### 表单输入绑定

#### 复选框

`true-value` 和 `false-value` 是 Vue 特有的 attributes，仅支持和 `v-model` 配套使用

（？没看懂用处

#### 修饰符

v-model.lazy .number .trim

```vue
<!-- 在 "change" 事件后同步更新而不是 "input" -->
<input v-model.lazy="msg" />
<input v-model.number="age" />
```

### 生命周期

onMounted()、onUpdated() 和 onUnmounted()

onBeforeMount()、onBeforeUpdate()、onBeforeUnmount()

onErrorCaptured()

onActivated() onDeactivated()

Dev only：onRenderTracked() onRenderTriggered() 

<img src="D:\Pictures\mdImg\lifecycle.16e4c08e.png" alt="lifecycle.16e4c08e" style="zoom:80%;" />

### 侦听器

响应式地执行有副作用的回调

#### watch()

```javascript
import { ref, watch } from 'vue'
watch(val,(newValue,oldValue)=>{})
```



参一 数据源source：

- 一个ref（包括计算属性）
- 一个响应式对象（不能直接侦听响应式对象的属性值，需要用一个返回该属性的 getter 函数
- 一个getter函数（`()=>val`）
- 多个数据源组成的数组

参二 callback（副作用effect的回调

别的参（选项式API也有

​	{deep:true}（慎用，会深层遍历，耗性能）

​	{flush: 'post'} 侦听器回调中能访问被 Vue 更新之后的DOM（后置刷新

默认懒执行：数据源变化才会执行回调

​	选项式API中解决方法：{immediate: true}强制立即执行

​	组合式API中解决方法：watchEffect()函数

#### watchEffect()

```javascript
import { ref, watchEffect } from 'vue'
watchEffect(()=>{xxx})
```

立即执行

**Vue 的会自动追踪副作用的依赖关系**，自动分析出响应源（类似计算属性行为

？没看懂用法区别（大概就是immediate:true的语法糖？

区别：

- `watch`追踪明确侦听的数据源
- `watchEffect`在副作用发生期间追踪依赖

#### 停止侦听器

调用 `$watch()` /`watch` /`watchEffect`返回的函数

### 模板引用

特殊的 `ref` attribute

组合式API中，为了获得模板引用，需要声明同名ref

```vue
<script setup>
import { ref, onMounted, watchEffect } from 'vue'
const input = ref(null)

watchEffect(() => {
  if (input.value) {
    input.value.focus()
  } else {
    // 此时还未挂载，或此元素已经被卸载（例如通过 v-if 控制）
  }
})
</script>

<template>
  <input ref="input" />
</template>

<script>
export default {
  setup() {
    const input = ref(null)
    return {
      input
    }
  }
}
</script>
```

#### 组件上的Ref

选项式API是组件实例this，父组件对子组件的每个属性和方法都有完全访问权

`<script setup>`组件默认私有，可通过`defineExpose`显示暴露

## 深入组件

### 注册

`<script setup>`中，导入的组件可以直接在模板中使用，无需注册

否则需要`components`选项显式注册

建议大驼峰格式注册

### props与emit

`<script setup>`通过`defineProps`、`defineEmits`（不需要显性导入。`defineProps` 会返回一个对象，其中包含了可以传递给组件的所有 props。`defineEmits`返回一个等同于 `$emit` 方法的 `emit` 函数。

```vue
<script setup>
  defineProps(['title'])
  defineEmits(['enlarge-text'])
  // 或
  const props = defineProps(['title'])
  console.log(props.title)
  const emit = defineEmits(['enlarge-text'])
  emit('enlarge-text')
</script>
<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
```

选项式用`props`，`setup()`参一为`props`，参二为`emit`函数

```javascript
export default {
  props: ['title'],
  emits: ['enlarge-text'],
  setup(props, ctx) {
      console.log(props.title)
      ctx.emit('enlarge-text')
  }
}
```

prop名字camelCase小驼峰，模板上改为kebab-case短横线

使用一个对象绑定多个prop

​	v-bind="obj"等价于 :key="obj.key"
