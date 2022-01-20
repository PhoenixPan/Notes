# JavaScript

## Convention  
1. Unlike HTML and CSS, we encourage space around operations(= + - * / );  
2. Avoid global variable and always declare local variable, otherwise it will become global variables;  
3. Declare all global variables at the beginning of a script, including loop index;  

## The order of importing scripts matter!
Load js at the end of the body to make sure its success. You can also use jQuery.  If "client" uses functions from "mqttws31", import it later. Otherwise an "class not defined" error will raise.  
```
<script type="text/javascript" src="mqttws31.js"></script>
<script type="text/javascript" language="javascript" src="client.js"></script>
```

## How to implement
1. JavaScript, both internal and external ones, can be placed within `<head>` or `<body>` or both of them;   
2. Use external .js file `<script src="myScript.js"></script>`   
2. `<script type="text/javascript">` is no longer required, as .js is the default scripting language in HTML;    
3. Advantage of separate file: clean and easy to maintain, cached JavaScript files can speed up page loads.  

## Output  
1. window.alert()：writing into an alert box  
2. document.write(): writing into the HTML output, will delete all existing HTML, should only be used for testing  
3. innerHTML: writing into an HTML element  
4. console.log(): writing into the browser console  
5. clear(): clear the console

## Syntax  
1. You can use both single or double quote, same as in HTML;  
2. Ending 0s will be omitted: 10.50 ---display--> 10.5;  

## Operations  
1. === - equal value and equal type   
2. !== - not equal value or not equal type  
3. ? - ternary operator  
4. innerHTML = typeof "John"; - Returns the type of a variable  
5. instanceof - Returns true if an object is an instance of an object type  
6. Exponentiation: 10 ** 2  
  
Examples:
```
var x = 5;
x === "5"; // false
x == "5";  // true
```
```
var x = null;
x === undefined; // false
x == undefined;  // true
```
```
true == "1";       // true
false == 0;        // true
null == undefined; // true
NaN == NaN;        // false
```

### Operate string with number
A string will be recognized as a number as long as it's not in PLUS operation and it consisits only numeric values  
```
//Same as Java
"a" + 1 + 2 // a12
1 + 2 + "a" // 3a

//Different from Java
1 - "2" // -1        
"2" - "1" // 1
"100" / 10 // 10
"10" * "10" // 100

```
### Automatic transform
```
var x = 5; // var will be treated as a number:6,7,8...
// var x = prompt("Give me a number");  //var will be treated as string: 51,511,5111...
function test() {
  x += 1;
  document.getElementById("text").innerHTML=x;
}
```

## Variable
1. objects and functions are also variables
2. Declared but not initialized variables `var num1;` (unassigned variables) will give "undefined" value and type:

	```
	var num1 = 1, num2;  
	document.getElementById("demo").innerHTML =num2 + "<br>" + (num1 + num2)+ "<br>" + typeof (num1 + num2);
	```
	These give: 
	undefined  
	NaN  
	number  

3. null is explicit empty. `var person = null` The value of person is null but typeof person is object, which is considered a bug
4. Empty an var / object: Both null and undefined can be used to empty an var or object  

	```
	typeof undefined             // undefined
	typeof null                  // object
	null === undefined           // false
	null == undefined            // true
	```
5. Hoisting: Variable declarations are processed before any code is executed, declaring a variable anywhere in the code is equivalent to declaring it at the top: (This does not happen in Java, where order of variable declarations matter)

	Example 1:  
	```
	var x = 5;

	(function () {
	    console.log(x);
	    var x = 10;
	    console.log(x); 
	})();
	// What's the result? 5 and 10?
	// Give undefined and 10 actually, since declaration of x jumpts to the top
	```
	is equivalent to
	```
	var x = 5;

	(function () {
	    var x;
	    console.log(x);
	    x = 10;
	    console.log(x); 
	})();
	```
	
	Example 2:  
	```
	var x = y, y = 'A';
	console.log(x + y); // undefinedA
	```
	```
	var x = y, y = 'A';
	console.log(y);     
	console.log(x + y); // AA
	```
	
6. You can re-declare a variable. If you don't assign a new value, it will keep the old one:  
  
	```
	var carName = "Volvo";
	var carName;  // still "Vovlo"
	```
