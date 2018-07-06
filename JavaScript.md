# Javascript

- [Basics](#Basics)
- [Operation](#Operation)
- [Variables](#Variables)
- [Functions](#Functions)
- [Array](#Array)
- [Object](#Object)
- [Prototype Chain](#PrototypeChain)
- [Closure](#Closure)
- [Error Handling](#ErrorHandling)


<a id="Basics"></a>  
## Basics
### Hoisting
Lift **declaration** to the top (not initialization)

#### Example 1
```
var x = 5;

(function () {
    console.log(x);
    var x = 10;
    console.log(x); 
})();
```
is equal to 
```
var x = 5;

(function () {
    var x;
    console.log(x);
    x = 10;
    console.log(x); 
})();
```

#### Example 2
```
var x = y, y = 'A';
console.log(x + y); // undefinedA
```
```
var x = y, y = 'A';
console.log(y);     
console.log(x + y); // undefinedA
```

#### Function hoisting: hoisting the entire function rather than the declaration
```
function testOuter() {
    function number() { return 1; }

    function testInner() {
        console.log("inner: " + number());
        function number() { return 2; }
    };
    testInner();
    console.log("outer: " + number());
}

testOuter();
```

### Storage
1. Window.localStorage: across session
2. Window.sessionStorage: live for one session, refresh is ok, die upon closing tab
3. Session cookies: live until the WINDOW is closed. Plain text for user, encrypted on only https. Cookies will be sent everytime a user sends a request, so try to avoid overloading it. Put authentication in cookies. Plain text for user, encrypted on only https. 

<a id="Operation"></a>  
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
    if num is 0, value becomes 10 instead of 0, as 0 is evaluated "false"
    ```
    function(num) {
    var value = num || 10;
    }
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


### Equal Sign
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

### for loop
1. `for-in` loops through the **enumerable** property names of an object, not the indexes of an array. Avoid it in iteration, it will include `Array.prototype.foo`.
2. old-fashioned indexing: low efficiency in sparse arraies (a[0], a[99999])
3. `for each`: recommended

<a id="Variables"></a>  
## Variables 
1. Undeclared variables: implicit globals
2. const: cannot be reassigned but can be changed (e.g. array)
3. null is explicit empty. var person = null The value of person is null but typeof person is object, which is considered a bug
4. You can re-declare a variable. If you don't assign a new value, it will keep the old one:
    ```
    var carName = "Volvo";
    var carName;  // still "Vovlo"
    ```
5. Lifetime: Local variables are deleted when the code block is completed; Global variables are deleted when you close the page
6. The global scope is the window object, all global variables belong to it: window.myVar

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

### undefined v.s null 
1. undefined == null? true, undefined === null? false
2. undefined is invalid as default parameter, null is valid


### NaN - Not a Number
1. Type is number.
2. Operate a number with non-numeric string: `var x = 100 / "Apple"`;
3. Operate a number with a NaN
    ```
    var x = NaN, y = 5;
    var z = x + y;   
    ```
4. Operate a number with an undefined value
    ```
    var x, y = 5;
    var z = x + y;         
    ```
5. Operate two non-numeric values: `"Hello" - "Dolly"`
6. Operate a string with a NaN
    ```
    var x = NaN, var y = "5";
    var z = x + y;  // z = NaN5
    ```

### let
1. `let` allows you to limit the scope to the block, statement, or expression on which it is used, unlike var, which defines variables for at least the scope of a function
    ```
    function varTest() {
    var x = 1;
    if (true) {
        var x = 2;  // same variable!
        console.log(x);  // 2
    }
    console.log(x);  // 2
    }

    function letTest() {
    let x = 1;
    if (true) {
        let x = 2;  // different variable
        console.log(x);  // 2
    }
    console.log(x);  // 1
    }
    ```
2. `let` does not create a property on the global object (window)
    ```
    var x = 'global';
    let y = 'global';
    console.log(this.x); // global
    console.log(this.y); // undefined
    ```
3. `let` cannot declared multiple times, it will raise syntax error. 'seitch' is considered as one block, there should be no duplicated 'let' in it.
    ```
    let a = 5;
    let a = 6; // Uncaught SyntaxError: Identifier 'a' has already been declared
    ```
4. `let` will hoist, it will be in a "dead zone", so referring it before declaration will cause ReferenceError: not defined
```
console.log(a)
let a = 6; // Uncaught ReferenceError: a is not defined
```
```
let a;
console.log(a) // undefined
```

<a id="Functions"></a>  
## Functions
1. Function expression: var fun = function() {...}; parsed when executed, cannot be hoisted
2. Function declaration: function fun() {...}; parsed when encountered, can be hoisted
3. Immediately Invoked Function Expression (self-executing anonymous function): avoid variable hoisting
https://www.tutorialspoint.com/es6/es6_functions.htm
4. Rest parameter (...args): only one allowed, as the last parameter. All rest params will be wrapped in an array

### Arrow Function 
1. Parenthesize the body of function to return an object: `params => ({foo: bar})`

### `call` Keyword
1. Allow object to use others' functions
2. Define the invoker of functions
3. We pass the caller of the function as the first param, but it's optional  
    `function.call([this][, arg1][, arg2],...)`
    if it's not passed, "this" is bound to global object 


#### Example 1
```
var obj1 = {
    sayHello: function(msg) {
        console.log("Hello," + this.name + " " + msg);
    },
    name: "John"
};

var obj2 = {
    name: "Mary"
};

var name = "global"

obj1.sayHello.call(obj2, "haha"); // Hello, Mary haha
obj1.sayHello.call("haha") // Hello, undefined undefined
obj1.sayHello.call() // Hello, global undefined
```
#### Example 2
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

### High-order Function
1. High-order function: takes another function as an argument, or that returns a function as a result.
2. Callback function: a function that gets executed at the end of an operation, once all of the other operations of been completed. Thanks to JS's single-thread nature. The ability to pass a callback function is critical when dealing with resources that may return a result after an undetermined period of time.

```
var attitude = function(original, replacement) {
  return function(source) {
    return source.replace(original, replacement);
  };
};

var snakify = attitude(/millenials/ig, "Snake People");
var hippify = attitude(/baby boomers/ig, "Aging Hippies");

console.log(snakify("The Millenials are always up to something."));
// The Snake People are always up to something.
console.log(hippify("The Baby Boomers just look the other way."));
// The Aging Hippies just look the other way.
```


<a id="Array"></a>  
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

1. `forEach(function callback(task[, index][, array]))`: process each element
2. `map(function callback(task[, index][, array]))`: process each element and return the array
3. `filter(function callback(task[, index][, array]))`: get elements matching the filter
4. `reduce(function callback(accumulator, currentValue[, index][,array])[, initial])`: 
    ```
    function combine(...arrays) {
     return arrays.reduce((previous, current) => {return previous.concat(current);}, [0]);
    }
    ```

5. `splice()`:  
    var nums = [1, 2, 3, 4];
    1. `splice(startIndex)`: nums.splice(2); //return [3, 4], nums = [1, 2]
    2. `splice(startIndex, deleteCount)`: nums.splice(2, 1); //return [3], nums = [1, 2, 4]
    3. `splice(startIndex, deleteCount, ...[values])`:  
        nums.splice(2, 1, ...[5, 6]); // return [3], nums = [1, 2, 5, 6, 4]


### Rest parameters and Spread operator 
1. Looks the same `...array`
2. Rest parameters (array instance): allows us to represent an indefinite number of arguments as an array
    ```
    function sum(a, b) {
        return a + b;
    }
    console.log( sum(1, 2, 3, 4, 5) ); // 3, but no error

    var sumAll = (...args) => { 
        let sum = 0;
        for (let arg of args) sum += arg;
        return sum;
    }
    console.log( sum(1, 2, 3, 4, 5) ); // 15
    ```
    Difference between `arguments` object and rest param:
    1. `arguments` is both array-like and iterable, but still not an array, so we can't use array functions on it
    2. `arguments` does not exist in arrow functions
    3. `arguments` contains all params, rest param contains only undefined params


3. Spread operator: turns an iterable (String, Array, Map, Set, ...) into arguments of a function or into elements of an array
    ```
    Math.max(...array);
    array1.push(...array2);
    [1, ...[2,3], 4]; // [1,2,3,4]
    const arr = [...array1, ...array2, ...array3]
    ```
    Difference between `Array.from()` and spread operator:
    1. `Array.from()` operates on both array-likes and iterables, but the spread operator operates only on iterables
    2. `Array.from()` generates an array strictly: [1, Array.from([2,3]), 4] ---> [1, [2, 3], 4]
    Work on objects too:
    ```
    var state = {"a": 1, "b": 2};
    var add = {"c":3, "d": 4};
    console.log({...state, ...add}); // {a: 1, b: 2, c: 3, d: 4}
    ```

[Rest parameters and spread operator](https://javascript.info/rest-parameters-spread-operator)  
[Rest parameters MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)


<a id="Object"></a>  
## Object
1. JavaScript objects cannot be compared, (x == y) is always false, use JSON or loop the properties

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
<a id="PrototypeChain"></a>  
## Prototype Chain
1. prototype: Only in functions.
2. getPrototypeOf: 
3. `__proto__`: 
    1. Available for all objects. 
    2. Links to prototype dynamically. 
    3. Never modify it. 
    `__proto__` is the actual object that is used in the lookup chain to resolve methods, etc. prototype is the object that is used to build _`__proto__` when you create an object with new:


```
function User(name) {
    this.name = name;
}
var user1 = new User("User 1");
var user2 = new User("User 2");

// prototype is not available in objects but in their contructor
user1.__proto__ === User.prototype 
user1.__proto__ === user1.__proto__.constructor.prototype
user1.prototype === undefined

User.prototype.ago = 20;
user1.age; // 20

User.prototype is found in the proto chain of the user1 object
Object.getPrototypeOf(user1) === User.prototype // true 
```

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

https://stackoverflow.com/questions/650764/how-does-proto-differ-from-constructor-prototype

<a id="Closure"></a>  
## Closure

1. Whenever you use function inside another function, a closure is used.
2. A closure in JavaScript is like keeping all the local variables stay alive in the status as they were even after the function is exited.

用闭包模拟私有方法，提供了许多与面向对象编程相关的好处，特别是数据隐藏和封装。


#### Example 1
```
function name() {
  console.log(alice);
  var alice = "Alice";
}
name(); // undefined becasue of hoisting
```
```
function name() {
    var say = function() { console.log(alice); }
    var alice = "Alice";
    return say;
}
name()(); // Alice
```
Closure keeps a stack-frame in memory when the outer function exits: so we can still access local variables of name() from say(). 

#### Example 2

在一个闭包内对变量的修改，不会影响到另外一个闭包中的变量。

```
var makeCounter = function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }  
};

var Counter1 = makeCounter();
var Counter2 = makeCounter();
console.log(Counter1.value()); /* logs 0 */
Counter1.increment();
Counter1.increment();
console.log(Counter1.value()); /* logs 2 */
Counter1.decrement();
console.log(Counter1.value()); /* logs 1 */
console.log(Counter2.value()); /* logs 0 */
```

```
var gLogNumber, gIncreaseNumber, gSetNumber;
function setupSomeGlobals() {
  var num = 42;
  // Store some references to functions as global variables
  gLogNumber = function() { console.log(num); }
  gIncreaseNumber = function() { num++; }
  gSetNumber = function(x) { num = x; }
}

setupSomeGlobals();
gIncreaseNumber();
gLogNumber(); // 43
gSetNumber(5);
gLogNumber(); // 5

var oldLog = gLogNumber;

setupSomeGlobals();
gLogNumber(); // 42, refer to a new closure

oldLog() // 5
```

#### Example 3
```
var getI;
function buildList(list) {
    var result = [];
    for (var i = 0; i < list.length; i++) {
        var item = 'item' + i;
        result.push( function() { console.log(item + ' ' + list[i])} );
        console.log(item) // item0, item1, item2, ...
    }
    getI = function() { console.log(i); }
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

#### Example 4
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


Immediately-Invoked-Function-Expressions
<a id="ModulePattern"></a>  
## Module Pattern
1. Module, aka Immediately-Invoked-Function-Expressions, used to mimic the Class behavior that contains private elements.
```
(function () {
  // code
})();
```
```
var Module = (function () {
  return {
    publicMethod: function () {
      // code
    }
  };
})();

Module.publicMethod();
```


<a id="ErrorHandling"></a>  
## Error Handling
1. Use `throw new Error("Error");` rather than `throw "Error";`, the latter may not contain message in error object in some browsers

2. `throw` anything from string, number, bool, object. These are all valid: 
    ```
    throw "An error has occurred";
    throw 6;
    throw true;
    throw new Date();
    ```  
    However, not all browsers handle it well. So it's better to stick with `new Error(whatever)`  

3. Throw a generic Error object: `throw {name: "Major failure", message: "Detected in ..."}` 

### Handle different types of errors

| Error type              | Error detail            |
| ----------------------- | ------------------------| 
Error | base type for all errors. Never actually thrown by the engine.
EvalError | thrown when an error occurs during execution of code via eval()
RangeError | thrown when a number is outside the bounds of its range. For example, trying to create an array with -20 items (new Array(-20)). These occur rarely during normal execution.
ReferenceError | thrown when an object is expected but not available, for instance, trying to call a method on a null reference.|
SyntaxError | thrown when the code passed into eval() has a syntax error.
TypeError | thrown when a variable is of an unexpected type. For example, new 10 or "prop" in true.|
URIError | thrown when an incorrectly formatted URI string is passed into encodeURI, encodeURIComponent, decodeURI, or decodeURIComponent.


```
try {
  throw new Date(); // not an Error type
} catch(err) {
  console.log(err);
  if (err instanceof Date) {
	console.log(err.message); 
  } else if (err instanceof ReferenceError){
    //handle the error
  } else if (err instanceof TypeError){
    //handle the error
  } else {
    //handle all others
}
```
### Create your own Error type
```
function MyError(message){
    this.message = message;
}
MyError.prototype = new Error();

try {
  throw new MyError("error message");
} catch(err) {
  console.log(err.name); // Error
  console.log(err.message); // error message
}
```

#### References
1. [The Error object and throwing your own errors](http://www.javascriptkit.com/javatutors/trycatch2.shtml)
2. [The art of throwing JavaScript errors](https://www.nczonline.net/blog/2009/03/10/the-art-of-throwing-javascript-errors-part-2/)


## Regex
Using a regular expression literal, which consists of a pattern enclosed between slashes, as follows:
```
var re = /exp/g;   // same as new RegExp('exp', 'g');
```

i:	Perform case-insensitive matching  
g:	Perform a global match (find all matches rather than stopping after the first match) m: Perform multiline matching  
```
var n = str.search(/mypattern/i);
var n = str.search(/mypattern/i);

/e/.test("The best things in life are free!"); // return true
/e/.exec("The best things in life are free!"); // return match text if found, or null if not
```

## CSS
1. All children: X Y {color:blue;}
2. All direct children: X > Y {color:blue;}
3. Immediate sibling: X + Y {color:blue;}
4. All preceding sibling: X ~ Y {color:blue;}
