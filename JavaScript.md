# Javascript

## Uncategorized
```
var cars = [
    {make:"BMW", model:"X5"},
    {make:"Benz", model:"385"}
];

for (var i = 0; i < cars.length; i++) {
	(function(i) {
    	this.print = function() {
        	console.log("Make:" + this.make + ", model:" + this.model);
		};
		this.print();
	}).call(cars[i], i);
}
```

## Basics
#### Hoisting
Lift declaration to the top (not initialization)

#### Storage
1. Window.localStorage: across session
2. Window.sessionStorage: live for one session, refresh is ok, die upon closing tab
3. Session cookies: live until the WINDOW is closed. Plain text for user, encrypted on only https. Cookies will be sent everytime a user sends a request, so try to avoid overloading it. Put authentication in cookies. Plain text for user, encrypted on only https. 

## Operation 

1. `undefined` means there is nothing, not even `null`
2. Object通过它的toString方法被强制转换为字符串,通过它的valueOf方法被强制转换为数字。带有valueOf方法的Object应该实现一个toString方法,这个toString方法返回的字符串就是那个valueOf返回的数字的字符串表示形式:
    ```
    var obj = {
        toString: function() {
            return 'obj';
        },
        valueOf: function() {
            return 1;
        }
    };

    console.log(1 + obj); // 2
    console.log('Hi' + obj); // Hi1
    ```
3. 判断一个值是否是未定义的应该使用typeof或者比较的方法,而不是根据这个值表现是true或者false来判断, because true is taken as 1, false is taken as 0
    ```
    function point(x, y) {
        if(typeof x === undefined || y === undefined) {
            return {
                x: x || 1,
                y: y || 1
            }
        }
        return {
            x: x,
            y: y
        }
    }
    console.log(point(0, 0)); // { x: 0, y: 0 }
    ```

1. 01 + "05" = 105 (string)
2. "01" + 05 = 015 (string)
3. 10 + null or null + 10 = 10
4. "test" + null or null + "test" = "testnull" or "nulltest"
5. 10 + true = 11
6. 10 + false = 10 
7. isNaN({} / undefined / NaN / 'foo' / {valueOf: 'foo'}) all true
8. Divide by 0 will give Infinity
9. 8 | 1 = 9


```
// Same as Java (+)
"a" + 1 + 2 // a12
1 + 2 + "a" // 3a

// Different from Java (-, *, /)
1 - "2" // -1        
"2" - "1" // 1
"100" / 10 // 10
"10" * "10" // 100
```


#### Equal Sign
1. 当使用==操作符进行相等的比较操作的时候,如果它的两个参数的类型是不一样的; 那么==会把它们先强制转换为相同类型参数,然后再进行比较
2. 使用===表明你的比较不会涉及任何的隐形的类型转换
3. 当对不同类型的数据进行比较的时候,你要首先把它们进行显示的类型转换。 然后再进行比较,这样会使你的程序更加清晰

    |     Argument Type 1     |     Argument Type 2     |     Coercions     |
    | ----------------------- | ------------------------| ------------------|
    | null                    | undefined               | None; always true |
    | null or undefined       | Anything other than null or undefined | None; always false |
    | Primitive string, number, or boolean | Date object | Primitive => number, Date object => primitive (try toString and then valueOf) |
    | Primitive string, number, or boolean | Non-Date object | Primitive => number, non-Date object => primitive (try valueOf and then toString) |
    | Primitive string, number, or boolean | Primitive string, number, or boolean | Primitive => number |

    Example of: primitive => number example
    ```
    var y = {valueOf: function() {return "3"}}; 
    y == 3 // true
    y === 3 // false

    var price = "15";
    console.log(+price === 15); // true
    ```

4. NaN == NaN, NaN === NaN, both false
5. {} == {}, {} === {}, both true
6. null == null, null === null, both true

## Array
```
array.join(" "); // get all elements concatenated on space
var array = [1,2,3];
var array  = new Array(1,2,3);
array.shift/unshift()   pop/peek the head
array.slice(start, end)
array.splice(index, numberOfElementsToRemove,( newItemsToBeAdded));
array.forEach(function(color) { // anonymous function
  console.log(color);
});
```

## Variables 
1. Undeclared variables: implicit globals
2. let: limits to block. It is not available using "var", which only limits to functions.
for (x in person)
3. const: cannot be reassigned but can be changed (e.g. array)

