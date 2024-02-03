---
link: "[[Regular Event Loop]]"
---
-  If you give `Call Stack` a code and say it to execute it after 5 seconds later, it can't do that. Because, `call stack` doesn't have timer.
- Browsers provides us with a lot of features like location service, timer, local storage, etc. Now, if the `Call Stack` which resides in `JS Engine` needs to use it, then it is done using the `Web APIs`.

![[js_runtime.png]]

> All these `Web APIs`, are not a part of `Javascript`, they are actually browsers power. We get all these access into `call stack` using the global object `window`, e.g. `window.setTimeout()`, we can access it also using `setTimeout()` because `window` is in global scope.


```js
console.log('Start');
setTimeout(function cb() {
	console.log('Callback');
}, 5000);
//setTimeout registers the cb(inside the web api environment) and sets the timer and once the call stack is clear it's executed if the timer has completed.
// Now we somehow need the cb into call stack this is done using event loop and callback queue.
// It goes to the call stack using the callback queue.
// And the event loop checks the callback queue and put the function inside the callback queue to call stack if it's empty.
console.log('End');
```

```js
console.log('Start');
document.getElementById('btn')
.addEventListener('click', function cb() {
	console.log('Callback');
})
console.log('End');

// addEventListener() is a DOM API.
// This also registers a callback with a 'click' event attached to it.
// The cb method stays there event after the complete execution of the code untill, we explicitly remove it or close the browser.
```

### Why do we need `Callback Queue` or `Task Queue`?

- Suppose that user clicks on the 'btn' then there will be 5 -6  callback function pushed to callback queue.
- Event loop gradually executes all the callback function one-by-one.

### `fetch()` doesn't works the same way as `setTimeout()` does.

```js
console.log('Start');
setTimeout(function cbT() {
	console.log('CB setTimeout');
}, 5000);
fetch('https://api.netflix.com')
.then(function cbF() {
	//This will execute once the promise is resolved
	console.log('CB Netflix');
});
console.log('End');
```

> Note: After the promise is resolved the `callback function` inside the `Web APIs environment` doesn't go to the `callback queue`, but to `microtask queue` which has higher priority than `callback queue`.
> 
> `mutation observers` and `Promises` goes inside the `microtask queue`, rest all goes to `callback queue`.
> 
> If microtask creates new microtask and the process goes on, then the callback queue will never get the chance and this is called `starvation`.