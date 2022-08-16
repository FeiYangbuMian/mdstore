## 英文

```
FBA 亚马逊物流 Fulfillment By Amazon
FBM ? FPM? MAP?
compliance ?
ASIN 亚马逊产品唯一编号 Amazon standard identification number
MSKU ?   SKU库存单位
Avg. 平均
pst 太平洋标准时间
merchandising 推销
inventory 详细目录
allocation配给量
remaining剩 余的
promotion 促销   coupon 优惠券
brand 品牌商标
purchase购买
reviews综述评论
rating 评级 等级
debounce防抖
claim 索赔 Reimbursed偿还 Expired过期的
currency 货币  currency symbol 货币符号
shipment 运输；货物
inbound 入境的；入站
profitability  盈利能力；收益性；利益率
YoY （Year Over Year）年同比
restock 补充货源
bullet point重点要点
manufacturers 制造商
rejected拒绝 fulfill 实现
review 审查检查
bull operation批量操作
parentage 出身
buybox 亚马逊黄金购物车
```



## CSS

```css
{
    display: grid;
	/* fr是flex简称？四列等分 */
	grid-template-columns: repeat(4, 1fr);  
	/* 两行，每行160px */
	grid-template-rows: repeat(2, 160px); 
	/* 间隙12px（row-gap column-gap） */
    grid-gap: 12px; 
}
```



哪里引入的public.scss？为什么可以用

找到了，在reset.scss里第一行`@import 'src/assets/scss/public';`



## JS代码片段

```javascript
// 检测传入的Promise是否处于Pending状态*
export const isPending = async(p) => {
 const empty = {}
 try {
  const resp = await Promise.race([p, empty])
  return resp === empty
 } catch (e) {
  return false
 }
}

// 检测 promise 状态
const PROMISE_STATE = {
  PENDING: 'pending',
  FULFILLED: 'fulfilled',
  REJECTED: 'rejected'
}
export function decidePromiseState(promise) {
  const inspectStr = util.inspect(promise)
  if (inspectStr.includes(PROMISE_STATE.PENDING)) {
    return PROMISE_STATE.PENDING
  } else if (inspectStr.includes(PROMISE_STATE.REJECTED)) {
    return PROMISE_STATE.REJECTED
  } else {
    return PROMISE_STATE.FULFILLED
  }
}
```

```javascript
// webpack中创建自己模块上下文 require.context
// 参数：要搜索的文件夹目录，是否搜子目录，匹配文件的正则
// 返回一个（require）函数，有三个属性：resolve(),keys(),id
// vue中的应用: router路由自动化挂载, 全局组件自动化注册, vuex状态自动化挂载
const modulesFiles = require.context('./modules', true, /\.js$/)

// you do not need `import app from './modules/app'`
// it will auto require all vuex module from modules file
const modules = modulesFiles.keys().reduce((modules, modulePath) => {
  // set './app.js' => 'app'
  const moduleName = modulePath.replace(/^\.\/(.*)\.\w+$/, '$1')
  const value = modulesFiles(modulePath)
  // value.default是它的值，可打印看出来，但是不懂为什么！
  modules[moduleName] = value.default
  return modules
}, {})

const store = new Vuex.Store({
  modules,
  getters
})
```

```javascript
/* 此方法用于打印请求时间 */
export const Logable = async function(query) {
  const host = query.baseURL || ''
  console.time(host + query.url)
  const res = await this({ ...query })
  console.timeEnd(host + query.url)
  return res
}
```

```javascript
// 引入多组件
import { BaseInfo, Tabs, Sales, Inventory, Claim, Merchandising, Reviews } from './components/index'
// index.js
export { default as BaseInfo } from './BaseInfo'
export { default as Tabs } from './Tabs'
export { default as Sales } from './Sales/index'
export { default as Inventory } from './Inventory/index'
export { default as Merchandising } from './Merchandising/index'
export { default as Claim } from './Claim/index'
export { default as Reviews } from './Reviews/index'

```



## Vue

深层选择器 ::v-deep



keep-alive 生命周期 **activated**



父子事件 `myChart`用于提取`chart`，执行其事件`legendselectchanged `



model选项 https://v2.cn.vuejs.org/v2/api/#model

