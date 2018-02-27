---
title: Javascript 기본 문법 및 개념 II
category: Javascript
tag: Javascript
---

JavaScript에 기본적인 문법을 요약,정리한 자료II 입니다.

## 유효범위
유효범위(scope)는 변수의 수명을 의미한다.

```javascript
var vscope = 'global';
// 함수 바깥의 vscope에 접근할 수 있다.
function fscope(){
    alert(vscope);
}
fscope(); // global
// 하지만 지역 변수를 우선적으로 찾아본 다음 없으면 전역 변수를 쓴다.
function fscope2(){
    var vscope = 'local'
    alert(vscope);
}
fscope2();// local

// 다음의 두 개 역시 결과값이 다르다.

// 함수 안에서 var 을 붙여서 변수를 선언한다면 이는 지역 변수가 된다.
function fscope3(){
    var vscope = 'local'
}

fscope3();
alert(vscope) // global

// 함수 안에서 var를 붙이지 않는다면, 전역 변수에 접근해서 바뀌는 것이다.
function fscope4(){
    vscope = 'local'
}

fscope4();
alert(vscope) // local
```

변수 선언시 ```var```를 쓰지 않으면 전역 변수가 되어버리는 것을 알 수 있다. 이는 협업시 변수 이름 중복 등의 문제가 될 수 있으니 ```var```를 붙여서 변수를 선언하는 것이 바람직하다. 하지만 불가피하게 전역 변수를 사용하는 경우의 경우 아래와 같은 방법으로 사용하는 것이 바람직하다.

```javascript
MYAPP = {} // global variable
MYAPP.calculator = {
    'left' : null,
    'right' : null
}
MYAPP.coordinate = {
    'left' : null,
    'right' : null
}

MYAPP.calculator.left = 10;
MYAPP.calculator.right = 20;
function sum(){
    return MYAPP.calculator.left + MYAPP.calculator.right;
}
document.write(sum());
```

이는 익명 함수를 사용함으로써 대체할 수 있다.

```javascript
(function(){
    var MYAPP = {} // local variable
    MYAPP.calculator = {
        'left' : null,
        'right' : null
    }
    MYAPP.coordinate = {
        'left' : null,
        'right' : null
    }
    MYAPP.calculator.left = 10;
    MYAPP.calculator.right = 20;
    function sum(){
        return MYAPP.calculator.left + MYAPP.calculator.right;
    }
    document.write(sum());
}())
```
JS의 지역변수는 함수 안에서만 유효하다. 또한 JS는 함수가 호출되는 시점이 아니라 함수가 선언된 시점에서의 유효범위를 가진다. 이를 **정적 유효범위(static scoping)** 이라고 한다.

```javascript
var i = 5;

function a(){
    var i = 10;
    b();
}

function b(){
    document.write(i);
}

a(); // 5
//호출 될 때의 i 값이 호출되는 것이 아니라 정의 될 때의 i 값인 전역변수가 결과로 나오게 된다.
```

## 값으로서의 함수와 콜백

JS에서는 함수도 객체이며 일종의 값이다. 변수에 함수를 담을 수 있다는 뜻이다. 한편, 객체에 의해서 ```.```으로 호출되는 함수를 특별히 메소드(method)라고 부른다.

```javascript
// 이러한 맥락에서 함수를 인자로 넣을 수 있다.
function cal(func, num){
    // 인자로 들어온 func를 호출 한다.
    return func(num)
}
function increase(num){
    return num+1
}
function decrease(num){
    return num-1
}
alert(cal(increase, 1)); // 2
alert(cal(decrease, 1)); // 0
```

함수는 함수의 리턴 값으로도 사용할 수 있다.

```javascript
function cal(mode){
    var funcs = {
        'plus' : function(left, right){return left + right},
        'minus' : function(left, right){return left - right}
    }
    return funcs[mode];
}
alert(cal('plus')(2,1));
alert(cal('minus')(2,1));   
```

배열의 값으로도 사용할 수 있다.