[Pointer example](http://www.cnblogs.com/vajoy/p/3703859.html)
```
var a = {n:1};
var b = a;
a.x = a = {n:2};
a.x; // undefined
b.x; // {n: 2}
```
1. a points to {n:1}
2. b points to a, the same, {n:1}
3. "." operation has the highest priority, so a.x (x:undefined) added to {n:1} ({n:1, x:null})
4. perform the operation from right to left: a points to {n:2}
5. a.x (still {n:1, x:null}) points to {n:2} (current a) and becomes {n:1, x:{n:2}}, which b already points to

#### undefined v.s null 
1. undefined == null? true, undefined === null? false
2. undefined is invalid as default parameter, null is valid


## Functions
1. Function expression: var fun = function() {...}; parsed when executed, cannot be hoisted
2. Function declaration: function fun() {...}; parsed when encountered, can be hoisted
3. Immediately Invoked Function Expression (self-executing anonymous function): avoid variable hoisting
https://www.tutorialspoint.com/es6/es6_functions.htm
4. Rest parameter: only one allowed, as last parameter

### Generator function
```
function* ask() { 
   const name = yield "What is your name?"; 
   const sport = yield "What is your favorite sport?"; 
   return `${name}'s favorite sport is ${sport}`; 
}  
const it = ask(); 
console.log(it.next()); 
console.log(it.next('Ethan'));  
console.log(it.next('Cricket')); 
```

### Arrow Function 
1. Parenthesize the body of function to return an object: `params => ({foo: bar})`

## Object
JavaScript objects cannot be compared, use JSON or loop the properties

### Inheritance
```
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price, catergory) {
  Product.call(this, name, price);
  this.category = catergory;
}

function Toy(name, price) {
  Product.call(this, name, price);
  this.category = 'toy';
}

var cheese = new Food('feta', 5, "food");
var fun = new Toy('robot', 40);
```

### Prototype Chain
``` 
Object.create()
var o = {
  a: 2,
  m: function() {
    return this.a + 1;
  }
};

console.log(o.m()); // 3
var p = Object.create(o); // p is an object that inherits from o
```

## Closure

1. Whenever you use function inside another function, a closure is used.
2. A closure in JavaScript is like keeping all the local variables stay alive in the status as they were even after the function is exited.

Example 1:
```
function name() {
  console.log(alice);
  var alice = "Alice";
}
name(); // undefined, hoisting

function name() {
    var say = function() { console.log(alice); }
    var alice = "Alice";
    return say;
}
name()(); // Alice
```
The local variables of name() are kept in a closure, so we can still get result from say()
Closure keeps a stack-frame in memory when the outer function exits

Example 2:
```
var gLogNumber, gIncreaseNumber, gSetNumber;
function setupSomeGlobals() {
  // Local variable that ends up within closure
  var num = 42;
  // Store some references to functions as global variables
  gLogNumber = function() { console.log(num); }
  gIncreaseNumber = function() { num++; }
  gSetNumber = function(x) { num = x; }
}

setupSomeGlobals();
gIncreaseNumber();
gLogNumber(); // 43  <==== three functions reference to the same, existing closure, rather than a new copy
gSetNumber(5);
gLogNumber(); // 5

var oldLog = gLogNumber;

setupSomeGlobals();
gLogNumber(); // 42 <==== three functions reference to the same closure

oldLog() // 5
```
Example 3:
```
var getI;
function buildList(list) {
    var result = [];
    for (var i = 0; i < list.length; i++) {
        var item = 'item' + i;
        result.push( function() {console.log(item + ' ' + list[i])} );
        console.log(item) // item0, item1, item2, normal
    }
    getI = function(){console.log(i);}
    // item = "changed"; // if we change item here, all following "item2 undefined" will be ""changed undefined"
   
   // logs "item2 undefined" 3 times, referring to the stack already
    for (var j = 0; j < result.length; j++) {
        result[j]();
    }
    return result;
}

function testList() {
    var fnlist = buildList([1,2,3]);

    for (var j = 0; j < fnlist.length; j++) {
        fnlist[j](); // the function use the current value of item and i in the single closure (where i = 3, item = item2.)
    }
}
getI(); // i is already 3. So we 
testList() //logs "item2 undefined" 3 times
```
Example 4: 
There is not a single closure per function declaration. There is a closure for each call to a function.
```
function newClosure(someNum, someRef) {
    // Local variables that end up within closure
    var num = someNum;
    var anArray = [1,2,3];
    var ref = someRef;
    return function(x) {
        num += x;
        anArray.push(num);
        console.log('num: ' + num +
            '; anArray: ' + anArray.toString() +
            '; ref.someVar: ' + ref.someVar + ';');
      }
}
obj = {someVar: 4};
fn1 = newClosure(4, obj);
fn2 = newClosure(5, obj);
fn1(1); // num: 5; anArray: 1,2,3,5; ref.someVar: 4;
fn2(1); // num: 6; anArray: 1,2,3,6; ref.someVar: 4;
obj.someVar++;
fn1(2); // num: 7; anArray: 1,2,3,5,7; ref.someVar: 5;
fn2(2); // num: 8; anArray: 1,2,3,6,8; ref.someVar: 5;
```
Example 5: 
```
for(var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(i);  
    }, 1000);
}
```

## Regex
Using a regular expression literal, which consists of a pattern enclosed between slashes, as follows:
```
var re = /exp/g;   // same as new RegExp('exp', 'g');
```

## CSS
1. All children: X Y {color:blue;}
2. All direct children: X > Y {color:blue;}
3. Immediate sibling: X + Y {color:blue;}
4. All preceding sibling: X ~ Y {color:blue;}

## Trick
```
function(num) {
  var value = num || 10; // if num is 0, value becomes 10 instead of 0, as 0 is evaluated "false"
}
```