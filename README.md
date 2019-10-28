# Javascript Callback, Promise and async/await

## Synchronous and Asynchronous

Synchronous Call | Asynchronous Call
------------ | -------------
A callback in which the code execution waits for an event before continuing. | A callback that does not block the execution of the program.
Code execution waits for the event before continuing. | Do not block the program from the code execution.
Programmer can use synchronous callbacks when it is necessary to execute tasks in a sequence and when it does not require much time for execution. | Programmer can use asynchronous callbacks when the tasks are not dependent on each other and when it takes time for execution.

## Callback

JavaScript is synchronous by default, and is single threaded. This means that code cannot create new threads and run in parallel. Find out what asynchronous code means and how it looks like

You can’t know when a user is going to click a button, so what you do is, you define an event handler for the click event. This event handler accepts a function, which will be called when the event is triggered

```javascript
document.getElementById('button').addEventListener('click', () => {
  //item clicked
})
```

A callback is a simple function that’s passed as a value to another function, and will only be executed when the event happens. We can do this because JavaScript has first-class functions, which can be assigned to variables and passed around to other functions (called higher-order functions)

It’s common to wrap all your client code in a load event listener on the window object, which runs the callback function only when the page is ready:

This is the so-called callback.

```javascript
window.addEventListener('load', () => {
  //window loaded
  //do what you want
})
```

Callbacks are used everywhere, not just in DOM events.

One common example is by using timers:

```javascript
setTimeout(() => {
  // runs after 2 seconds
}, 2000)
```

**Question:** Callback is synchronous or asynchronous.
**Answer:** 

## Promise

<ul>
	<li>The <b>Promise</b> object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.</li>
	<li>A promise is commonly defined as a proxy for a value that will eventually become available.</li>
	<li>Promises are one way to deal with asynchronous code, without getting stuck in callback hell.</li>
	<li>Promises have been part of the language for years (standardized and introduced in ES2015), and have recently become more integrated, with async and await in ES2017.</li>
	<li>Async functions use promises behind the scenes, so understanding how promises work is fundamental to understanding how async and await work.</li>
	<li><b>Promises are eager</b>, meaning that a promise will start doing whatever task you give it as soon as the promise constructor is invoked. If you need lazy, check out observables or tasks.</li>
	<li>A Promise is in one of these states:
		<ul>
			<li><b>pending</b>: initial state, neither fulfilled nor rejected.</li>
			<li><b>fulfilled</b>: meaning that the operation completed successfully.</li>
			<li><b>rejected</b>: meaning that the operation failed.</li>
		</ul>
	</li>
</ul>

**Question** Which JS APIs use promises?
In addition to your own code and libraries code, promises are used by standard modern Web APIs such as:

- the Battery API
- the Fetch API
- Service Workers

It's unlikely that in modern JavaScript you'll find yourself not using promises, so let's start diving right into them.

A **Simple** way to create a promise:

```javascript
Promise.resolve(2)
```

```javascript
let someObject={ a: 1 }
Promise.resolve(someObject)
```

```javascript
var promise1 = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('foo');
  }, 300);
});

promise1.then(function(value) {
  console.log(value);
  // expected output: "foo"
});

console.log(promise1);
// expected output: [object Promise]
```

As the Promise.prototype.then() and Promise.prototype.catch() methods return promises, they can be chained.

```javascript
var promise = 'fail';
var cherryPromise = new Promise(function(resolve,reject){
	if(promise == 'success'){
		resolve("Resolved");
	} else if(promise == 'fail'){
		reject('Rejected');
	} else {
		console.log('XYZ');
	}
});
cherryPromise.then(function(data){
	console.log(data);
}).catch(function(data){
	console.log(data);
}).finally(function(){
	console.log('Finally');
});

// Rejected
// Finally
// If promise is resolved so the output is Resolved and when the promise is fail
// It return rejected and in case of nothing it will be pending
```

```javascript
new Promise(function(resolve, reject) {
  setTimeout(() => resolve(2), 1000);
}).then(function(result) {
  console.log(result); // 2
  return result * 2;
}).then(function(result) {
  console.log(result); // 4
  return result * 3;
}).then(function(result) {
  console.log(result); // 12
  return result * 4;
});

// 2
// 4
// 12
```

### Promise Properties

**Promise.length**
Length property whose value is always 1 (number of constructor arguments).

**Promise.prototype**
Represents the prototype for the Promise constructor.

### Promise Methods

**Promise.all(iterable)**
Wait for all promises to be resolved, or for any to be rejected.
If the returned promise resolves, it is resolved with an aggregating array of the values from the resolved promises in the same order as defined in the iterable of multiple promises. If it rejects, it is rejected with the reason from the first promise in the iterable that was rejected.

```javascript
var promise1 = Promise.resolve(3);
var promise2 = 42;
var promise3 = new Promise(function(resolve, reject) {
  setTimeout(resolve, 100, 'foo');
});

Promise.all([promise1, promise2, promise3]).then(function(values) {
  console.log(values);
});
// expected output: Array [3, 42, "foo"]
```

**Promise.allSettled(iterable)**
Wait until all promises have settled (each may resolve, or reject).
Returns a promise that resolves after all of the given promises have either resolved or rejected, with an array of objects that each describe the outcome of each promise.