```javascript
var process = [
    function(input){ return input + 10;},
    function(input){ return input * input;},
    function(input){ return input / 2;}
];
var input = 1;
for(var i = 0; i < process.length; i++){
    input = process[i](input); // 1 -> 11 -> 121 -> 60.5
}
alert(input); // 60.5
```
**콜백(Call-back)** 은 함수가 받는 인자가 함수인 경우를 의미한다.

```javascript
var numbers = [20, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
var sortfunc = function(a, b) {
  console.log(a, b);
  return a - b;
}

// sortfunc가 음수, 양수, 0를 return한다면 대소를 판별한다.
console.log(numbers.sort(sortfunc))
// 이 때 sortfunc를 콜백 함수라 부른다.
```

콜백은 비동기처리에서 유용하게 사용된다. 비동기처리란 시간이 오래 걸리는 작업을 그 시점에서 계속 처리하는 것이 아니라 나중 시점에서 처리할 수 있도록 뒤로 미루고 백그라운드에서 이를 계속해서 처리하는 것을 의미한다. JS에서는 이 같은 작업을 Ajax(Asynchronous Javascript and XML)를 통해서 비동기처리를 한다. 또한 그 때 콜백 함수들이 사용된다.

## 클로저

클로저(closure)는 내부함수가 외부함수의 맥락에 접근할 수 있는 것을 가르킨다. 내부함수, 외부함수, 맥락의 정의는 다음과 같다.

### 내부함수, 외부함수

```javascript
// 외부함수 outer, 내부함수 inner
function outter(){
    function inner(){
        var title = 'coding everybody';
        alert(title);
    }
    inner();
}
outter();

// 내부함수에서 정의되어 있지 않은 title 변수를 외부함수에서 찾을 수 있다.
function outter(){
    var title = 'coding everybody';  
    function inner(){        
        alert(title);
    }
    inner();
}
outter();
```
다음을 통해서 내부함수가 외부함수에 접근하는 것을 볼 수 있다.

```javascript
function outter(){
    var title = 'coding everybody';  
    return function(){        
        alert(title);
    }
}
inner = outter(); // 이 때 outter()는 분명히 종료된다.
inner(); // 하지만 inner()를 호출할 때 outter의 title이 쓰인다. 즉, 외부함수를 사용한다.
```

```javascript
function factory_movie(title){
    // 내부함수 : get_title, set_title
    return {
        get_title : function (){
            return title;
        },
        set_title : function(_title){
            title = _title
        }
    }
}
ghost = factory_movie('Ghost in the shell');
matrix = factory_movie('Matrix');

alert(ghost.get_title()); // "Ghost in the shell"
alert(matrix.get_title()); // "Matrix"

ghost.set_title('공각기동대'); // title은 해당 메소드만을 이용해서 바꿀 수 있다.

alert(ghost.get_title()); // "공각기동대"
alert(matrix.get_title()); // "Matrix"
```

클로저를 사용할 때 흔히 범하는 오류

```javascript
var arr = []
for(var i = 0; i < 5; i++){
    arr[i] = function(){
        return i;
    }
}
for(var index in arr) {
    console.log(arr[index]());
}

/*
 5
 5
 5
 5
 5
 */

 // arr[i]에 담긴 것은 함수이다. 이것은 아직 시행되지 않았다. arr[index]()를 입력해야 비로소 함수가 시행된다. 따라서 이 시행 시점에서의 i값(5)이 반환되게 된다.

 var arr = []
 for(var i = 0; i < 5; i++){
     arr[i] = function(id){
       return function(){
         return id;
       }
     }(i);
 }
 for(var index in arr) {
     console.log(arr[index]());
 }
 /*
  0
  1
  2
  3
  4
  */
// 내부 함수 i 값에서 외부 함수의  값 id를 참조할 수가 있게 된다.
```

## arguments

arguments는 유사 배열이다.

```javascript
// arguments는 JS와 사용자 사이에 약속된 객체이다.
function sum(){
    _sum = 0;    
    for(var i = 0; i < arguments.length; i++){
        document.write(i+' : '+arguments[i]+'<br />');
        _sum += arguments[i];
    }   
    return _sum;
}
document.write('result : ' + sum(1,2,3,4)); // 10
```

