>Typed JavaScript at Any Scale.
>添加了类型系统的 JavaScript，适用于任何规模的项目。

根据类型检查的时机来分类：动态类型，静态类型

动态类型：运行时才会进行类型检查->导致运行时错误。JavaScript是解释型语言





Typed JavaScript at Any Scale.

添加了类型系统的 JavaScript，适用于任何规模的项目。

静态类型、弱类型

数据类型：原始数据类型，对象类型
原始数据类型：布尔值、数值、字符串、null、undefined、Symbol
非原始数据类型：Object
:string :number :boolean 
:undefined :null 是所有类型子类型
数组：
:元素类型[] :number[]
:Array<元素类型> 数组泛型 Array<number>
任意值
:any，未声明也是任意值
空值：
:void 与any类型相反 无类型 函数无返回值，变量只能undefined或null
:never 用不存在的值类型
:object
联合类型：
 :String|number
接口：用接口（Interfaces）来定义对象的类型
可选属性、任意属性、只读属性
interface Person {
  readonly id: number; // 只读属性
  name: string;
  age?: number; // 可选属性
  [propName: string]: any; // 任意属性
}
let tom: Person = {
  id: 89757,
  name: 'Tom',
  gender: 'male'
};
只读的约束：第一次给对象赋值时，不是第一次给只读属性赋值时

函数：输入输出都要约束
可选参数? 
默认参数 (first:string="test" )
剩余参数 (…items:any[])
类型断言：
尖括号语法

let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;


as语法 （JSX只能使用这个


let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;


类型推论：赋值。不赋值any不被类型检查
let {a, b}: {a: string, b: number} = o; // o={a:1,b:1}
联合类型 :string | number，可访问共同属性。赋值后类型推论可访问对应属性。

声明文件：

​	declare var 声明全局变量

​	declare function 声明全局方法

​	declare class 声明全局类

​	declare enum 声明全局枚举类型

​	declare namespace 声明（含有子属性的）全局对象

​	interface 和 type 声明全局类型

内置对象：

​	Boolean Error Date RegExp

​	Document HTMLElement Event NodeList
