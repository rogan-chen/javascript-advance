---
title: TypeScript小结
date: 2020-08-02
sidebar: 'auto'
categories:
 - JavaScript
tags:
 - TypeScript
publish: true
---

## 1. TypeScript是什么

- Typescript是由微软开发的一款**开源**的编程语言
- Typescript是**Javascript的超集**，遵循最新的ES5/ES6规范。
- TypeScript扩展了Javascript语法
- TypeScript更像后端Java、C#这样的面向对象语言可以让JS开发大型企业应用
- 越来越多的项目是基于TS的，比如VSCode、Angular6、Vue3、React16
- TS提供的类型系统可以帮助我们在写代码的时候提供更丰富的语法提示
- 在创建前的编译阶段经过**类型系统的检查**，就可以避免很多线上的错误

![TypeScript&&JavaScript](/javascript/1.typescript.jpg)

## 2. TypeScript安装和编译

#### 2.1 安装

```shell
# 全局安装
cnpm i typescript -g

# 将ts文件编译成js文件
tsc helloworld.ts
```

#### 2.2 vscode

```shell
# 生成配置文件
tsc --init

# 执行编译
tsc 文件名.ts
```

## 3. 数据类型

#### 3.1 布尔类型(boolean)

```typescript
let married: boolean = false;
```

#### 3.2 数字类型(number)

```typescript
let age: number = 10;
```

#### 3.3 字符串类型(string)

```typescript
let name: string = 'rogan';
```

#### 3.4 数组类型(array)

```typescript
let ages: number[] = [13, 25, 36];
let names: Array<string> = ['rogan', 'lily', 'allen'];
```

#### 3.5 元组类型(tuple)

在TypeScript的基础类型中，元组(Tuple)表示一个已知**数量**和**类型**的数组

```typescript
let person: [string, number] = ['rogan', 18];
```

| 元组                     | 数组                     |
| :----------------------- | :----------------------- |
| 每项可以是不同类型的数据 | 每项都是通向类型的数据   |
| 有预定的长度             | 没有预定长度             |
| 用于表示固定的结构       | 用于表示长度不固定的列表 |

#### 3.6 枚举类型(enum)

- 事先考虑某个变量的所有可能值，尽量用自然语言中的单词表示它的每一个值
- 比如性别、月份、星期、颜色、单位、学历

###### 3.6.1 普通枚举

```typescript
// 字符串枚举
enum Gender{
    GIRL='girl',
    BOY='boy'
}
console.log(`李雷是${Gender.BOY}`);
console.log(`韩梅梅是${Gender.GIRL}`);

// 数字枚举
enum Week{
    MONDAY=1,
    TUESDAY=2
}
console.log(`今天是星期${Week.MONDAY}`);

// 混合枚举
enum Person{
    name='Rogan',
    age=18,
}
console.log(`我的名字叫${Person.name}，我今年${Person.age}岁了`)
```

###### 3.6.2 常数枚举

- 常数枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员。
- 假如包含了计算成员，则会在编译阶段报错

```typescript
const enum Colors {
    Red,
    Yellow,
    Blue
}

let myColors = [Colors.Red, Colors.Yellow, Colors.Blue];
```

#### 3.7 任意类型(any)

- `any`就是可以赋值给任意类型
- 第三方库没有提供类型文件时可以使用`any`
- 类型转换遇到困难时
- 数据结构太复杂难以定义

```typescript
let root:any=document.getElementById('root');
root.style.color='red';
```

#### 3.8 null和undefined

- `null`和`undefined`是其它类型的子类型，可以赋值给其它类型，如数字类型，此时，赋值后的类型会变成`null`或`undefined`
- `strictNullChecks`参数用于新的严格空检查模式,在严格空检查模式下， `null`和`undefined`值都不属于任何一个类型，它们只能赋值给自己这种类型或者`any`

```typescript
// 非严格模式下
let x: number;
x = 1;
x = undefined;    
x = null;  
```

#### 3.9 void类型

- `void`表示没有任何类型
- 当一个函数没有返回值时，TS会认为它的返回值是`void`类型(JavaScript默认返回类型是`undefined`)。

#### 3.10 never类型

- never是其它类型(null undefined)的子类型，代表不会返回值
- void可以被赋值为null和undefined的类型。never则是一个不包含值的类型
- 拥有void返回值类型的函数能正常运行。拥有never返回值类型的函数无法正常返回，无法终止，或会抛出异常

#### 3.11 类型推论

- 是指编程语言中能够自动推导出值的类型的能力，它是一些强静态类型语言中出现的特性
- 定义时未赋值就会推论成any类型
- 如果定义的时候就赋值就能利用到类型推论

#### 3.12 包装对象(Wrapper Object)

- JavaScript的类型分为两种：原始数据类型（Primitive data types）和对象类型（Object types）。
- 所有的原始数据类型都没有属性（property）
- 原始数据类型
    - boolean
    - number
    - string
    - null
    - undefined
    - Symbol

```typescript
let name = 'zhufeng';
console.log(name.toUpperCase());
console.log((new String('zhufeng')).toUpperCase());
```

- 当调用基本数据类型方法的时候，JavaScript 会在原始数据类型和对象类型之间做一个强制性切换

```typescript
let isOK: boolean = true; // 编译通过
let isOK: boolean = Boolean(1) // 编译通过
let isOK: boolean = new Boolean(1); // 编译失败
```

#### 3.13 联合类型

- 联合类型（Union Types）表示取值可以为多种类型中的一种
- 未赋值时联合类型上只能访问两个类型共有的属性和方法

```typescript
let name: string | number;
console.log(name.toString());
name = 3;
console.log(name.toFixed(2));
name = 'zhufeng';
console.log(name.length);
```