FUNCNAME.length 는 함수가 정의한 인자의 개수, arguments.length는 실제로 인자로 들어온 인자의 개수를 의미한다.

```javascript
function zero(){
    console.log(
        'zero.length', zero.length,
        'arguments', arguments.length
    );
}
function one(arg1){
    console.log(
        'one.length', one.length,
        'arguments', arguments.length
    );
}
function two(arg1, arg2){
    console.log(
        'two.length', two.length,
        'arguments', arguments.length
    );
}
zero(); // zero.length 0 arguments 0
one('val1', 'val2');  // one.length 1 arguments 2
two('val1');  // two.length 2 arguments
```

## 함수의 호출

가장 기본적인 방법

```javascript
function func(){

}

func();

function sum(arg1, arg2) {
  return arg1+arg2
}

// 기본적인 방법
sum(1, 2)
```
함수의 내장 메소드인 ```apply```를 사용해 볼 수도 있다. ```apply```는 ```var this = o1; var this = o1```를 암시적으로 선언한다.  

```javascript
o1 = {val1:1, val2:2, val3:3}
o2 = {v1:10, v2:50, v3:100, v4:25}
function sum(){
    var _sum = 0;
    for(name in this){
        _sum += this[name];
    }
    return _sum;
}
alert(sum.apply(o1)) // 6
alert(sum.apply(o2)) // 185
```

## 객체 지향 프로그래밍(Object Oriented Programming)

소프트웨어가 커지면서 같은 기능을 하는 것들 끼리 그룹화 할 필요가 생깁니다. 예컨대 posting이 여러 개 생기면 그 포스팅들은 모두 제목, 본문인 자료가 있고 삭제, 수정 같은 행동을 할 수 있어야 합니다. 여기서 제목, 본문과 같은 부분을 posting의 속성(Property)라고 하고 삭제, 수정과 같은 행동을 posting의 메소드(Method)라고 합니다. 그리고 결국 그 둘이 이루는 posting을 객체라고 부릅니다.

객체 지향 프로그래밍은 현실을 담는 설계도를 만드는 것이라고 할 수 있습니다. 이를 위해서는 현실을 **추상화** 하는 능력이 필요합니다. 그리고 객체를 만든다는 것의 의미는 현실의 한 부분을 **부품화** 하는 것 입니다. 부품화를 한다는 것은 사용자에게 그것을 잘 사용할 수 있도록 도구를 알려주어야 한다는 것 입니다. 이를 **정보의 은닉화 또는 캡슐화** 라고 합니다.

## 생성자와 new

```javascript
var person = {}
person.name = 'egoing';
person.introduce = function(){
  return 'My name is ' + this.name;
}

document.write(person.introduce())

var person = {
  'name' : 'egoing';
  'introduce' : function(){
    return 'My name is ' + this.name;
  }
}

document.write(person.introduce())
```

하지만 이렇게 하다 보면 코드의 중복이 생길 수 있다. ```new```를 사용하면 새로운 객체를 만들어 준다. 함수에 ```new```를 붙이면 객체를 만들어 준다. 객체를 함수를 통해서 정의한다.

```javascript
function Person(name){
  // 초기화(initialization)
  this.name = name;
  this.introduce = function(){
    return 'My name is ' + this.name;
  }
}

var p1 = new Person('egoing');
var p2 = new Person('kky');
```

## 전역객체

모든 전역변수와 함수는 사실 window 객체(전역 객체)의 속성. 객체를 명시하지 않으면 암시적으로 window의 속성로 간주된다. node.js에서는 window 대신 global이 이 역할을 한다.

```javascript
var o = {'func':function(){
   alert('Hello?');
}}

o.func();
window.o.func();
// global.o.func();
// node.js의 경우
```

## this

this는 함수 내에서 함수 호출 맥락을 의미한다. 따라서 상황에 따라서 가르키는 것이 달라진다. 전역 함수에서는 전역 객체 window를 의미한다.

