#  接口

[[toc]]

## 定义

可描述类的一部分抽象行为，也可描述对象的结构形状

接口一般首字母大写，有的编程语言上面建议接口的名称加上I前缀

赋值的时候，变量的形状必须要跟接口的形状保持一致

接口中可定义可选属性、只读属性、任意属性

```typeScript
//定义接口
interface  Istate {
    name:string,
    age:nmber
}

var obj:Istate;
obj = {name:"张三",age:10}

//定义接口
interface LabelledValue {
    label:string
}

function printLabel(labelledObj:LabelledValue){
    console.log(labelledObj.label)
}
```

```typeScript
interface NameInfo {
    firstName: string,
    lastName: string,
}

const getFullName = ({ firstName, lastName }: NameInfo): string => {
    return `${firstName}${lastName}`
}

getFullName({
    firstName: 'hhhh',
    lastName: 'uuuu'
})
```



## 可选属性 ?

非必须属性

```typeScript
interface  Istate2 {
    name:string,
    age?:nmber //?代表可有可无
}

var obj2:Istate2;
obj2 = {name:"张三",age:10}
obj2 = {name:"李四",sex:"男"}


//属性个数不确定的时候 必须是any任意类型
interface  Istate3 {
    name:string|number,//此处可用联合类型
    age?:nmber,
    [propName:string]:any // 索引签名
}

var obj3:Istate3;
obj3 = {name:"李四",sex:"男",isMarry:true}
```

```typeScript
interface Square {
    color:string,
    area:number
}

interface SquareConfig {
    color?:string,
    width?:number
}

function createSquare(config:SquareConfig):Square{

    let newSquare = {color:'white',area:100};
    if(config.color){
       newSquare.color =   config.color;
    }
     if(config.width){
       newSquare.area =   config.width * config*width;
    }
    return newSquare;
}

let mySquare = createSquare({color:'black'})
```

## 只读属性 readonly
```typeScript
interface Istate4 {
    name:string,
    readonly age:number  //age变为了可读属性
}

var obj4:Istate4 = {name:"李四",age:12}
obj4.name="张三"

//age定义为可读属性，一旦赋予初始值之后，就不能再对其更改了
```
```typeScript
interface Point{
    readonly x:number,
    readonly y:number
}

let p1:Point = {x:10,y:20};

p1.x=3; //error
```

泛型只读数组类型ReadonlyArray
```typeScript
let a:number[]=[1,2,3,4];
let ro:ReadonlyArray<number> =a;
ro[0]=12;//error
```

变量用const
属性用readonly


## 额外属性检查

任意数量其他属性
```typeScript
interface SquareConfig{
    color?:string,
    width?:number,
    [propName:string]:any
}
```
##  绕开额外属性检查

- 方式1：类型断言 `as`
- 方式2：索引签名
- 方式3：类型兼容性

```typeScript
interface Vegetable {
    color?: string,// 可选属性？
    readonly type: string,// 只读属性
    [prop: string]: any // 索引签名
}

const getVegetables = ({ color, type }: Vegetable) => {
    return `A ${color ? (color + ' ') : ' '}${type}`
}

let vegetables = getVegetables({
    color: 'red',
    type: 'tomato',
    size: 2
} as Vegetable)// 类型断言
console.log('vegetables: ', vegetables);

// 类型兼容性
let veg = {
    color: 'red',
    type: 'tomato',
    size: 2
}
console.log('11', getVegetables(veg));
```



## 对class类的约束

```typeScript
//接口定义
//定义接口的时候，只定义声明即可，不包含具体内容
interface Iprinter{
    Printing(msg:string):string;
}

interface Imessage{
    getmsg():string;
}

//实现接口/实现多个接口，要实现里面的内容
class colorprinter implements Iprinter,Imessage{
    Printing(msg:string):string{
        return "打印"+msg+"成功"
    }
    getmsg():string{
        return "惠普hp"
    }
}

//将类实例化
let p1 = new colorprinter();
let val = p1.Printing("简历");
console.log(p1.getmsg())
```


## 对函数的约束

```typeScript
interface Imyfunction{
    (a:string,b:number):boolean;
}

let fun1:Imyfunction;
fun1 = function(a:string,b:number):boolean{
    return fasle
}


interface SearchFunc{
    (source:string,subString:string):boolean
}
let mySearch:SearchFunc
mySearch = function(src:string,sub:string):boolean{
    let result = src.search(sub)
    return result >-1
}
```

```typeScript
// interface AddFunc {
//     (num1: number, num2: number): number
// }

// 类型别名的形式
type AddFunc = (num1: number, num2: number) => number

const add: AddFunc = (n1, n2) => n1 + n2
```