#### 3.14 类型断言

- 类型断言可以将一个联合类型的变量，指定为一个更加具体的类型
- 不能将联合类型断言为不存在的类型

```typescript
let name: string | number;
console.log((name as string).length);
console.log((name as number).toFixed(2));
console.log((name as boolean));
```

#### 3.15 字面量类型

- 可以把字符串、数字、布尔值字面量组成一个联合类型

```typescript
type ZType = 1 | 'One' | true;
let t1:ZType = 1;
let t2:ZType = 'One';
let t3:ZType = true;
```

#### 3.16 字符串字面量 vs 联合类型

- 字符串字面量类型用来约束取值只能是某几个字符串中的一个, 联合类型(Union Types)表示取值可以为多种类型中的一种
- 字符串字面量限定了使用该字面量的地方仅接受特定的值,联合类型对于值并没有限定，仅仅限定值的类型需要保持一致

## 4. 函数

#### 4.1 函数的定义

- 可以指定参数的类型和返回值的类型

```typescript
function hello(name:string):void {
    console.log('hello',name);
}
hello('zfpx');
```

#### 4.2 函数表达式

```typescript
type GetUsernameFunction = (x:string,y:string)=>string;
let getUsername:GetUsernameFunction = function(firstName,lastName){
  return firstName + lastName;
}
```

#### 4.3 没有返回值

```typescript
let hello2 = function (name:string):void {
    console.log('hello2',name);
    return undefined;
}
hello2('zfpx');
```

#### 4.4 可选参数

```typescript
function print(name:string,age?:number):void {
    console.log(name,age);
}
print('zfpx');
```

#### 4.5 默认参数

```typescript
function ajax(url:string,method:string='GET') {
    console.log(url,method);
}
ajax('/users');
```

#### 4.6 剩余参数

```typescript
function sum(...numbers:number[]) {
    return numbers.reduce((val,item)=>val+=item,0);
}
console.log(sum(1,2,3));
```

#### 4.7 函数重载

- 在Java中的重载，指的是两个或者两个以上的同名函数，参数不一样
- 在TypeScript中，表现为给同一个函数提供多个函数类型定义

```typescript
function attr(val: string): void;
function attr(val: number): void;
function attr(val:any):void {
    if (typeof val === 'string') {
        obj.name=val;
    } else {
        obj.age=val;
    }
}
attr('zfpx');
attr(9);
attr(true);
console.log(obj);
```

## 5. 类

#### 5.1 如何定义类

```typescript
class Person{
    name:string;
    getName():void{
        console.log(this.name);
    }
}
let p1 = new Person();
p1.name = 'zhufeng';
p1.getName();
```

#### 5.2 存取器

- 在TypeScript中，我们可以通过存取器来改变一个类中属性的读取和赋值行为
- 构造函数
    - 主要用于初始化类的成员变量属性
    - 类的对象创建时自动调用执行
    - 没有返回值

```typescript
class User {
    myname:string;
    constructor(myname: string) {
        this.myname = myname;
    }
    get name() {
        return this.myname;
    }
    set name(value) {
        this.myname = value;
    }
}

let user = new User('zhufeng');
user.name = 'jiagou'; 
console.log(user.name); 
```

#### 5.3 参数属性

```typescript
class User {
    // public myname: string;
    constructor(public myname: string) {}
    get name() {
        return this.myname;
    }
    set name(value) {
        this.myname = value;
    }
}

let user = new User('zhufeng');
console.log(user.name); 
user.name = 'jiagou'; 
console.log(user.name);
```

#### 5.4 readonly

- readonly修饰的变量只能在构造函数中初始化
- 在TypeScript中，const是常量标志符，其值不能被重新分配
- TypeScript的类型系统同样也允许将interface、type、 class上的属性标识为 readonly
- readonly实际上只是在编译阶段进行代码检查。而const则会在运行时检查（在支持const语法的 JavaScript 运行时环境中）

```typescript
class Animal {
    public readonly name: string
    constructor(name:string) {
        this.name = name;
    }
    changeName(name:string){
        this.name = name;
    }
}

let a = new Animal('zhufeng');
a.changeName('jiagou');
```

#### 5.5 继承

- 子类继承父类后子类的实例就拥有了父类中的属性和方法，可以增强代码的可复用性
- 将子类公用的方法抽象出来放在父类中，自己的特殊逻辑放在子类中重写父类的逻辑
- super可以调用父类上的方法和属性

```typescript
class Person {
    name: string;//定义实例的属性，默认省略public修饰符
    age: number;
    constructor(name:string,age:number) {//构造函数
        this.name=name;
        this.age=age;
    }
    getName():string {
        return this.name;
    }
    setName(name:string): void{
        this.name=name;
    }
}

class Student extends Person{
    no: number;
    constructor(name:string,age:number,no:number) {
        super(name,age);
        this.no=no;
    }
    getNo():number {
        return this.no;
    }
}

let s1=new Student('zfpx',10,1);
console.log(s1);
```

#### 5.6 类里面的修饰符

```typescript
class Father {
    public name: string;  //类里面 子类其它任何地方外边都可以访问
    protected age: number; //类里面 子类都可以访问,其它任何地方不能访问
    private money: number; //类里面可以访问，子类和其它任何地方都不可以访问
    constructor(name:string,age:number,money:number) {//构造函数
        this.name=name;
        this.age=age;
        this.money=money;
    }
}
```

#### 5.7 静态属性 && 静态方法

```typescript
class Father {
    static className='Father';
    static getClassName() {
        return Father.className;
    }
    public name: string;
    constructor(name:string) {//构造函数
        this.name=name;
    }

}
console.log(Father.className);
console.log(Father.getClassName());
```

