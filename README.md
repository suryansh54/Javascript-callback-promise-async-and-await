# Javascript Callback, Promise and async/await

### Synchronous and Asynchronous

Synchronous Call | Asynchronous Call
------------ | -------------
A callback in which the code execution waits for an event before continuing. | A callback that does not block the execution of the program.
Code execution waits for the event before continuing. | Do not block the program from the code execution.
Programmer can use synchronous callbacks when it is necessary to execute tasks in a sequence and when it does not require much time for execution. | Programmer can use asynchronous callbacks when the tasks are not dependent on each other and when it takes time for execution.

### Callback

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

### Promise

<ul>
  <li>The **Promise** object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.</li>
  <li>A **Promise** is a special kind of javascript object which contains another object</li>
  <li>**Promises are eager**, meaning that a promise will start doing whatever task you give it as soon as the promise constructor is invoked. If you need lazy, check out observables or tasks.</li>
  <li>A Promise is in one of these states:
    <ul>
      <li>**pending**: initial state, neither fulfilled nor rejected.</li>
      <li>**fulfilled**: meaning that the operation completed successfully.</li>
      <li>**rejected**: meaning that the operation failed.</li>
    </ul>
  </li>
</ul>

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

### async/await