## 对数组的约束

```typeScript
interface Istuarr{
    [index:number]:string;
}

let arr1:Istuarr;
arr1=["aaa","bbb"]
```

## 对json的约束

```typeScript
interface Idata{
    name:string,
    readonly age:number, //只读属性
    email?:string  //可选属性
}

function showdata(n:Idata){
    console.log(JSON.stringify(n))
}
showdata({name:"嘻哈",age:18})
```

## 接口的继承

和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里,提高可复用性。

```typeScript
interface Priter{
    getmsg();
}

interface ColorPriter extends Printer{
    printing();
}

class HPPrinter implements ColorPrinter{
    printing(){
        console.log('打印成功')
    }
    getmsg(){
        console.log('HP10022')
    }
}

var hp = new HPPrinter();
hp.getmsg();
hp.printing();
```

```typeScript
interface Vegetables {
    color: string
}

// interface Tomato {
//     color: string,
//     radius: number
// }
interface Tomato extends Vegetables {
    radius: number
}

// interface carrot {
//     color: string,
//     length: number
// }
interface Carrot extends Vegetables {
    length: number
}

// 定义对象
const tomato: Tomato = {
    radius: 1,
    color: 'red'
}

const carrot: Carrot = {
    length: 2,
    color: 'orange'
}

```


一个接口可以继承多个接口，创建出多个接口的合成接口。

```typeScript
interface Shape {
  color: string
}

interface PenStroke {
  penWidth: number
}
//可以继承多个接口
interface Square extends Shape, PenStroke {
  sideLength: number
}

let square = {} as Square; // //类型断言为Square
square.color = 'blue'
square.sideLength = 10
square.penWidth = 5.0
```

## 可索引类型

与使用接口描述函数类型差不多，我们也可以描述那些能够“通过索引得到”的类型，比如 `a[10]` 或 `ageMap['daniel']`。 可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。 让我们看一个例子：

```typeScript
interface StringArray{
    [index:number]:string
}

let myArray:StringArray
myArray = ['Bob','Fred']
let myStr:string = myArray[0];//数字索引
```

上面例子里，我们定义了 `StringArray` 接口，它具有索引签名。 这个索引签名表示了当用 `number` 去索引 `StringArray` 时会得到 `string` 类型的返回值。


```typeScript
// interface RoleDic {
//     [id: number]: string
// }

// const rolel: RoleDic = {
//     0: 'super_admin'
// }

interface RoleDic {
    [id: string]: string
}

// 当限定属性名为字符串的时候,js中会会自动把数字转为字符串
const rolel: RoleDic = {
    a: 'super_admin',
    1: 'admin'
}
```


typeScript 支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。 这是因为当使用 number 来索引时，JavaScript 会将它转换成string 然后再去索引对象。 也就是说用 100（一个 number）去索引等同于使用'100'（一个 string ）去索引，因此两者需要保持一致。

> 数字索引和字符串索引可以同时使用的，当同时使用时数字索引返回值必须是字符串索引返回值的子类型，因为此时会将数字转为字符串array['0']

```typeScript
class Animal{
    name:string
}

class Dog extends Animal{
    breed:string
}

interface NotOkay {
    [x:number]:Dog
    [x:string]:Animal
}

// 错误：使用数值型的字符串索引，有时会得到完全不同的Animal!
interface NotOkay {
  [x: number]: Animal
  [x: string]: Dog
}
```
>当索引类型和非索引类型同时存在时，非索引类型必须和索引类型一致

```typeScript
interface NumberDictionary{
    [index:string]:number
    length:number
    name:string  //error
}
```

> 索引类型可以设置为只读

```typeScript
interface ReadonlyStringArray{
    readonly [index:number]:string
}

let myArray:ReadonlyStringArray =['ww','ee']
myArray[0]="uuu"; //error
```


## 混合类型接口(ts3.1才支持混合类型接口)

```typeScript
// js计数器(方式1需要全局定义变量，有时变量可能会污染，所以该方式不是最好)
// let count = 0;
// const countUp = () => count++
// countUp();

// js闭包（方式2）
// const countUp = (() => {
//     let count = 0;
//     return () => {
//         count++
//     }
// })()
// countUp();

// js直接给函数添加属性（方式3）
// let countUp = () => {
//     countUp.count++
// }
// countUp.count = 0;
// countUp();

//
interface Counter {
    (): void,// 定义函数，无返回值
    count: number
}

const getCounter = (): Counter => {
    const c = () => { c.count++ }
    c.count = 0;
    return c;
}

const counter: Counter = getCounter();
counter();
console.log(counter.count);//1
```