#### 5.8 装饰器

- 装饰器是一种特殊类型的声明，它能够被附加到类声明、方法、属性或参数上，可以修改类的行为
- 常见的装饰器有类装饰器、属性装饰器、方法装饰器和参数装饰器
- 装饰器的写法分为普通装饰器和装饰器工厂

###### 5.8.1 类装饰器

- 类装饰器在类声明之前声明，用来监视、修改或替换类定义

```typescript
namespace a {
    interface Person {
        name: string;
        eat: any
    }
    function enhancer(target: any) {
        target.prototype.name = 'zhufeng';
        target.prototype.eat = function () {
            console.log('eat');
        }
    }
    @enhancer
    class Person {
        constructor() { }
    }
    let p: Person = new Person();
    console.log(p.name);
    p.eat();
}

namespace b {
    interface Person {
        name: string;
        eat: any
    }
    function enhancer(name: string) {
        return function enhancer(target: any) {
            target.prototype.name = name;
            target.prototype.eat = function () {
                console.log('eat');
            }
        }
    }

    @enhancer('zhufeng')
    class Person {
        constructor() { }
    }
    let p: Person = new Person();
    console.log(p.name);
    p.eat();
}

namespace c {
    interface Person {
        name: string;
        eat: any
    }
    function enhancer(target: any) {
        return class {
            name: string = 'jiagou'
            eat() {
                console.log('吃饭饭');
            }
        }
    }
    @enhancer
    class Person {
        constructor() { }
    }
    let p: Person = new Person();
    console.log(p.name);
    p.eat();
}
```

###### 5.8.2 属性装饰器

- 属性装饰器表达式会在运行时当作函数被调用，传入下列2个参数
- 属性装饰器用来装饰属性
    - 第一个参数对于静态成员来说是类的构造函数，对于实例成员是类的原型对象
    - 第二个参数是属性的名称
- 方法装饰器用来装饰方法
    - 第一个参数对于静态成员来说是类的构造函数，对于实例成员是类的原型对象
    - 第二个参数是方法的名称
    - 第三个参数是方法描述符

```typescript
namespace d {
    function upperCase(target: any, propertyKey: string) {
        let value = target[propertyKey];
        const getter = function () {
            return value;
        }
        // 用来替换的setter
        const setter = function (newVal: string) {
            value = newVal.toUpperCase()
        };
        // 替换属性，先删除原先的属性，再重新定义属性
        if (delete target[propertyKey]) {
            Object.defineProperty(target, propertyKey, {
                get: getter,
                set: setter,
                enumerable: true,
                configurable: true
            });
        }
    }
    function noEnumerable(target: any, property: string, descriptor: PropertyDescriptor) {
        console.log('target.getName', target.getName);
        console.log('target.getAge', target.getAge);
        descriptor.enumerable = true;
    }
    function toNumber(target: any, methodName: string, descriptor: PropertyDescriptor) {
        let oldMethod = descriptor.value;
        descriptor.value = function (...args: any[]) {
            args = args.map(item => parseFloat(item));
            return oldMethod.apply(this, args);
        }
    }
    class Person {
        @upperCase
        name: string = 'zhufeng'
        public static age: number = 10
        constructor() { }
        @noEnumerable
        getName() {
            console.log(this.name);
        }
        @toNumber
        sum(...args: any[]) {
            return args.reduce((accu: number, item: number) => accu + item, 0);
        }
    }
    let p: Person = new Person();
    for (let attr in p) {
        console.log('attr=', attr);
    }
    p.name = 'jiagou';
    p.getName();
    console.log(p.sum("1", "2", "3"));
}
```

###### 5.8.3 方法装饰器

```typescript
```

###### 5.8.4 参数装饰器

- 会在运行时当作函数被调用，可以使用参数装饰器为类的原型增加一些元数据
    - 第1个参数对于静态成员是类的构造函数，对于实例成员是类的原型对象
    - 第2个参数的名称
    - 第3个参数在函数列表中的索引

```typescript
namespace d {
    interface Person {
        age: number;
    }
    function addAge(target: any, methodName: string, paramsIndex: number) {
        console.log(target);
        console.log(methodName);
        console.log(paramsIndex);
        target.age = 10;
    }
    class Person {
        login(username: string, @addAge password: string) {
            console.log(this.age, username, password);
        }
    }
    let p = new Person();
    p.login('zhufeng', '123456')
}
```

###### 5.8.5 装饰器执行顺序

- 有多个参数装饰器时：从最后一个参数依次向前执行
- 方法和方法参数中参数装饰器先执行。
- 类装饰器总是最后执行
- 方法和属性装饰器，谁在前面谁先执行。因为参数属于方法一部分，所以参数会一直紧紧挨着方法执行

```typescript
namespace e {
    function Class1Decorator() {
        return function (target: any) {
            console.log("类1装饰器");
        }
    }
    function Class2Decorator() {
        return function (target: any) {
            console.log("类2装饰器");
        }
    }
    function MethodDecorator() {
        return function (target: any, methodName: string, descriptor: PropertyDescriptor) {
            console.log("方法装饰器");
        }
    }
    function Param1Decorator() {
        return function (target: any, methodName: string, paramIndex: number) {
            console.log("参数1装饰器");
        }
    }
    function Param2Decorator() {
        return function (target: any, methodName: string, paramIndex: number) {
            console.log("参数2装饰器");
        }
    }
    function PropertyDecorator(name: string) {
        return function (target: any, propertyName: string) {
            console.log(name + "属性装饰器");
        }
    }

    @Class1Decorator()
    @Class2Decorator()
    class Person {
        @PropertyDecorator('name')
        name: string = 'zhufeng';
        @PropertyDecorator('age')
        age: number = 10;
        @MethodDecorator()
        greet(@Param1Decorator() p1: string, @Param2Decorator() p2: string) { }
    }
}
/**
name属性装饰器
age属性装饰器
参数2装饰器
参数1装饰器
方法装饰器
类2装饰器
类1装饰器
 */
```