​	允许一个自定义组件在使用v-model时定制prop和event（默认prop是value，event是input）

​	举例：commerceweb3p\src\components\PacAsinDetail\components\Tabs.vue



provide  inject ~~reject~~ 依赖注入

## 代码理解

this.$el 获取当前组件

this.$route.matched 路由自带匹配方法

1. local与vuex的common数据来源

seller下拉框通过sellerMarket接口获取数据，有hejeme, hansum, WINKO, real Flame Company

brand通过brandCategory接口获取数据，有HEJEME, HandSun

getOneCategory？

#### main.js

63-93行（看懂了 应该是qiankun子应用写法

找不到`__POWERED_BY_QIANKUN__`因为是子应用吗?

```javascript
// false 独立运行时,true 非独立运行时
Vue.prototype.$isQianKun = window.__POWERED_BY_QIANKUN__
```

##### 微前端-qiankun

官网：[qiankun](https://qiankun.umijs.org/zh/guide/getting-started)

案例用法：[案例](https://blog.csdn.net/weixin_60035749/article/details/125187696?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1-125187696-blog-121010082.pc_relevant_multi_platform_whitelistv1_exp2&spm=1001.2101.3001.4242.2&utm_relevant_index=4)

#### AppModule

AppModule文件夹下asinDetailMixin.js混入

负责处理全局处理 asinDetail 抽屉组件与新手引导功能

```javascript
// asinDetailDrawer事件绑定初始化 抽屉页
this.asinDetailDrawerBusInit()
// asinDetailDriver 事件绑定初始化 引导页
this.asinDetailDriverBusInit()
```

通信机制利用util/bus的总线$emit $on

被混入在app.vue里

引导插件drive.js https://kamranahmed.info/driver.js/

https://github.com/kamranahmedse/driver.js

```javascript
// asindetail 新手引导事件初始化
if (this.tableData?.length) {
    this.$nextTick(() => {
        utilBus.$emit('openAsinDetailDriver', '.' + this.tableConfig.rowClassName)
    })
}
```



#### util

util下的model.js什么意思？

用到了sellerMarketInitPro混入

#### mixins

##### chartResizeMixin

对所有chart的混入，调用`chart.resize()`

1. 在`window`发生`resize`事件时
2. `#pac_main`发生`transitionend`事件，CSS 完成过渡后触发

涉及防抖`debounce`，`keep-alive`生命周期`activated`及`deactivated`

##### customColumnsMixin

定制表头展示内容（列名），应该是（先跳过继续看下面

##### queryMixinOnlyCompare

没有用到，对应的`@/util/storage`也没有用到

##### ! sellerMarketInitPro

用于一些选择器的选项数据填充：Seller、Market、

选择器的值与选项填充，默认全选，seller有不同的market，seller变化market全选

用了model.js，作用：将值赋给对应的变量（链式对象等

```javascript
// 主页面在对应生命周期调用 sellerMarketInitPro 初始化seller market
// 主页面需要设置 watch监听 seller改变 调用 sellerMarketChange 改变market对应值

// 举例 C:\workplat\commerceweb3p\src\views\Reviews\Reviews\index.vue

modelUtil.model.call(this, {
    [this.sellerMarketModelConfig.marketOptionModel]:_.uniqBy(marketOptions, 'value')
})

// 组件data(){}
// seller market写入配置
sellerMarketModelConfig: {
    sellerOptionModel: 'publicFormOptions.seller',
    marketOptionModel: 'publicFormOptions.market'
},
// 公用表单下拉数据
publicFormOptions: {
   seller: [],
   market: []
},
```





#### const

定义变量键值对等

#### ？directive

自定义指令，定义了，有用到吗？

#### util

##### bus

用于绑定全局自定义事件$on $emit

$off在beforeDestroy钩子里

openAsinDetailDriver 新手引导事件

##### cancelRequestWrap

取消请求

```javascript
export function cancelRequestWrap(requestIntance) {
  return function requestOfCallCancel(config) {
    if (!config) {
      throw Error('Missing required parameters')
    }
    let cancelSouce = axios.CancelToken.source()
    let cancelSouceToken = cancelSouce.token
    const assCancelConfig = Object.assign({}, config, {
      cancelToken: cancelSouceToken
    })
    const apiInstancePro = requestIntance(assCancelConfig)
    // 绑定在生成的 Promise实例上
    apiInstancePro._cancel = () => {
      cancelSouce.cancel('CANCEL')
      cancelSouce = null
      cancelSouceToken = null
    }
    return apiInstancePro
  }
}
```

但是封装axios时，使用**requestOfCallCancel**与**$instance**一致，为什么？

应该：多出可手动取消功能

```javascript
// request.js
export const $instance = Instance
// 可取消请求实例
export const requestOfCallCancel = cancelRequestWrap(Instance)

// views\Business\Sales\components\TabTable\index.vue
// 如果上一个请求的 promise对象存在  就取消上一个请求
if (this.tableFetchData.apiPro && this.tableFetchData.apiPro._cancel) {
	this.tableFetchData.apiPro._cancel()
	this.tableFetchData.apiPro = null
}
```

看一看promise，为何需要return一个Promise？

https://www.cnblogs.com/hjj2ldq/p/9489598.html

##### common

一些格式化，对日期、数据、货币等的处理

day.js+插件QuarterOfYear（季度）

##### lib

数字加减乘数等运算

##### ? model

？好像是选择器键值对的处理，在`sellerMarketMixin`有用到

-> 将值赋给对应的变量（链式对象等，用处已知，代码还没看懂

##### public

`import util from 'util'`什么意思？util哪来的

一些共用方法吧

##### request

将`axios`封装为`$instance`，可以再过一下拦截器的封装

##### storage

存储信息，没注意看，应该是存于store或local等地吧

##### tagRequest

未用到吧，之前的axios封装，现在用request.js

##### useRequest

**Cancelable ** **Logable** **UseServise_1P**

#### package.json

##### echarts

`legendselectchanged` 切换图例选中状态后的事件

`resize()` 改变图表尺寸，在容器大小发生改变时需要手动调用

`dispose()`销毁实例



`datazoom`的`type`属性`inside`和`slider`有什么区别

​	inside可以对坐标系操作；slider有单独的滑动条

> [内置型数据区域缩放组件（dataZoomInside）](https://echarts.apache.org/zh/option.html#dataZoom-inside)：内置于坐标系中，使用户可以在坐标系上通过鼠标拖拽、鼠标滚轮、手指滑动（触屏上）来缩放或漫游坐标系。
>
> [滑动条型数据区域缩放组件（dataZoomSlider）](https://echarts.apache.org/zh/option.html#dataZoom-slider)：有单独的滑动条，用户在滑动条上进行缩放或漫游。
>
> [框选型数据区域缩放组件（dataZoomSelect）](https://echarts.apache.org/zh/option.html#toolbox.feature.dataZoom)：提供一个选框进行数据区域缩放。即 [toolbox.feature.dataZoom](https://echarts.apache.org/zh/option.html#toolbox.feature.dataZoom)，配置项在 `toolbox` 中。



##### @pacvue/element-ui

https://element-ui.pacvue.com/#/zh-CN/component/table

##### driver.js

在文件夹AppModule中定义

https://github.com/kamranahmedse/driver.js

##### vuedraggable.js 

vue拖拽组件

##### diff.js 

区分两个文本字符串

## 组件封装

#### TypeTable 对el-table的封装

用到了el-table-column自定义列模板，+了分页

```javascript
<!--
fetchFun // Function  { seachQuery , initPage = true , sortPageQuery }  获取 data的 函数 可选  自动处理分页与排序逻辑
	 this.fetchFun(null, false/true, this.query) // 仅在页码、筛选时自动调用fetchFun

属性query // Object { pageNum,pageSize,total,order orderBy } 集合分页与排序query 同步外部query参数

// fetchFun 在this.searchQuery变化时 参一为searchQuery
// fetchFun 在页码、每页数、排序筛选变化时 参一为null

-->
```





## 规范

#### git commit规范

fix: PCP-686 【asin detail page-reviews】列表加载中没有加载图标

feat

chore

refactor

doc等等

账号：zhanghuizhen

密码：25800852Pac

## 提问

axios的封装与使用，catch等捕获? await有try catch吗

-> 所有的promise, await都需要try-catch-finally

​	finally放loading=false等结束操作

charts使用

->每个chart都写自己的option，动态变化legend的selected，与自定义的图例联动

