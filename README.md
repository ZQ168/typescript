# typescript
首先要安装typescript


## 基本数据类型：
一、布尔类型：  
boolean   true  false
```
let isDone: boolean = false;
```
二、数字  :number  
```
let num: number = 6;
```
三、字符串  :string  
```
let str: string = "bob";
```
四、数组类型:  
在 TypeScript 中，数组类型有多种定义方式，比较灵活。  
1、「类型 + 方括号」表示法  
```
let fibonacci: number[] = [1, 1, 2, 3, 5];
```
2、数组泛型  
我们也可以使用数组泛型（Array Generic）Array<elemType>来表示数组 
```         
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```
3、用接口表示数组  
```
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5]
```
数组：建议使用前两个方法，比较简单  

arguments 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口：  
```
function sum() {  
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}
```
五、元组类型：属于数组的一种，可以指定值（单个值）的类型  
```
let  arr:[string,number,boolean]=[‘1’,1,false];
```
六、枚举类型  
随着计算机的不断普及，程序不仅仅只用于数值计算，还更广泛地用于处理非数值数据  
事先考虑某一个变量可能取的值，尽量用自然语言中含义清楚的单词来表示每一个值，这种方法称为枚举方法，把这种方法定义的类型称为枚举类型  
enum类型是对JavaScript标准数据类型的一个补充
```
Enum  枚举名{
	标识符[整型常量]
}
Enum Flag{success=1,error=-1}
Var f:Flag=Flag.success
```
默认没有赋值的话，会打印索引值  
```
Enum Color{red,blue,orange}
var c:Color = Color.orange
```
七、任意类型any  (anyscript)  
八、null和undefined   使用联合类型
```
let s:string | null | undefined = undefined;
```

九、void类型  
Typescript中的void表示没有任何类型，一般定义方法的时候方法没有返回值  
使用：void来表示一个函数没有一个返回值  
```
function log(message:string):void{}
```
十、never类型   
never类型表示的是那些永不存在的值的类型。  
例如，never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是never类型，当它们被永不为真的类型保护所约束时。  
```
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {

    }
}
```
其他类型值包含null和undefined的子类型，代表不会出现的值。这意味着never的变量只能被never类型所赋值   
类型断言   
通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。  
类型断言有两种形式。 
其一是“尖括号”语法：
```
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```
另一个为as语法：
```
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```
### 函数  
```
// Named function
function add(x, y) {
    return x + y;
}

// Anonymous function
let myAdd = function(x, y) {  
    return x + y; 
};
```
让我们为上面那个函数添加类型：
```
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { 
    return x + y; 
};
```
可选参数和默认参数   
```
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

let result1 = buildName("Bob");  // works correctly now
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");  // ah, just right
```
剩余参数  
```
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```

ts函数的重载  
Java中方法的重载：重载指的是两个或两个以上同名的函数，但他们参数不一样，这样就会有函数重载的情况  
Typescript中的重载，通过为同个函数与提供多个函数类型定义来实现多种功能的目的  
函数参数解构的写法：
```
function f({ x: number }) {
  // Error, x is not defined?
  console.log(x);
}
function f({x}:{x:number}) {
  console.log(x)
}
```
### 泛型：  
使用any类型会导致这个函数可以接收任何类型的arg参数，这样就丢失了一些信息：传入的类型与返回的类型应该是相同的。如果我们传入一个数字，我们只知道任何类型的值都有可能被返回。  
因此，我们需要一种方法使返回值的类型与传入参数的类型是相同的。这里，我们使用了类型变量，它是一种特殊的变量，只用于表示类型而不是值  
```
function  identity<T>(arg:T):T {
    return arg;
}
```
我们给indentity添加了类型变量T。T帮助我们捕获用户传入的类型（比如：number），之后我们就可以使用这个类型。之后我们再次使用了T当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。这允许我们跟踪函数里使用的类型的信息。  
我们把这个版本的identity函数叫做泛型，因为它可以适用于多个类型。不同于使用any，它不回丢失信息，想第一个例子那像保持准确性，传入所有的参数，包含类型参数  
```
let output = identity<string>(“myString”);
```
这里我们明确的指定了T是string类型，并做为一个参数传给函数，使用了<>括起来而不是（）  
第二种方法更普遍。利用了类型推论—即编译器会根据传入的参数自动地帮助我们确定T的类型  
```
let  output = indetity(“myString”);
```
注意我们没必要使用尖括号（<>）来明确地传入类型；编译器可以查看myString的值，然后把T设置为它的类型。类型推论帮助我们保持代码精简和高可读性。如果编译器不能够自动地推断出类型的话，只能像上面一样明确传入T的类型，在一些复杂的情况下，这是可能出现的  
泛型类型  
In dentity通用函数，可以适用于不同的类型。我们研究一下函数本身的类型，以及如何创建泛型接口
泛型函数的类型与非泛型函数的类型没什么不同，只是有一个类型参数在最前面，像函数声明一样：  
### 接口（interfaces）    
接口的作用：在面向对象的编程中，接口是一种规范的定义。定义了行为和动作的规范。在程序设计中，接口起到了一种限制和规范的作用。接口定义了某一批所需要遵守的规范。  
Typescript中的接口分类  
1、	属性类接口  
2、	函数类接口  
3、	可索引接口  
4、	类类型接口  
5、	接口扩展  
属性接口 对json的约束  
ts中定义方法传入参数对json的约束  
行为和动作的规范，对批量方法进行约束  
接口可选属性（参数的顺序无所谓）  
```
interface Config{
	type:string;
	url:sting;
	data?:string;
	datatype:string;
}
function ajax(config:Config){
	car xhr= new XMLHttpRequest();
	xhr.open(config.type,config.url,true);
	xhr.send(config.data);
	xhr.readystatechange=function(){
		if(xhr.readyState===4 && xhr.status==200){
	
        }
    }
}
```
```
interface Person {
	name:sting;
	age:number;
}
let tom:Person={
	name:’tom’,
	age:25
}
```
interface
interface User {
  name: string
  age: number
}