#### 5.9 抽象类

- 抽象描述一种抽象的概念，无法被实例化，只能被继承
- 无法创建抽象类的实例
- 抽象方法不能在抽象类中实现，只能在抽象类的具体子类中实现，而且必须实现

```typescript
abstract class Animal {
    name!:string;
    abstract speak():void;
}
class Cat extends Animal{
    speak(){
        console.log('喵喵喵');
    }
}
let animal = new Animal();//Cannot create an instance of an abstract class
animal.speak();
let cat = new Cat();
cat.speak();
```

#### 5.10 抽象类 vs 接口

- 不同类之间公有的属性或方法，可以抽象成一个接口（Interfaces）
- 而抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现
- 抽象类本质是一个无法被实例化的类，其中能够实现方法和初始化属性，而接口仅能够用于描述,既不提供方法的实现，也不为属性进行初始化
- 一个类可以继承一个类或抽象类，但可以实现（implements）多个接口
- 抽象类也可以实现接口

#### 5.11 抽象方法

- 抽象类和方法不包含具体实现，必须在子类中实现
- 抽象方法只能出现在抽象类中
- 子类可以对抽象类进行不同的实现

```typescript
abstract class Animal{
    abstract speak():void;
}
class Dog extends  Animal{
    speak(){
        console.log('小狗汪汪汪');
    }
}
class Cat extends  Animal{
    speak(){
        console.log('小猫喵喵喵');
    }
}
let dog=new Dog();
let cat=new Cat();
dog.speak();
cat.speak();
```

#### 5.12 重写（override）vs 重载（overload）

- 重写是指子类重写继承自父类中的方法
- 重载是指为同一个函数提供多个类型定义

#### 5.13 继承 vs 多态

- 继承(Inheritance)子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性
- 多态(Polymorphism)由继承而产生了相关的不同的类，对同一个方法可以有不同的行为

## 6. 接口

- 接口一方面可以在面向对象编程中表示为行为的抽象
- 接口就是把一些类中共有的属性和方法抽象出来,可以用来约束实现此接口的类
- 一个类可以继承另一个类并实现多个接口
- 接口像插件一样是用来增强类的，而抽象类是具体类的抽象概念
- 一个类可以实现多个接口，一个接口也可以被多个类实现，但一个类的可以有多个子类，但只能有一个父类

#### 6.1 接口

- interface中可以用分号或者逗号分割每一项，也可以什么都不加

###### 6.1.1 对象的抽象

```typescript
interface Speakable{
    speak():void;
    name?:string;//？表示可选属性
}

let speakman:Speakable = {
    speak(){},//少属性会报错
    name,
    age//多属性也会报错
}
```

###### 6.1.2 行为的抽象

```typescript
//接口可以在面向对象编程中表示为行为的抽象
interface Speakable{
    speak():void;
}
interface Eatable{
    eat():void
}
//一个类可以实现多个接口
class Person implements Speakable,Eatable{
    speak(){
        console.log('Person说话');
    }
    eat(){}
}
class TangDuck implements Speakable{
    speak(){
        console.log('TangDuck说话');
    }
    eat(){}
}
```

###### 6.1.3 任意属性

```typescript
//无法预先知道有哪些新的属性的时候,可以使用 `[propName:string]:any`,propName名字是任意的
interface Person {
  readonly id: number;
  name: string;
  [propName: string]: any;
}

let p1 = {
  id:1,
  name:'zhufeng',
  age:10
}
```

#### 6.2 接口的继承

- 一个接口可以继承自另外一个接口

```typescript
interface Speakable {
    speak(): void
}
interface SpeakChinese extends Speakable {
    speakChinese(): void
}
class Person implements SpeakChinese {
    speak() {
        console.log('Person')
    }
    speakChinese() {
        console.log('speakChinese')
    }
}
```

#### 6.3 readonly

```typescript
interface Person{
  readonly id:number;
  name:string
}
let tom:Person = {
  id :1,
  name:'zhufeng'
}
tom.id = 1; // 报错
```

#### 6.4 函数类型接口

- 对方法传入的参数和返回值进行约束

```typescript
interface discount{
  (price:number):number
}

let cost:discount = function(price:number):number{
   return price * .8;
}
```

#### 6.5 可索引接口

- 对数组和对象进行约束
- userInterface表示index的类型是number，那么值的类型必须是string
- UserInterface2表示：index的类型是string，那么值的类型必须是string

```typescript
interface UserInterface {
  [index:number]:string
}
let arr:UserInterface = ['zfpx1','zfpx2'];
console.log(arr);

interface UserInterface2 {
  [index:string]:string
}
let obj:UserInterface2 = {name:'zhufeng'};
```

#### 6.6 类接口

```typescript
interface Speakable {
    name: string;
    speak(words: string): void
}
class Dog implements Speakable {
    name!: string;
    speak(words:string) {
        console.log(words);
    }
}
let dog = new Dog();
dog.speak('汪汪汪');
```

## 7. 泛型

- 泛型(Generics)是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性
- 泛型T作用域只限于函数内部使用

#### 7.1 泛型函数

```typescript
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
      result[i] = value;
    }
    return result;
  }
let result = createArray2<string>(3,'x');
console.log(result);
```

#### 7.2 类数组

- 类数组（Array-like Object）不是数组类型，比如 arguments

#### 7.3 泛型类