```javascript
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 100, 'foo'));
const promises = [promise1, promise2];

Promise.allSettled(promises).
  then((results) => results.forEach((result) => console.log(result.status)));

// expected output:
// "fulfilled"
// "rejected"
```

**Promise.race(iterable)**
Wait until any of the promises is resolved or rejected.
If the returned promise resolves, it is resolved with the value of the first promise in the iterable that resolved. If it rejects, it is rejected with the reason from the first promise that was rejected.

```javascript
var promise1 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 500, 'one');
});

var promise2 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 100, 'two');
});

Promise.race([promise1, promise2]).then(function(value) {
  console.log(value);
  // Both resolve, but promise2 is faster
});
// expected output: "two"
```

**Promise.reject(reason)**
Returns a new Promise object that is rejected with the given reason.

```javascript
function resolved(result) {
  console.log('Resolved');
}

function rejected(result) {
  console.error(result);
}

Promise.reject(new Error('fail')).then(resolved, rejected);
// expected output: Error: fail
```

**Promise.resolve(value)**
Returns a new Promise object that is resolved with the given value. If the value is a thenable (i.e. has a then method), the returned promise will "follow" that thenable, adopting its eventual state; otherwise the returned promise will be fulfilled with the value. Generally, if you don't know if a value is a promise or not, Promise.resolve(value) it instead and work with the return value as a promise.

```javascript
var promise1 = Promise.resolve(123);
promise1.then(function(value) {
  console.log(value);
  // expected output: 123
});
```

### Promise Prototype

**Promise.prototype.catch()**
Appends a rejection handler callback to the promise, and returns a new promise resolving to the return value of the callback if it is called, or to its original fulfillment value if the promise is instead fulfilled.

```javascript
var promise1 = new Promise(function(resolve, reject) {
  throw 'Uh-oh!';
});

promise1.catch(function(error) {
  console.error(error);
});
```

**Promise.prototype.then()**
Appends fulfillment and rejection handlers to the promise, and returns a new promise resolving to the return value of the called handler, or to its original settled value if the promise was not handled (i.e. if the relevant handler onFulfilled or onRejected is not a function).

**Promise.prototype.finally()**
Appends a handler to the promise, and returns a new promise which is resolved when the original promise is resolved. The handler is called when the promise is settled, whether fulfilled or rejected.

```javascript
let isLoading = true;

fetch(myRequest).then(function(response) {
    var contentType = response.headers.get("content-type");
    if(contentType && contentType.includes("application/json")) {
      return response.json();
    }
    throw new TypeError("Oops, we haven't got JSON!");
  })
  .then(function(json) { /* process your JSON further */ })
  .catch(function(error) { console.error(error); /* this line can also throw, e.g. when console = {} */ })
  .finally(function() { isLoading = false; });
```
**Random example on promise**

```javascript
// A asyncronous Promise is created with 'new Promise( (resolve, reject) => {...} )'

// resolve() and reject() are how we stop the promise execution. 
// Note: Promise execution does not stop at the last line of the handler function. only reject() or resolve() do that.

// A Promise is resolved only once, regardless of how many then() handlers
let unresolvedPromise = new Promise( 
  (resolve, reject) => {
    setTimeout(function () { resolve(3) }, 3000)
  }
)

unresolvedPromise.then(innerValue => console.log('resolved with: ', innerValue)) 
// the then() handler is called when the promise is resolved

console.log('unresolvedPromise state is: ', unresolvedPromise);
console.log('LAST LINE IN THIS FILE');
```

```javascript
// whether the value will be there in future, or it’s already known, I just call `then()` in either case
// the `then()` handler is always called asyncronously [i.e.: behaves like using setTimeout(..., 0)]

let resolvedPromise = Promise.resolve('someValue')

let unresolvedPromise = new Promise( 
  (resolve, reject) => {
    setTimeout(function () { resolve(3) }, 3000)
  }
)

unresolvedPromise.then(innerValue => console.log('unresolvedPromise FIRST then()', innerValue)) // the then() handler is called when the promise is resolved
unresolvedPromise.then(innerValue => console.log('unresolvedPromise SECOND then()', unresolvedPromise))
resolvedPromise.then(innerValue => console.log('resolvedPromise\'s then()', innerValue))

console.log('unresolvedPromise state is: ', unresolvedPromise)
console.log('LAST LINE IN THIS FILE')
```

```javascript
// `Promise.all()` creates a Promise that is resolved when all promises are resolved

let promise1 = Promise.reject('Reject');

let promise2 = Promise.resolve('Resolve');

Promise.all([promise1, promise2])
  .then(done => console.log('Done', done))
  .catch((err)=>{console.log("ERROR",err)})

console.log('LAST LINE IN THIS FILE');
```

**References**
- https://nodejs.dev/understanding-javascript-promises
- https://www.vojtechruzicka.com/javascript-async-await
- http://javascript.info/promise-basics
- http://javascript.info/async-await
- https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://javascript.info/promise-chaining
- https://developer.mozilla.org/he/docs/Web/JavaScript/Reference/Global_Objects/Promise

## async/await