interface SetUser {
  (name: string, age: number): void;
}

type类型：  
type User = {
  name: string
  age: number
};

type SetUser = (name: string, age: number)=> void;
### 类（Classes） 
ts表示类  
```
class Person{
	name:string;   // 属性前面省略，代表public
	constructor(name:string) { // 构造函数，实例化类的时候触发
	    this.name=name
    }
    run():void{

    }
}
``` 
类里面的修饰符  
typescript中提供三种修饰符  
public   公有，在类里面、子类、类外面都可以访问  
protected    保护类型，在类里面，子类里面可以访问，在类外部都没法访问  
private 私有 在类里面可以访问、子类，类外部都没法访问  
默认不加修饰符就是public  
理解private  
当成员被标记成private时，它就不能在声明它的类的外部访问。比如：
```
class Animal {
    private name: string;
    constructor(theName: string) { 
        this.name = theName; 
    }
}
new Animal("Cat").name; // 错误: 'name' 是私有的.
```  
理解protected  
protected修饰符与private修饰符的行为很相似，但有一点不同，protected成员在派生类中仍然可以访问。例如：  
```
class Person {
    protected name: string;
    constructor(name: string) { 
        this.name = name; 
    }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name)
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name); // 错误
```
readonly修饰符  
使用readonly关键字将属性设置为只读的。
```
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}
let dad = new Octopus("Man with the 8 strong legs");
dad.name = "Man with the 3-piece suit"; // 错误! name 是只读的.
```
typescript中实现继承  (省略)  
静态属性、静态方法  
Es5的静态方法的表示：  
```
function Person{
	this.run=function(){
        
    }
}
Person.sex='男'; // 静态属性
Person.print=function(){  // 静态方法

}
```

静态方法：  
```
class  Person{
	public name:string
	static sex:'男'
	constructor(name:string){
		this.name=name
    }
    run(){  // 实例方法
        alert(this.name)
    }
    static print(){
        console.log(this.name) // 报错 静态方法中没有办法直接调用类中的属性，只能调用静态属性,this.sex可用
    }
}
```
实例方法的调用：  
```
var p = new Person(‘张三’)
p.run();
```
静态方法的调用
```
Person.print()
```

多态属于继承  
抽象方法、抽象类  
abstract用来表示抽象    
抽象方法只能放在抽象类中  ,抽象类和抽象方法用来定义标准
```
abstract class Animal{
	abstract eat():any
}
```
abstract类无法创建实例  
```
var a =new Animal(); // 报错
```
抽象类的抽象方法在子类就必须要实现。不实现就会报错  










类型别名声明（type sn=number|string;）  
接口声明（interface I{x:number[];}）  
类声明（class c{}）  
枚举声明（enum E{A,B,C}）  
指向某个类型的import声明  

学习成本：
需要理解接口（interfaces）、泛型（Generics）、类（Classes）、枚举类型（Enums）

我们定义了一个接口Person，接着定义了一个变量tom。它的类型是Person。这样，我们就约束了tom的形状和接口Person一致



可调用的
可以使用类型别名或者接口来表示一个可被调用的类型注解

单例模式
传统的单例模式可以用来解决所有代码必须写到class中的问题
class Sigleton {
	private static indatnce:Singleton;
	private constructor() {
}
Public static getInstance(){
	If(!Singleton.instance) {
	Sigleton.insance=new Singleton();
}
Return Singleton.instance;
}
}


let someThing = new Singleton(); // Error: constructor of 'singleton' is private

let instacne = Singleton.getInstance(); // do some thing with the instance




Var p =new Person();

多态：父类定义一个方法不去实现，让继承它的子类去实现，每个子类有不同的表现

抽象类：typescript的抽象类，它是提供其他继承的基类，不能直接被实例化。
用abstract关键字定义的抽象类和抽象方法。抽象类中的抽象方法不包含其实现并且必须在派生类中实现。