```typescript
class MyArray<T>{
    private list:T[]=[];
    add(value:T) {
        this.list.push(value);
    }
    getMax():T {
        let result=this.list[0];
        for (let i=0;i<this.list.length;i++){
            if (this.list[i]>result) {
                result=this.list[i];
            }
        }
        return result;
    }
}
let arr=new MyArray();
arr.add(1); arr.add(2); arr.add(3);
let ret = arr.getMax();
console.log(ret);
```

#### 7.4 泛型接口

```typescript
interface Calculate{
  <T>(a:T,b:T):T
}
let add:Calculate = function<T>(a:T,b:T){
  return a;
}
add<number>(1,2);

```

#### 7.5 多个类型参数

```typescript
function swap<A,B>(tuple:[A,B]):[B,A]{
  return [tuple[1],tuple[0]];
}
let swapped = swap<string,number>(['a',1]);
console.log(swapped);
console.log(swapped[0].toFixed(2));
console.log(swapped[1].length);
```

#### 7.6 默认泛型类型

```typescript
function createArray3<T=number>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
let result2 = createArray3(3,'x');
console.log(result2);
```

#### 7.7 泛型约束

- 在函数中使用泛型的时候，由于预先并不知道泛型的类型，所以不能随意访问相应类型的属性或方法

```typescript
function logger<T>(val: T) {
    console.log(val.length); //直接访问会报错
}

//可以让泛型继承一个接口
interface LengthWise {
    length: number
}

//可以让泛型继承一个接口
function logger2<T extends LengthWise>(val: T) {
    console.log(val.length)
}
logger2('zhufeng');
logger2(1);
```

#### 7.8 泛型接口

```typescript
interface Cart<T>{
  list:T[]
}
let cart:Cart<{name:string,price:number}> = {
  list:[{name:'zhufeng',price:10}]
}
console.log(cart.list[0].name,cart.list[0].price);
```

#### 7.9 泛型类型别名

- 泛型类型别名可以表达更复杂的类型

```typescript
type Cart<T> = {list:T[]} | T[];
let c1:Cart<string> = {list:['1']};
let c2:Cart<number> = [1];
```

#### 7.10 泛型接口 vs 泛型类型别名

- 接口创建了一个新的名字，它可以在其他任意地方被调用。而类型别名并不创建新的名字，例如报错信息就不会使用别名
- 类型别名不能被 extends和 implements,这时我们应该尽量使用接口代替类型别名
- 当我们需要使用联合类型或者元组类型的时候，类型别名会更合适

## 8. 结构类型系统

#### 8.1 接口的兼容性

- 如果传入的变量和声明的类型不匹配，TS就会进行兼容性检查
- 原理是`Duck-Check`,就是说只要目标类型中声明的属性变量在源类型中都存在就是兼容的

```typescript
interface Animal {
    name: string;
    age: number;
}

interface Person {
    name: string;
    age: number;
    gender: number
}
// 要判断目标类型`Person`是否能够兼容输入的源类型`Animal`
function getName(animal: Animal): string {
    return animal.name;
}

let p = {
    name: 'zhufeng',
    age: 10,
    gender: 0
}

getName(p);
//只有在传参的时候两个变量之间才会进行兼容性的比较，赋值的时候并不会比较,会直接报错
let a: Animal = {
    name: 'zhufeng',
    age: 10,
    gender: 0
}
```

#### 8.2 基本类型的兼容性

```typescript
//基本数据类型也有兼容性判断
let num : string|number;
let str:string='zhufeng';
num = str;

//只要有toString()方法就可以赋给字符串变量
let num2 : {
  toString():string
}

let str2:string='jiagou';
num2 = str2;
```

#### 8.3 类的兼容性

- 在TS中是结构类型系统，只会对比结构而不在意类型

```typescript
class Animal{
    name:string
}
class Bird extends Animal{
   swing:number
}

let a:Animal;
a = new Bird();

let b:Bird;
//并不是父类兼容子类，子类不兼容父类
b = new Animal();
```

```typescript
class Animal{
  name:string
}
//如果父类和子类结构一样，也可以的
class Bird extends Animal{}

let a:Animal;
a = new Bird();

let b:Bird;
b = new Animal();
```

```typescript
//甚至没有关系的两个类的实例也是可以的
class Animal{
  name:string
}
class Bird{
  name:string
}
let a:Animal ;
a = new Bird();
let b:Bird;
b = new Animal();
```

#### 8.4 函数的兼容性

- 比较函数的时候是要先比较函数的参数，再比较函数的返回值

###### 8.4.1 比较参数

```typescript
type sumFunc = (a:number,b:number)=>number;
let sum:sumFunc;
function f1(a:number,b:number):number{
  return a+b;
}
sum = f1;

//可以省略一个参数
function f2(a:number):number{
   return a;
}
sum = f2;

//可以省略二个参数
function f3():number{
    return 0;
}
sum = f3;

 //多一个参数可不行
function f4(a:number,b:number,c:number){
    return a+b+c;
}
sum = f4;
```

###### 8.4.2 比较返回值

```typescript
type GetPerson = ()=>{name:string,age:number};
let getPerson:GetPerson;
//返回值一样可以
function g1(){
    return {name:'zhufeng',age:10};
}
getPerson = g1;
//返回值多一个属性也可以
function g2(){
    return {name:'zhufeng',age:10,gender:'male'};
}
getPerson = g2;
//返回值少一个属性可不行
function g3(){
    return {name:'zhufeng'};
}
getPerson = g3;
//因为有可能要调用返回值上的方法
getPerson().age.toFixed();
```

#### 8.5 函数参数的协变

- 当比较函数参数类型时，只有当源函数参数能够赋值给目标函数或者反过来时才能赋值成功

#### 8.6 泛型的兼容性

