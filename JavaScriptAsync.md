- [Promise](#Promise)
- [await](#await)

<a id="Promise"></a>  
## Promise
### Why do we use Promise?
1. Enable easier async
2. Avoid callback hall, flatten with `.then()`
3. Get back the control

### Basics
1. `catch(reject)` is a syntax suger for `then(undefined, reject)`

#### Basic example  
```
var promiseCount = 0;
function testPromise() {
    let thisPromiseCount = ++promiseCount;
    console.log("Sync starts: " + thisPromiseCount)

    let myPromise = new Promise((resolve, reject) => {
            console.log("Async starts: " + thisPromiseCount);
            window.setTimeout(function() { resolve(thisPromiseCount);}, 1000);
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

    console.log("Sync ends: " + thisPromiseCount)
}

testPromise();
```

Abbreviated promise: could you still tell which one is which?  
```
var testPromise = new Promise(res => res(1));
testPromise.then(value => value * 2).then(v => console.log(v));
```

### Promise chaining
1. It's recommended to end all promise chain with `catch()`

#### Basic example
```
var wait = time => new Promise(
  res => setTimeout(() => res(), time)
);

wait(200)
  .then(() => new Promise(res => res('foo')))   // onFulfilled() can return a new promise, `x`
  .then(a => a) // the next promise will assume the state of `x`
  // Above we returned the unwrapped value of `x`
  // so `.then()` above returns a fulfilled promise with that value:
  .then(b => console.log(b)) // 'foo'
  // Note that `null` is a valid promise value:
  .then(() => null)
  .then(c => console.log(c)) // null
  .then(() => {throw new Error('foo');}) // This error is not reported yet:
  // Instead, the returned promise is rejected with the error as the reason:
  .then(                                                                     
    d => console.log(`d: ${ d }`), // Nothing is logged here due to the error above:
    e => console.log(e)) // [Error: foo], now we handle the error (rejection reason)
  // With the previous exception handled, we can continue:
  .then(f => console.log(`f: ${ f }`)) // f: undefined
  // The following doesn't log. e was already handled, so this handler doesn't get called:
  .catch(e => console.log(e))
  .then(() => { throw new Error('bar'); })
  // When a promise is rejected, success handlers get skipped.
  // Nothing logs here because of the 'bar' exception:
  .then(g => console.log(`g: ${ g }`))
  .catch(h => console.log(h)) // [Error: bar]
;
```

### Error handling
#### Why do we need `catch()`
Bad code! error in handleSuccess will be swallowed. Use catch (then(null, error))
```
save().then(
  handleSuccess,
  handleError
);
// without catch()
```
Ok! allow different ways to handle different error
```
save().then(
    handleSuccess,
    handleNetworkError
  )
  .catch(handleProgrammerError)
;
```

#### Order of handling  
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

#### fetch
Use ES6 fetch API, which return a promise
```
 const promise = fetch(`http://www.example.com?num1=${num1}&num2=${num2}`)
        .then(x => x.json());
```

#### Promise with Ajax
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