```javascript
// javascript
function func(){
    if(window === this){
        document.write("window === this");
    }
}
func(); //window === this
```
this의 값은 함수가 소속된 객체를 의미한다.

```javascript
var o = {
    func : function(){
        if(o === this){
            document.write("o === this");
        }
    }
}
o.func();   // o === this
```

함수의 맥락에서는 this가 window를 가르키지만 생성자의 맥락에서는 this는 생성되는 객체를 의미한다.

```javascript
var funcThis = null;

function Func(){
    funcThis = this;
}
var o1 = Func();
if(funcThis === window){
    document.write('window <br />');
}
// window

var o2 = new Func();
if(funcThis === o2){
    document.write('o2 <br />');
}
// o2
```

리터럴이라는 것은 객체 생성 과정에서 문법적 편의를 위해서 허용되는 것이다. 이를 통해서 함수도 객체라는 것을 알 수 있다.

```javascript
var a = new Object();
a.name = 'Hello';
// 객체 리터럴
var a = {name : "Hello"} ;

var b = new Array(1, 2, 3);
// 배열 리터럴
var b = [1, 2, 3];

var sum = new Function('x', 'y', 'return x+y;');
// 함수 리터럴
function sum(x, y) {return x+y;}
```

함수의 메소드인 apply, call을 이용하면 this의 값을 제어할 수 있다. 함수가 누구의 소속이냐에 따라서 함수가 속하는 객체 즉, this 값이 달라진다.

```javascript
var o = {}
var p = {}
function func(){
    switch(this){
        case o:
            document.write('o<br />');
            break;
        case p:
            document.write('p<br />');
            break;
        case window:
            document.write('window<br />');
            break;          
    }
}

func(); // window
func.apply(o); // o
func.apply(p); // p
```

## 상속

상속이란 자식 객체가 부모 객체를 물려받은 것입니다. 부모 객체에서 필요한 기능만 취하고 더할 수 있는 것은 더해서 자식 객체를 만듬으로써 코드의 재사용성을 취하고 맥락에 맞는 객체를 만들 수 있는 것 입니다.

```javascript
function Person(name){
    this.name = name;
}
Person.prototype.name=null;
Person.prototype.introduce = function(){
    return 'My name is '+this.name;
}

function Programmer(name){
    this.name = name;
}

function Designer(name){
    this.name = name;
}
// 상속되는 부분
// 생성자.prototype 에 상속되는 부모 객체를 넣는다.
// 두 번째 처럼 새로운 function을 넣을 수 도 있다.
Programmer.prototype = new Person();
Programmer.prototype.coding = function(){
  return "hello world"
}

Designer.prototype = new Person();
Designer.prototype.design = function(){
  return "beautiful!";
}



var p1 = new Programmer('egoing');
document.write(p1.introduce()+"<br />");
document.write(p2.coding()+"<br />")

var p2 = new Designer('lee');
document.write(p2.introduce()+"<br />");
document.write(p2.design()+"<br />")
```

## prototype

다음과 같은 상속 관계가 있다고 하자.(Ultra-Super-Sub) 함수 앞에 ```new```를 붙이면 생성자 함수가 된다. prototype은 객체의 속성을 받아오는 것을 의미한다. 한편 하나의 객체의 속성에 접근하면 자식부터 그 속성 여부를 확인하고 상위로 올라가면서 이를 또 뒤진다.

```javascript
function Ultra(){}
Ultra.prototype.ultraProp = true;

function Super(){}
Super.prototype = new Ultra();

function Sub(){}
Sub.prototype = new Super();

var o = new Sub();
console.log(o.ultraProp); // true
```

## 표준 내장 객체의 확장(Standard Built-in Object)

표준 내장 객체는 JS가 기본적으로 가지고 있는 객체들을 의미한다.참고로 호스트 환경에 따라서 제공 받는 객체가 다를 수도 있다. 다음은 표준 내장 객체의 리스트이다.

- Object
- Function
- Array
- String
- Boolean
- Number
- Math
- Date
- RegExp

표준 내장 객체와 반대되는 개념은 사용자 정의 객체이다. 또한 표준 내장 객체를 사용자 정의 함수를 통해서 확장시킬 수 있다. 다음은 Array를 확장 시킨 예제이다.