- 泛型在判断兼容性的时候会先判断具体的类型,然后再进行兼容性判断

#### 8.7 枚举的兼容性

- 枚举类型与数字类型兼容，并且数字类型与枚举类型兼容
- 不同枚举类型之间是不兼容的

```typescript
//数字可以赋给枚举
enum Colors {Red,Yellow}
let c:Colors;
c = Colors.Red;
c = 1;
c = '1';

//枚举值可以赋给数字
let n:number;
n = 1;
n = Colors.Red;
```

## 9. 类型保护

- 类型保护就是一些表达式，他们在编译的时候就能通过类型信息确保某个作用域内变量的类型
- 类型保护就是能够通过关键字判断出分支中的类型

#### 9.1 typeof类型保护

```typescript
function double(input: string | number | boolean) {
    if (typeof input === 'string') {
        return input + input;
    } else {
        if (typeof input === 'number') {
            return input * 2;
        } else {
            return !input;
        }
    }
}
```

#### 9.2 instanceof类型保护

```typescript
class Animal {
    name!: string;
}
class Bird extends Animal {
    swing!: number
}
function getName(animal: Animal) {
    if (animal instanceof Bird) {
        console.log(animal.swing);
    } else {
        console.log(animal.name);
    }
}
```

#### 9.3 null类型保护

- 如果开启了`strictNullChecks`选项，那么对于可能为`null`的变量不能调用它上面的方法和属性

```typescript
function getFirstLetter(s: string | null) {
    //第一种方式是加上null判断
    if (s == null) {
        return '';
    }
    //第二种处理是增加一个或的处理
    s = s || '';
    return s.charAt(0);
}

//它并不能处理一些复杂的判断，需要加非空断言操作符
function getFirstLetter2(s: string | null) {
    function log() {
        console.log(s!.trim());
    }
    s = s || '';
    log();
    return s.charAt(0);
}
```

#### 9.4 链判断运算符

- 链判断运算符是一种先检查属性是否存在，再尝试访问该属性的运算符，其符号为 ?.
- 如果运算符左侧的操作数 ?. 计算为 undefined 或 null，则表达式求值为 undefined 。否则，正常触发目标属性访问，方法或函数调用。

```typescript
a?.b; //如果a是null/undefined,那么返回undefined，否则返回a.b的值.
a == null ? undefined : a.b;

a?.[x]; //如果a是null/undefined,那么返回undefined，否则返回a[x]的值
a == null ? undefined : a[x];

a?.b(); // 如果a是null/undefined,那么返回undefined
a == null ? undefined : a.b(); //如果a.b不函数的话抛类型错误异常,否则计算a.b()的结果

a?.(); //如果a是null/undefined,那么返回undefined
a == null ? undefined : a(); //如果A不是函数会抛出类型错误
//否则 调用a这个函数
```

> 链判断运算符 还处于 stage1 阶段,TS 也暂时不支持

#### 9.5 可辨识的联合类型

- 就是利用联合类型中的共有字段进行类型保护的一种技巧
- 相同字段的不同取值就是可辨识

```typescript
interface WarningButton{
  class:'warning',
  text1:'修改'
}
interface DangerButton{
  class:'danger',
  text2:'删除'
}
type Button = WarningButton|DangerButton;
function getButton(button:Button){
 if(button.class=='warning'){
  console.log(button.text1);
 }
 if(button.class=='danger'){
  console.log(button.text2);
 }
}
```

#### 9.6 in操作符

- `in`运算符可以被用于参数类型的判断

```typescript
interface Bird {
    swing: number;
}

interface Dog {
    leg: number;
}

function getNumber(x: Bird | Dog) {
    if ("swing" in x) {
      return x.swing;
    }
    return x.leg;
}
```

#### 9.7 自定义的类型保护

- `TypeScript`里的类型保护本质上就是一些表达式，它们会在运行时检查类型信息，以确保在某个作用域里的类型是符合预期的
- `type is Type1Class`就是类型谓词
- 谓词为`parameterName is Type`这种形式,`parameterName`必须是来自于当前函数签名里的一个参数名
- 每当使用一些变量调用`isType1`时，如果原始类型兼容，`TypeScript`会将该变量缩小到该特定类型

```typescript
function isType1(type: Type1Class | Type2Class): type is Type1Class {
    return (<Type1Class>type).func1 !== undefined;
}
```

```typescript
interface Bird {
  swing: number;
}

interface Dog {
  leg: number;
}

//没有相同字段可以定义一个类型保护函数
function isBird(x:Bird|Dog): x is Bird{
  return (<Bird>x).swing == 2;
  //return (x as Bird).swing == 2;
}

function getAnimal(x: Bird | Dog) {
  if (isBird(x)) {
    return x.swing;
  }
  return x.leg;
}
```

#### 9.8 unknown（不常用）

- TypeScript 3.0 引入了新的unknown 类型，它是 any 类型对应的安全类型
- unknown 和 any 的主要区别是 unknown 类型会更加严格：在对 unknown 类型的值执行大多数操作之前，我们必须进行某种形式的检查。而在对 any 类型的值执行操作之前，我们不必进行任何检查

###### 9.8.1 any类型

- 在 TypeScript 中，任何类型都可以被归为 any 类型。这让 any 类型成为了类型系统的 顶级类型 (也被称作 全局超级类型)。
- TypeScript允许我们对 any 类型的值执行任何操作，而无需事先执行任何形式的检查

```typescript
let value: any;

value = true;             // OK
value = 42;               // OK
value = "Hello World";    // OK
value = [];               // OK
value = {};               // OK
value = Math.random;      // OK
value = null;             // OK
value = undefined;        // OK
```

###### 9.8.2 unknown类型