7. Dynamic types:

	```
	var x;               // Now x is undefined
	var x = 5;           // Now x is a Number
	var x = "John";      // Now x is a String
	```
8. Shorthand statement:

	```
	var person = "John Doe", carName = "Volvo", price = 200;
	// is equal to
	var person = "John Doe", 
	carName = "Volvo", 
	price = 200;
	```

### Variable Scope  
1. Variables declared within a JavaScript function, become LOCAL to the function. A variable declared outside a function, becomes GLOBAL. Variable assigned within a function that has not been declared becomes GLOBAL:  

	```
	var numOfWheels = 4; // global
	function myFunction() {
	    carName = "Volvo";        // global
	    var carName = "Volvo";    // local
	}
	console.log("Outer:" + carName); // not defined, override
	```

2. Lifetime: Local variables are deleted when the function is completed; Global variables are deleted when you close the page
3. The global scope is the window object, all global variables belong to it: `window.numOfWheels`
4. Define a same-name local variable will temporarily override global variable

	```
	var text = "X";
	function test() {
		var text = "Y";
		console.log(text);  // give Y
	}
	console.log(text); // give X
	```

5. Lexical Scope or Static Scope: a function within another function, the inner function has access to the scope in the outer function. It does not work backwards, meaning local variables in children functions cannot be accessed in parent functions  
6. Closure: The closure concept we’ve used here makes our scope inside sayHello inaccessible to the public scope. 

	```
	var sayHello = function (name) {
	  var text = 'Hello, ' + name;
	  return function () {
	    console.log(text);
	  };
	};
	sayHello('Todd'); // do nothing
	
	var helloTodd = sayHello('Todd');
	helloTodd();  // this will do
	```
7. Private method: used to protect global namespace and scope from polluted by unnecessary functions:

	```
	var Module = (function () {
	  var privateMethod = function () {

	  };
	  return {
	    publicMethod: function () {

	    }
	  };
	})();
	```
More about code structure - second half of: https://toddmotto.com/everything-you-wanted-to-know-about-javascript-scope/  
Examples: http://stackoverflow.com/questions/500431/what-is-the-scope-of-variables-in-javascript  

### let 
1. 'let' allows you to limit the scope to the block, statement, or expression on which it is used, unlike `var`, which defines variables for at least the scope of a function

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
2. `let` does not create a property on the global object

	```
	var x = 'global';
	let y = 'global';
	console.log(this.x); // global
	console.log(this.y); // undefined
	```
3. 'let' cannot declared multiple times, it will raise syntax error. 'seitch' is considered as one block, there should be no duplicated 'let' in it.

	```
	let a = 5;
	let a = 6;
	```
4. Though 'let' will hoist, it will be in a "dead zone", so referring it before declaration will cause ReferenceError


### this
1. By default this refers to the outer most global object, the window

	```
	var myFunction = function () {
	  console.log(this); // this = window
	};
	myFunction();

	var myObject = {};
	myObject.myMethod = function () {
	  console.log(this); // this = myObject
	};

	var nav = document.querySelector('.nav');
	var toggleNav = function () {
	  console.log(this); // this = <nav> element
	  setTimeout(function () {
	    console.log(this); // this = window
	  }, 1000);
	};
	
	var nav = document.querySelector('.nav');
	var toggleNav = function () {
	  var save = this;
	  console.log(save); // <nav> element
	  setTimeout(function () {
	    console.log(save); // <nav> element
	  }, 1000);
	};
	```

## Function
1. Two ways to define functions
	```
	// function declaration
	function myFunction(str) {
		return null;
	}
	
	// function express
	var myFunction = function() {
		return null;
	}
	```
2. Accessing a function without () will return the function definition(function itself)
3. You can call function within the script
4. Pass multiple argument

	```
	function greet(name1, name2) {
		console.log("Hello " + name1 + " and " + name2 + "!");
	}

	// greet("Johns","David");
	```
5. No executions after return, though you can leave your codes there(not good anyway)

##### setInterval
1. The number returned is the progress number, we could use "clearInterval(n)" to terminate it
2. Use anonymous function 

	```
	setInterval(function(){}, time)
	```
3. Example: increment. If you pass the i into the function, it will forever be 2, as you always pass 1 into it
	```
	var i = 1;

	setInterval(increment, 1000);

	function increment(){
		console.log(i);
	    i = i % 360 + 1;
	}
	```

