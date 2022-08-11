# CSS

## 选择器

紧邻同胞元素

后续同胞

### 伪类选择符

伪类：它为所依附的元素设定某种幽灵类

#### 结构伪类

指代文档中的标记结构

选择根元素 :root

​	css变量，自定义属性(--*)，使用var()

选择空元素 :empty

选择唯一的子代 :only-child  :only-of-type

选择第一个和最后一个子代 :first-child :last-child

选择第一个和最后一个某种元素 :first-of-type :last-of-type

选择每第n个子元素 :nth-child(n) :nth-last-child()

​	n从0开始，html从1开始。odd even

选择每第n个某种元素:nth-of-type() nth-last--of-type()

#### 动态伪类

超链接伪类 :link :visited

用户操作伪类 :focus :hover :active

​	推荐顺序：link-visited-hover-active --> link-visited-focus-hover-active 

#### UI状态伪类 

:enabled :disabled :checked :indeterminate :default :valid :invalid :in-range :out-of-range :required :optional :read-write :read-only

启用和禁用的UI元素:enabled :disabled

选择状态:checked :indeterminate 

```css
input[type="checkbox"]:not(:checked)
```

默认选项伪类 :default

可选性伪类:required :optional

有效性伪类 :valid :invalid

范围伪类:in-range :out-of-range

可变性伪类:read-write :read-only

#### :target伪类

URL的片段标识符(哈希值)，所指向的文档片段(CSS中)称为目标(target)

#### :lang伪类

#### 否定伪类

:not()

### 伪元素选择符

装饰首字母::first-letter

装饰首行::first-line

装饰/创建前置和后置内容元素 ::before ::after(15章详细说明)

