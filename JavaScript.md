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
Hoisting: declare at the beginning (not initializing)
let: limits to block. It is not available using "var", which only limits to functions.
for (x in person)
onmousedown, onmouseup, onclick 

Window.localStorage: across session
Window.sessionStorage: live for one session, refresh is ok, die upon closing tab
session cookies: live until the WINDOW is closed. Plain text for user, encrypted on only https. Cookies will be sent everytime a user sends a request, so try to avoid overloading it. Put authentication in cookies, as storages are not sent with the requests everytime. Plain text for user, encrypted on only https. Cookies will be sent everytime a user sends a request, so try to avoid overloading it. Put authentication in cookies, as storages are not sent with the requests everytime

```
document.querySelector()
querySelectorAll
getElementsByClassName
document.getElementById();
document.getElementsByTagName();
document.bogy;
```
```
$(".plan-projects-container").append("<div>")
```

## Regex
Using a regular expression literal, which consists of a pattern enclosed between slashes, as follows:
```
var re = /exp/g;    same as new RegExp('exp', 'g');
```

## Operation 
01 + "05" = "105"

undefined & null: 
== true
=== false

//Same as Java
"a" + 1 + 2 // a12
1 + 2 + "a" // 3

//Different from Java
1 - "2" // -1        
"2" - "1" // 1
"100" / 10 // 10
"10" * "10" // 100

Divide by 0 will also give Infinity

## Array
```
array.join(" "); // get all elements concatenated on space
```
```
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
Undeclared variables: implicit globals

Example: 
```
var a = {n:1};
  var a b = =;
a.x b  3; // equals to b = 3; (impsame results, why
a.x; // undefined
b.x; // {n: 2}
a; // {n: 2}
b; // equals a = {n:2};
// a= a.x = {n:2}; (impli// call here: b = 3, a = 3
})();
// call here: b = 3, a = undefined
```
Example: 
```
var a = {n:1};
var b = a;
a.x = a = {n:2};
// a= a.x = {n:2}; // same results, why
a.x; // undefined
b.x; // {n: 2}
a; // {n: 2}
b; //{n: 1, x: {n:2}}
```

1. a points to {n:1}
2. b points to a, same, {n:1}
3. 连等会先确定所有变量的指针，再从右向左，所以a.x有 {n:1, x:null}, b.x 相同
4. a points to {n:2}
5. a.x, {n:1, x=null}, points to {n:2} and becomes {n:1, x:{n:2}}, which b points to

const: cannot be reassigned but can be changed (array element)

## Functions
``` 
function test() { console.log("a")};
var test2 = function() {console.log("b")}; // assign anonymous 
(function() {alert('Hello World');})(); // run anonymous 
```
Function expression: var fun = function() {...};
parsed when executed, cannot be hoisted
Function declaration: function fun() {...};
parsed when encountered, can be hoisted

Immediately Invoked Function Expression (self-executing anonymous function): avoid variable hoisting
https://www.tutorialspoint.com/es6/es6_functions.htm


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
// Parenthesize the body of function to return an object literal expression:
params => ({foo: bar}) 


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

## CSS
All children: X Y {color:blue;}
All direct children: X > Y {color:blue;}
Immediate sibling: X + Y {color:blue;}
All preceding sibling: X ~ Y {color:blue;}



## Trick
function(num) {
  var value = num || 10; // if num is 0, value becomes 10 instead of 0, as 0 is evaluated "false"
}