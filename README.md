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

### async/await