## 类实现接口（类类型）

强制一个类去符合某种契约

实现（implements）是面向对象中的一个重要概念。一般来讲，一个类只能继承自另一个类，有时候不同类之间可以有一些共有的特性，这时候就可以把特性提取成接口（interfaces），用 `implements` 关键字来实现。这个特性大大提高了面向对象的灵活性。

接口描述了类的公共部分，而不是公共和私有两部分。
```typeScript
interface ClockInterface {
  currentTime: Date
}

class Clock implements ClockInterface {
  currentTime: Date
  constructor(h: number, m: number) { }
}
```
你也可以在接口中描述一个方法，在类里实现它，如同下面的 setTime 方法一样：
```typeScript
interface ClockInterface {
  currentTime: Date
  setTime(d: Date)
}

class Clock implements ClockInterface {
  currentTime: Date
  setTime(d: Date) {
    this.currentTime = d
  }
  constructor(h: number, m: number) { }
}
```

举例来说，门是一个类，防盗门是门的子类。如果防盗门有一个报警器的功能，我们可以简单的给防盗门添加一个报警方法。这时候如果有另一个类，车，也有报警器的功能，就可以考虑把报警器提取出来，作为一个接口，防盗门和车都去实现它：
```typeScript
interface Alarm {
    alert(): void;
}
​
class Door {
}
​
class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert');
    }
}
​
class Car implements Alarm {
    alert() {
        console.log('Car alert');
    }
}
```

一个类可以实现多个接口：

Car 实现了 Alarm 和 Light 接口，既能报警，也能开关车灯
```typeScript
interface Alarm {
    alert(): void;
}
​
interface Light {
    lightOn(): void;
    lightOff(): void;
}
​
class Car implements Alarm, Light {
    alert() {
        console.log('Car alert');
    }
    lightOn() {
        console.log('Car light on');
    }
    lightOff() {
        console.log('Car light off');
    }
}
```

## 实例接口和构造器接口

```typeScript
//实例接口
interface ClockInterface{
    tick()
}

//构造器接口
interface ClockConstructor{
    new(hour:number,minute:number):ClockInterface
}

//创建方法
function createClock(ctor:ClockConstructor,hour:number,minute:number):ClockInterface{
    return new ctor(hour,minute)
}

//定义类去实现接口
class DigitalClock implements ClockInterface{
    constructor(h:number,m:number){
        
    }
    tick(){
        console.log('beep beep')
    }
}

class AnalogClock implements ClockInterface{
    constructor(h:number,m:number){
        
    }
    tick(){
        console.log('tick beep')
    }
}

//
let digital = createClock(DigitalClock,hour:12,minute:17)

let analog = createClock(AnalogClock,hour:7,minute:32)
```
## 混合类型

```typeScript
interface Counter {
    (start:number):string,
    interval:number
    reset():void
}

//定义函数
function getCounter():Counter{
    let counter = (function (start:number){
        
    }) as Counter
    
    counter.interval = 123
    counter.reset = function (){
        
    }
    return counter;
}


let c = getCounter()
c(10)//函数类型
c.reset(); //对象类型
c.interval = 5.0; //属性类型
```

## 接口继承类

当接口继承了一个类类型时，它会继承类的成员但不包括其实现。 就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。 接口同样会继承到类的 private 和 protected 成员。 这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现（implement）。

当你有一个庞大的继承结构时这很有用，但要指出的是你的代码只在子类拥有特定属性时起作用。 这个子类除了继承至基类外与基类没有任何关系。例：
```typeScript
class Control {
  private state: any
}

interface SelectableControl extends Control {
  select(): void
}

class Button extends Control implements SelectableControl {
  select() { }
}

class TextBox extends Control {
  select() { }
}

// Error：“ImageC”类型缺少“state”属性。
class ImageC implements SelectableControl {
  select() { }
}
```

在上面的例子里，`SelectableControl` 包含了 `Control` 的所有成员，包括私有成员 `state`。 因为 `state` 是私有成员，所以只能够是 `Control` 的子类们才能实现 `SelectableControl` 接口。 因为只有 `Control` 的子类才能够拥有一个声明于`Control` 的私有成员 `state`，这对私有成员的兼容性是必需的。

在 `Control` 类内部，是允许通过 `SelectableControl` 的实例来访问私有成员 state 的。 实际上，`SelectableControl` 接口和拥有 `select` 方法的 `Control` 类是一样的。`Button`和 `TextBox` 类是 `SelectableControl` 的子类（因为它们都继承自`Control` 并有 `select` 方法），但 `ImageC` 类并不是这样的。