```javascript
// Array에 추가하는 작업
Array.prototype.random = function(){
  var index = Math.floor(this.length*Math.random());
  return this[index];
}

var arr = new Array('seoul','new york','ladarkh','pusan', 'Tsukuba');
console.log(arr.random());
```

Object 객체는 모든 객체의 원형이다. 따라서 Object를 확장한다면 모든 객체에서 이를 사용할 수 있을 것이다. 즉, ```Object.prototype.func(){};``` 를 정의한다면 모든 객체에서 func(){}를 쓸 수 있습니다. 다음을 Object에 대한 확장이다.

```javascript
Object.prototype.contain = function(needle) {
    for(var name in this){
        if(this[name] === needle){
            return true;
        }
    }
    return false;
}
var o = {'name':'egoing', 'city':'seoul'}
console.log(o.contain('egoing')); // true
var a = ['egoing','leezche','grapittie'];
console.log(a.contain('leezche')); // true
```

다만 이렇게 Object같은 광역 객체에 메소드를 추가하는 것은 위험합니다. 예컨대 아래와 같은 문제가 있습니다.

```javascript
for(var name in a){
  console.log(name);
}

/*
egoing
leezche
grapittie
contain
*/
```

모든 객체에 contain이 추가되버려서 우리의 예상과 다르게 결과가 나오는 것을 알 수 있다.

## 데이터 타입

데이터 타입은 원시 데이터 타입과 참조 객체 데이터 타입으로 분류됩니다.

### 참조(레퍼) 객체

```javascript
var str = 'coding';
// str = new String('coding');
console.log(str.length); // 6
console.log(str.charAt(0)) // 'C'
```

문자열은 분명히 Property와 메소드가 있다. 그렇다면 문자열이 객체라는 소리이다. 하지만 문자열을 객체라고 부르지는 않고 원시 데이터 타입이라고 부른다. 문자열과 관련되 어떤 작업을 하려고 할 때 JS는 임시로 문자열 객체를 만들고 사용이 끝나면 제거한다.(이러한 처리는 내부적으로 일어난다.) 이렇게 임시로 만들지는 객체를 래퍼 객체(wrapper object)라고 한다. 다음은 래퍼 객체 리스트이다.

- 숫자 --> Number
- 문자열 --> String
- 불리언 --> Boolean
- null --> x
- undefined --> x

## 참조

### 복제

시스템 위의 데이터를 복제 하는데는 비용이 거의 들지 않는다. 이러한 특징이 소프트웨어가 기존 산업과 다른 점이다. 다음을 복제를 하는 경우이다.

```javascript
var a = 1;
var b = a;
b = 2;
console.log(a); // 1
```
### 참조

다음은 우리의 직관과 다르게 코드가 동작되는 경우이다. 원시 데이터 타입이 아닌 객체는 위와 다르게 동작한다.

```javascript
var a = {'id':1};
var b = a;
b.id = 2;
console.log(a.id);  // 2
```

<center><a href="https://imgur.com/JpI5fkr"><img src="https://i.imgur.com/JpI5fkr.png" width="500px" height="600px" title="source: imgur.com" /></a></center>

객체를 참조하게 되면 같은 객체를 참조하게 된다. 따라서 b를 수정한다면 a 역시 달라지게 되는 것이다. 한편 객체를 새로 만들게 되면 참조 화살표가 바뀌게 된다.

```javascript
var a = {'id':1};
var b = a;
// b는 다른 객체를 가르키게 된다.
b = {'id':2};
console.log(a.id);  // 1
```
### 함수와 참조 인자

함수로 위와 같은 관계를 파악할 수도 있다.

```javascript
var a = {'id':1};
function func(b){
    b.id = 2;
}
func(a);
// a, b가 같은 객체를 참조한다.
console.log(a.id);  // 2
```

```javascript
var a = {'id':1};
function func(b){
    b = {'id':2};
}
func(a);
// a, b가 다른 객체를 참조한다.
console.log(a.id);  // 1
```
