# Javascript Callback, Promise and async/await

## Callback

JavaScript is synchronous by default, and is single threaded. This means that code cannot create new threads and run in parallel. Find out what asynchronous code means and how it looks like

You can’t know when a user is going to click a button, so what you do is, you define an event handler for the click event. This event handler accepts a function, which will be called when the event is triggered

```javascript
document.getElementById('button').addEventListener('click', () => {
  //item clicked
})
```
This is the so-called callback.

## Promise

## async/await
