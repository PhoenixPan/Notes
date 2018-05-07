
- [Promise](#Promise)

<a id="Promise"></a>  
## Promise
```
'use strict';
var promiseCount = 0;
function testPromise() {
    let thisPromiseCount = ++promiseCount;
    console.log("Sync starts: " + thisPromiseCount)

    let myPromise = new Promise((resolve, reject) => 
        {
            console.log("Async starts: " + thisPromiseCount);
            window.setTimeout(function() { resolve(thisPromiseCount);}, 1000);
        }
    );

    myPromise
    .then(
        function(fulfillment) { // fulfillment is thisPromiseCount
            console.log("Async ends: " + fulfillment);
        })
    .catch(
       (reason) => {
            console.log('Handle rejected promise ('+ reason +') here.');
        });

    console.log("Sync ends: " + thisPromiseCount)
}

testPromise();
```

<a id="await"></a>  
## await
1. `async functions` return promises, `await` awaits promises.

### Basic
```
function getPromise() {
	return new Promise((resolve, reject) => {
  		window.setTimeout(function() {
			resolve("Success");
		}, 2000);
	});
}

async function resolvePromise() {
    console.log("start resolving...");
	var result = await getPromise();
	console.log("promise resolved: " + result);
    return result;
}

// with await: result is "Success", promise resolved
// without await: result is a promise, promise also resolved
var result = await resolvePromise();
```
### Multiple async tasks
Preferred way:  
```
var [result1, result2] = await Promise.all([resolvePromise(), resolvePromise()]);
console.log("completed");
```
Unrecommended way:  
```
var promise1 = resolvePromise();
var promise2 = resolvePromise();
var result1 = promise1;
var result2 = promise2;
console.log("completed");
```

#### fetch
Use ES6 fetch API, which return a promise
```
 const promise = fetch(`http://www.example.com?num1=${num1}&num2=${num2}`)
        .then(x => x.json());
```