## Array 
1. It is allowed to skip some index: array[1000] = "Test"; what's in between will be "undefined".
2. Most useful methods
```
array.push("newElement);
var lastElement = array.pop();

// to add/remove element from the beginning, use unshift()/shift()

array.indexOf("where");
var newArray = array.slice(1,3);

// Splice: remove
var removed = array.splice(index, numberOfElementsToRemove,( newItemsToBeAdded));

var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 1, "Lemon", "Kiwi");
// Banana,Orange,Lemon,Kiwi,Mango

```

## forEach
```
array.forEach(functionForEveryElement);

// Example
colorArray.forEach(printColors);

// Example: anonmyous function
colorArray.forEach(function(color) {
  console.log(color);
});
```

## Object
1. Values pairs are called properties;  
2. JavaScript objects cannot be compared, (x == y) is always false ;  
3. JavaScript treats primitive values as objects when executing methods and properties;  
4. Change properties in two ways:  

  ```
  objectName["propertyName"] // not valid if the name starts with a number
  objectName.propertyName
  ```
5. Methods are build-in functions:  

  ```
  var person = {
      firstName: "John",
      lastName : "Doe",
      id       : 5566,
      fullName : function() {
         return this.firstName + " " + this.lastName;
      }
  };
  person.fullName();
  ```
6. primitive objects

  ```
  var x = 500;             
  var y = new Number(500);

  // (x == y) is true because x and y have equal values
  // (x === y) is false because x and y have different types
  ```

## HTML events
onchange:	An HTML element has been changed  
onclick:	The user clicks an HTML element  
onmouseover:	The user moves the mouse over an HTML element  
onmouseout:	The user moves the mouse away from an HTML element  
onkeydown:	The user pushes a keyboard key  
onload:	The browser has finished loading the page  
More on: http://www.w3schools.com/jsref/dom_obj_event.asp  

## String 
##### Escape
```
var y = "We are the so-called \"Vikings\" from the north."
```
\n seems to work as new line, but it only renders one space in html. Use "<br>" to be effective.  
\r:	carriage return   
\t:	tab  
\b:	backspace  
\f:	form feed  

##### break a long string 
With single \: not safe everywhere
```
document.getElementById("demo").innerHTML = "Hello \
  Dolly!";
```
Actually break it is the safest way
```
document.getElementById("demo").innerHTML = "Hello" + 
"Dolly!";
```

##### Don't create string objects, it's slow, confusing, and unpredictable.

## String methods  
Complete reference: http://www.w3schools.com/jsref/jsref_obj_string.asp
```
var num = str.length;  
var num = str.indexOf("yes");  
var num = str.lastIndexOf("a");  
var num = str.search("a");        // equal to indexOf() but more powerful wth regexp
  
var str = str.slice(start, end)          // accept negative values, not in IE8 or earlier  
var str = str.slice(start)               // to the end
var str = str.substring(start, end)      // don't accept negative values  
var str = str.substring(start)           // to the end
var str = str.substr(start, length)      // start can be negative  

var str = str.replace("Microsoft","mypattern");  // replace only the first, accept regular expression
var str = str.replace(/Microsoft/g,"mypattern"); // add a g tag to replace all

var str = str.toUpperCase(); 
var str = str.toLowerCase(); 

var str = str1.concat(str2);   // same as + 
var str = str1.concat("something else",str2); 

var chr = str.charAt(0); 
var num = str.charCodeAt(0); 
```
1. Strings are immutable  
2. Don't access string as an array. It does not work in all browsers, it's confusing,.
```
var str = "HELLO WORLD";
str[0];
```
convert the string to array first:
```
var str = "a,b,c,d,e,f";
var arr = str.split();       // put the entire string in arr[0]
//var arr = str.split("");   // Split in characters
document.getElementById("demo").innerHTML = arr[0];
```