- 就像所有类型都可以被归为 any，所有类型也都可以被归为 unknown。这使得 unknown 成为 TypeScript 类型系统的另一种顶级类型（另一种是 any）
- 任何类型都可以赋值给unknown类型

```typescript
let value: unknown;

let value1: unknown = value;   // OK
let value2: any = value;       // OK
let value3: boolean = value;   // Error
let value4: number = value;    // Error
let value5: string = value;    // Error
let value6: object = value;    // Error
let value7: any[] = value;     // Error
let value8: Function = value;  // Error
```

###### 9.8.3 缩小unknown类型范围

- 如果没有类型断言或类型细化时，不能在unknown上面进行任何操作
- typeof
- instanceof
- 自定义类型保护函数
- 可以对 unknown 类型使用类型断言

```typescript
const value: unknown = "Hello World";
const someString: string = value as string;
```

###### 9.8.4 联合类型中的unknown类型

- 在联合类型中，unknown 类型会吸收任何类型。这就意味着如果任一组成类型是 unknown，联合类型也会相当于 unknown

```typescript
type UnionType1 = unknown | null;       // unknown
type UnionType2 = unknown | undefined;  // unknown
type UnionType3 = unknown | string;     // unknown
type UnionType4 = unknown | number[];   // unknown
```

###### 9.8.5 交叉类型中的unknown类型

- 在交叉类型中，任何类型都可以吸收 unknown 类型。这意味着将任何类型与 unknown 相交不会改变结果类型

```typescript
type IntersectionType1 = unknown & null;       // null
type IntersectionType2 = unknown & undefined;  // undefined
type IntersectionType3 = unknown & string;     // string
type IntersectionType4 = unknown & number[];   // number[]
type IntersectionType5 = unknown & any;        // any
```

###### 9.8.6 never是unknown的子类型

```typescript
type isNever = never extends unknown ? true : false;
```

###### 9.8.7 keyof unknown等于never

```typescript
type key = keyof unknown;
```

###### 9.8.8 只能对unknown进行等或不等的操作，不能执行其他操作

```typescript
un1===un2;
un1!==un2;
un1 += un2;
```

###### 9.8.9 不能做的操作

- 不能访问属性
- 不能作为函数调用
- 不能当作类的构造函数不能创建实例

###### 9.8.10 映射属性

- 如果映射类型遍历的时候是unknown,不会映射属性

```typescript
type getType<T> = {
  [P in keyof T]:number
}
type t = getType<unknown>;
```

## 10. 类型变换

#### 10.1 交叉类型

- 交叉类型（Intersection Types）表示将多个类型合并为一个类型

```typescript
interface Bird {
    name: string,
    fly(): void
}

interface Person {
    name: string,
    talk(): void
}

type BirdPerson = Bird & Person;
let p: BirdPerson = { name: 'zhufeng', fly() { }, talk() { } };
p.fly;
p.name
p.talk;
```

#### 10.2 typeof

- 可以获取一个变量的类型

```typescript
//先定义变量，再定义类型
let p1 = {
    name:'zhufeng',
    age:10,
    gender:'male'
}

type People = typeof p1;

function getName(p:People):string{
    return p.name;
}

getName(p1);
```

#### 10.3 索引访问操作符

- 可以通过[]获取一个类型的子类型

```typescript
interface Person{
    name:string;
    age:number;
    job:{
        name:string
    };
    interests:{name:string,level:number}[]
}
let FrontEndJob:Person['job'] = {
    name:'前端工程师'
}
let interestLevel:Person['interests'][0]['level'] = 2;
```

#### 10.4 keyof

- 索引类型查询操作符

```typescript
interface Person{
  name:string;
  age:number;
  gender:'male'|'female';
}
//type PersonKey = 'name'|'age'|'gender';
type PersonKey = keyof Person;

function getValueByKey(p:Person,key:PersonKey){
  return p[key];
}
let val = getValueByKey({name:'zhufeng',age:10,gender:'male'},'name');
console.log(val);
```

#### 10.5 映射类型

- 在定义的时候用in操作符去批量定义类型中的属性

```typescript
interface Person{
  name:string;
  age:number;
  gender:'male'|'female';
}
//批量把一个接口中的属性都变成可选的
type PartPerson = {
  [Key in keyof Person]?:Person[Key]
}

let p1:PartPerson={};
//也可以使用泛型
type Part<T> = {
  [key in keyof T]?:T[key]
}
let p2:Part<Person>={};
```

#### 10.6 内置工具类型

###### 10.6.1 Partial

- 将传入的属性由非可选变为可选

```typescript
type Partial<T> = { [P in keyof T]?: T[P] };

interface A {
  a1: string;
  a2: number;
  a3: boolean;
}

type aPartial = Partial<A>;

const a: aPartial = {}; // 不会报错
```

###### 10.6.2 Required

- 将传入的属性中的可选项变为必选项，这里用了 -? 修饰符来实现

```typescript
//type Required<T> = { [P in keyof T]-?: T[P] };

interface Person{
  name:string;
  age:number;
  gender?:'male'|'female';
}
/**
 * type Require<T> = { [P in keyof T]-?: T[P] };
 */
let p:Required<Person> = {
  name:'zhufeng',
  age:10,
  //gender:'male'
}
```

###### 10.6.3 Readonly

- 通过为传入的属性每一项都加上 readonly 修饰符来实现只读功能

```typescript
interface Person{
  name:string;
  age:number;
  gender?:'male'|'female';
}
//type Readonly<T> = { readonly [P in keyof T]: T[P] };
let p:Readonly<Person> = {
  name:'zhufeng',
  age:10,
  gender:'male'
}
p.age = 11;
```

###### 10.6.4 Pick

