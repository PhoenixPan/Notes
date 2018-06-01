- [Promise](#promise)
- [Generator](#generator)
- [await](#await)
- [fetch](#fetch)


<a id="promise"></a>  
# Promise
## Why do we use Promise?
1. Enable easier async
2. Avoid callback hell, flatten with `.then()`
3. Mitigrate Inversion of Control (IOC)

Promise should be treated as a blackbox, only the function responsible for creating the promise should know the promise status and have access to resolve/reject functions.

Promise constructor takes a function, which takes two parameters: `resolve()` and `reject()`. You can optionally `resolve(value)` or `reject(value)` with values, which will be passed to the callback functions attached with `then()` or `catch()` when the promise is **settled**. Promise status and its value must not change once it's settled. 

```
promise.then(
  onFulfilled?: Function,
  onRejected?: Function
) => Promise
```

The `then()` method:
1. `then()` returns a new promise
2. `catch(reject)` is a syntax suger for `then(undefined, reject)`
3. If the arguments supplied to `then()` are not functions, this `then()` will be ignored and the chain will continue with the same promise:  
  `myPromise.then(888) // => myPromise`
4. Both `onFulfilled()` and `onRejected()` are optional
5. `onFulfilled()` called if fulfilled, with the promise's value as the first argument
6. `onRejected()` called if rejected, with the reason for rejection as the first argument, use Error objects to catch

[Reference: What is Promise](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)

## Examples
1. Promise is async
    ```
    var promiseCount = 0;
    function testPromise() {
        ++promiseCount;
        console.log("Sync starts: " + promiseCount)

        let myPromise = new Promise((resolve, reject) => {
                console.log("Async starts: " + promiseCount);
                window.setTimeout(function() { resolve(promiseCount);}, 1000);
            }
        );

        myPromise
        .then(
            function(fulfillment) { 
                console.log("Async ends: " + fulfillment);
            })
        .catch(
          (reason) => {
                console.log('Handle rejected promise ('+ reason +') here.');
            });

        console.log("Sync ends: " + promiseCount)
    }

    testPromise();
    testPromise();
    ```
2. Create a promise
    ```
    var testPromise = new Promise(res => res(1));
    testPromise.then(value => value * 2).then(v => console.log(v));
    ```
    ```
    function test() {
      return Promise.resolve(1);
    }
    var testPromise = test();
    testPromise.then(value => value * 2).then(v => console.log(v));
    ```

## Promise chaining
### Examples
1. Basic chaining behavior
    ```
    var wait = time => new Promise(
      res => setTimeout(() => res(), time)
    );

    wait(200)
      .then(() => new Promise(res => res('foo')))   // return a new promise p
      .then(a => a) // this promise will assume the state of p and return a fulfilled promise with that value:
      .then(b => console.log(b)) // 'foo'
      .then(() => {throw new Error('foo');})
      // When a promise is rejected, the resolve handler gets skipped
      .then(                                                                     
        d => console.log(`d: ${ d }`), // not logged
        e => console.log(e)) // log the error: Error: foo
      // With the previous exception handled, we can continue:
      .then(f => console.log(`f: ${ f }`)) // f: undefined
      .catch(e => console.log(e)) // not logged because there's no error
      .then(() => { throw new Error('bar'); })
      .then(g => console.log(`g: ${ g }`)) // not logged because of the 'bar' error
      .catch(h => console.log(h)) // Error: bar
    ;
    ```

    ```
    wait(200)
      .then(a => 300) //swallowed
      .then(a => 500)
      .then(b => console.log(b)) // 500
      .then(c => console.log(c)) // undefined
    ```

2. `null` is a valid promise value:
    ```
    wait(200)
      .then(() => null)
      .then(n => console.log(n)) // null
    ```

3. End all promise chain with `catch()`. Why? Proper error handling.   
    In this example, error in handleSuccess will be swallowed:
    ```
    save()
    .then(
      handleSuccess, // error swallowed here
      handleError
    );
    // without catch()
    ```
    Now we allow different ways to handle different error
    ```
    save()
    .then(
        handleSuccess,
        handleNetworkError
      )
    .catch(handleProgrammerError)
    ;
    ```

## Order of error handling  
```
var mypromise = new Promise((res, rej) => {
	rej("Error1"); // catch Error1
	//res("success"); // catch Error2
});

mypromise.then(
  res => console.log(res)
).then(
  res => {throw new Error("error2");}
).catch(
  err => console.log(err)
)
```

## fetch
Use ES6 fetch API, which return a promise
```
 const promise = fetch(`http://www.example.com?num1=${num1}&num2=${num2}`)
        .then(x => x.json());
```

## Promise with Ajax
```
var getJSON = function(url) {
  var promise = new Promise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
  });

  return promise;
};

getJSON("https://httpbin.org/get").then(function(response) {
  console.log('Contents: ',response);
}, function(error) {
  console.error('Oops!', error);
});
```

<a id="generator"></a>  
# Generator
1. Return a Generator object to iterator
2. First call of `next()` executes until the first `yield` statement
  ```
  function* logGenerator() {
    console.log(0, yield);
    console.log(1, yield);
    console.log(2, yield);
  }

  var gen = logGenerator();

  gen.next('zika');
  gen.next('pretzel');
  gen.next('california');
  gen.next('mayonnaise');

  // 0 "pretzel"
  // 1 "california"
  // 2 "mayonnaise"
  ```
3. Execution ends with `return;`

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


<a id="await"></a>  
# await
1. `async functions` return promises, `await` awaits promises.

## Basic
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

## Multiple async tasks
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

<a id="fetch"></a>  
## Fetch
1. Problems:
  1. Error handling
  2. Two steps required: `response.json()`
  3. No timeout


[Fetch vs Axios http request](https://medium.com/@sahilkkrazy/fetch-vs-axios-http-request-c9afa43f804e)