## Number
Complete reference: http://www.w3schools.com/jsref/jsref_obj_number.asp  
1. JS numbers are always 64-bits floating point;  
2. Integers are considered accurate up to 15 digits: var y = 9999999999999999; // y = 10000000000000000;  
3. The maximum number of decimals is 17, but the floating point is not always accurate: var x = 0.2 + 0.1; // x = 0.30000000000000004;   
4. To ensure an accurate decimal, to multiply and divide: var x = (0.2 * 10 + 0.1 * 10) / 10;  
5. If a number goes outside of the largest possible value, it becomes "Infinity": while (num != -Infinity);   
6. Divide by 0 will also give Infinity: var y = -2 / 0; (// y = -Infinity;)  
7. Infinity is a number type;  

### Scientific notation
```
var y = 123e5;      // 12300000
var z = 123e-5;     // 0.00123
```
### NaN - Not a Number
Type is number. 
```
// operate with non-numeric string
var x = 100 / "Apple";

// operate a number with a NaN
var x = NaN, y = 5;
var z = x + y;   

// operate a number with an undefined value
var x, y = 5;
var z = x + y;         

// operate two non-numeric values
"Hello" - "Dolly" // NaN

// Difference: operate a string with a NaN
var x = NaN, var y = "5";
var z = x + y;  // z = NAN5
```

### Number methods
```
// Return a STRING
(1 + 1).toString();
(1).toExponential();  // 3.000000e+0
(5.555).toFixed(2);   // 5.56  perfect for working with money
(1923.656).toPrecision(2)  // 1.9e+3   only display these many units

// Return a NUMBER
(123).valueOf(); // used internally to convert number objects to primitive values. Not in your code. 

// Convert variables to number
innerHTML = Number("10");     // 10
innerHTML = Number("10 10");  // NaN
innerHTML = parseInt("10")    // 10
innerHTML = parseInt("10.33 10")  // 10
innerHTML = parseInt("year 10")   // NaN
innerHTML = parseFloat("10.33")   // 10.33
```
### Number properties  
Number.MAX_VALUE:	Returns the largest number possible in JavaScript  
Number.MIN_VALUE:	Returns the smallest number possible in JavaScript  
Number.NEGATIVE_INFINITY:	Represents negative infinity (returned on overflow)  
Number.NaN:	Represents a "Not-a-Number" value  
Number.POSITIVE_INFINITY:	Represents infinity (returned on overflow)  

## DOM
1. DOM is a programming interface for HTML, XML and SVG documents which provides a structured representation of the document as a tree. 
2. DOM is not a part of the JavaScript language. It can also be accessed by other languages.

### Select elements  

1. Direct reference:

	```
	document.URL
	document.body
	document.head
	document.links // all anchor tags
	```

2. document.querySelector: Use CSS style selector to perform all functions that "getElement" has. It only works when your DOM is ready. If it returns null, try to place all JS script at bottom of the page.

	```
	document.querySelector("h1");     // select the first element that fits.
	document.querySelectorAll("h1");  // select all elements that fit
	```

3. document.getElement: get element by difference references

	```
	document.getElementById(id)  
	document.getElementsByTagName(name)  // return a node list (a light-weight array-like) of all elements with this tag
	document.getElementsByClassName(name)  // return a node list of elements
	```

4. Adding and Deleting Elements  

	```
	document.createElement(element)  
	document.removeChild(element)  
	```

### Manipulate style
1. Simple way:

	```
	element.style.color = "red";
	element.style.border = "1px solid blue";
	```

2. More decent way: Instead of manipulating sytles directly using "element.style.color", we could put the new style in a CSS class and add the class to the element using JS

	```
	tag.classList.add("myclass");
	tag.classList.remove("myclass");
	tag.classList.toggle("myclass");  // turn on/off a class depends on the current status
	```
	
### Manipulate text
1. textContent: obtain the plain text from this element  

	```
	element.textContent = "I <strong>like</strong> it"; // display "I <strong>like</strong> it"
	```
2. innerHTML: obtain the text as well as the tags inside it
	```
	element.innerHTML = "I <strong>like</strong> it";  // display "I like it"
	```

### Manipulate attributes

```
element.getAttribute("href");
element.setAttribute("href", "http://www.mysite.com");
```

### Events
1. addEventListener: 

	```
	element.addEventListener(eventType, functionToCall);
	```
2. Trigger an input without clicking:eventType = "change"


## Date()
getDate()	Get the day as a number (1-31)  
getDay()	Get the weekday a number (0-6)  
getFullYear()	Get the four digit year (yyyy)  
getHours()	Get the hour (0-23)  
getMilliseconds()	Get the milliseconds (0-999)  
getMinutes()	Get the minutes (0-59)  
getMonth()	Get the month (0-11)  
getSeconds()	Get the seconds (0-59)  
getTime()	Get the time (milliseconds since January 1, 1970)  

## RegExp
/pattern/modifiers;  
```
var n = str.search(/mypattern/i);
var n = str.search(/mypattern/i);

/e/.test("The best things in life are free!"); // return true
/e/.exec("The best things in life are free!"); // return match text if found, or null if not
```
i:	Perform case-insensitive matching
g:	Perform a global match (find all matches rather than stopping after the first match)
m:	Perform multiline matching

```
str.replace(/"target"/flag, "newchar");
```

## Get input or uploaded files
[My solution](https://github.com/PhoenixPan/WebDevelopment/blob/master/Widget/upload-file.html) that includes both input upload and drag-and-drop.   
References: https://www.html5rocks.com/en/tutorials/file/dndfiles/

## Read files
### csv
File & FileReader
```
var f = new File([""], "data1.csv");

var reader = new FileReader();
reader.onload = function(e) {
  var text = reader.result;
  console.log(text);
}

reader.readAsText(f);
```

### http request
```
readTextFile("/custom/acfi/acfi1_adl.csv");

function readTextFile(file) {
    var rawFile = new XMLHttpRequest();
    rawFile.open("GET", file, false);
    rawFile.onreadystatechange = function ()
    {
        if(rawFile.readyState === 4)
        {
            if(rawFile.status === 200 || rawFile.status == 0)
            {
                var allText = rawFile.responseText;
                console.log(allText);
            }
        }
    }
    rawFile.send(null);
}
```

### jQuery ajax
```
$(document).ready(function() {
    $.ajax({
        type: "GET",
        url: "acfi1_adl.csv",
        dataType: "text",
        success: function(data) {processData(data);}
     });
});

function processData(allText) {
    var allTextLines = allText.split(/\r\n|\n/);
    var headers = allTextLines[0].split(',');
    var lines = [];

    for (var i=1; i<allTextLines.length; i++) {
      console.log(escape(allTextLines[i]));
}
```

## Check false value
```
if (!value) {
    // ...
}
```
The six falsy values of JavaScript are  
1. undefined
2. null
3. false
4. the empty string
5. 0 (of type number)
6. NaN.

## Difference between innerHTML and outerHTML
innerHTML: give only the contents between the tags;  
outerHTML: give the entire html element including the tags.  

## Use setTimeout to replace setInterval
http://stackoverflow.com/questions/6685396/execute-the-setinterval-function-without-delay-the-first-time

1. It could have immediate first call
2. No need for expelicit stop (clearInterval)

```
function countdownDate() {
    // Get todays date and time
    var countDownDate = new Date("May 29, 2017 17:00:00").getTime();
    var now = new Date().getTime();

    // Find the distance between now an the count down date
    var distance = countDownDate - now;

    // Time calculations for days, hours, minutes and seconds
    var days = Math.floor(distance / (1000 * 60 * 60 * 24));
    var hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

    // Display the result in the element with id="demo"
    var countdownElement = $(".countdown");
    countdownElement.html("Time left: " + days + " Days " + hours + " Hours");

    // If the count down is finished, write some text
    if (distance < 0) {
        countdownElement.html("Event expired, thank you for your participation!");
        $("#page-home .main-container .fmc-style-btn").addClass("ui-state-disabled");
    }
    setTimeout(countdownDate, 60000);
}
```

## Print a div
1. Print in a new window:
```
    var prtContent = document.getClassesByName(".acfi-main-table")[0];
    var WinPrint = window.open('', '', 'left=0,top=0,width=800,height=900,toolbar=0,scrollbars=0,status=0');
    WinPrint.document.write('<head><title>ACFI Calculator</title>');
    WinPrint.document.write('<style type="text/css"> * {border: 1px solid black;}</style>');
    WinPrint.document.write('</head><body>');
    WinPrint.document.write(prtContent.outerHTML);
    WinPrint.document.write('</body>');
    WinPrint.document.close();
    WinPrint.focus();
    WinPrint.print();
    WinPrint.close();
```


## Mistakes
true, because x is now 10, and 10 is true
```
var x = 0;
if (x = 10)
```