- 能够帮助我们从传入的属性中摘取某一项返回

```typescript
interface Animal {
  name: string;
  age: number;
  gender:number
}
/**
 * From T pick a set of properties K
 * type Pick<T, K extends keyof T> = { [P in K]: T[P] };
 */
// 摘取 Animal 中的 name 属性
interface Person {
    name: string;
    age: number;
    married: boolean
}
function pick<T, K extends keyof T>(obj: T, keys: K[]): Pick<T, K> {
    const result: any = {};
    keys.map(key => {
        result[key] = obj[key];
    });
    return result
}
let person: Person = { name: 'zhufeng', age: 10, married: true };
let result: Pick<Person, 'name' | 'age'> = pick<Person, 'name' | 'age'>(person, ['name', 'age']);
console.log(result);
```

###### 10.6.6 Record

```typescript
function mapObject<K extends string | number, T, U>(obj: Record<K, T>, map: (x: T) => U): Record<K, U> {
    let result: any = {};
    for (const key in obj) {
        result[key] = map(obj[key]);
    }
    return result;
}
let names = { 0: 'hello', 1: 'world' };
let lengths = mapObject<string | number, string, number>(names, (s: string) => s.length);
console.log(lengths);//{ '0': 5, '1': 5 }
```

###### 10.6.7 Proxy

```typescript
type Proxy<T> = {
    get(): T;
    set(value: T): void;
}
type Proxify<T> = {
    [P in keyof T]: Proxy<T[P]>
}
function proxify<T>(obj: T): Proxify<T> {
    let result = {} as Proxify<T>;
    for (const key in obj) {
        result[key] = {
            get: () => obj[key],
            set: (value) => obj[key] = value
        }
    }
    return result;
}
let props = {
    name: 'zhufeng',
    age: 10
}
let proxyProps = proxify(props);
console.log(proxyProps);

function unProxify<T>(t: Proxify<T>): T {
    let result = {} as T;
    for (const k in t) {
        result[k] = t[k].get();
    }
    return result;
}

let originProps = unProxify(proxyProps);
console.log(originProps);
```

###### 10.6.8 映射类型修饰符的控制

- TypeScript中增加了对映射类型修饰符的控制
具体而言，一个 readonly 或 ? 修饰符在一个映射类型里可以用前缀 + 或-来表示这个修饰符应该被添加或移除
- TS 中部分内置工具类型就利用了这个特性（Partial、Required、Readonly...），这里我们可以参考 Partial、Required 的实现

#### 10.7 条件类型

- 在定义泛型的时候能够添加进逻辑分支，以后泛型更加灵活

###### 10.7.1 定义条件类型

```typescript
interface Fish {
    name: string
}
interface Water {
    name: string
}
interface Bird {
    name: string
}
interface Sky {
    name: string
}
//三元运算符
type Condition<T> = T extends Fish ? Water : Sky;
let condition: Condition<Fish> = { name: '水' };
```

###### 10.7.2 条件类型的分发

```typescript
interface Fish {
    fish: string
}
interface Water {
    water: string
}
interface Bird {
    bird: string
}
interface Sky {
    sky: string
}

type Condition<T> = T extends Fish ? Water : Sky;
//(Fish extends Fish ? Water : Sky) | (Bird extends Fish ? Water : Sky)
// Water|Sky
let condition1: Condition<Fish | Bird> = { water: '水' };
let condition2: Condition<Fish | Bird> = { sky: '天空' };
```

###### 10.7.3 内置条件类型

* Exclude: 从 T 可分配给的类型中排除 U

```typescript
type Extract<T, U> = T extends U ? T : never;

type  E = Extract<string|number,string>;
let e:E = '1';
```

* NonNullable: 从 T 中排除 null 和 undefined

```typescript
type NonNullable<T> = T extends null | undefined ? never : T;

type  E = NonNullable<string|number|null|undefined>;
let e:E = 123;
```

* ReturnType: 获取函数类型的返回类型

```typescript
function getUserInfo() {
  return { name: "zhufeng", age: 10 };
}

// 通过 ReturnType 将 getUserInfo 的返回值类型赋给了 UserInfo
type UserInfo = ReturnType<typeof getUserInfo>;

const userA: UserInfo = {
  name: "zhufeng",
  age: 10
};
```

* Parameters
* InstanceType
* infer

## 11. 模块vs命名空间

#### 11.1 模块

- 模块是TS中外部模块的简称，侧重于代码和复用
- 模块在期自身的作用域里执行，而不是在全局作用域里
- 一个模块里的变量、函数、类等在外部是不可见的，除非你把它导出
- 如果想要使用一个模块里导出的变量，则需要导入

```typescript
export const a = 1;
export const b = 2;
export default 'zhufeng';
```

```typescript
import name, { a, b } from './1';
console.log(name, a, b);
```

#### 11.2 命名空间

- 在代码量较大的情况下，为了避免命名空间冲突，可以将相似的函数、类、接口放置到命名空间内
- 命名空间可以将代码包裹起来，只对外暴露需要在外部访问的对象，命名空间内通过export向外导出
- 命名空间是内部模块，主要用于组织代码，避免命名冲突

###### 11.2.1 内部划分

```typescript
export namespace zoo {
    export class Dog { eat() { console.log('zoo dog'); } }
}
export namespace home {
    export class Dog { eat() { console.log('home dog'); } }
}
let dog_of_zoo = new zoo.Dog();
dog_of_zoo.eat();
let dog_of_home = new home.Dog();
dog_of_home.eat();
```

```typescript
import { zoo } from './3';
let dog_of_zoo = new zoo.Dog();
dog_of_zoo.eat();
```

## 总结

![TypeScript小结思维导图](/javascript/TypeScript小结.png)