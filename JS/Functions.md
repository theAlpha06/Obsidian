---
link: "[[Event Loop]]"
---
### Function Statement aka Function Declaration

```js
function a() {
	console.log('a called');
}
a();
```

### Function Expression

```js
var b = function () {
	console.log('b called');
}
b();
```

### Anonymous Function

- They are used when the functions are used as values
- This is also function statement but their use case is different.
- The functions which don't have any name is called anonymous function.
```js
function () {}
```

<span style="color: red">Syntax
Error: Function Statements require a function name</span>

### Named Function Expression

```js
var c = function xyz() {
	console.log('c called');
}
c();
```


```js
xyz();
```

<span style="color: red">ReferenceError: xyz is not defined</span>

```js
var c = function xyz() {
	console.log(xyz);
}
c();
```

- This will work because xyz is not created outside the scope it's local to c.

### Difference between Parameters and Arguments?

```js
function g(parameters) {
	console.log(parameters);
}
g('arguments');
```


### First Class Function

- We can even pass function as an arguments.
- The ability of functions to be used as value and can be passes as arguments to another function and can be returned from function, this ability is known as first class function.
- Functions are first class citizens in JS. It means the same thing.

### Callback Function

- You can pass a function to another function. This function which we pass to another function is called callback function.
- These callback functions gives us the access to the asynchronous world in a synchronous single threaded language.

```js
	setTimeout(function () {
->		console.log('Timer function');
	}, 5000);
	function x(y) {
		console.log('x');
		y();
	}
	x(function y(){
->		console.log('y');
	})
```

> Everything in the code is executed through the `Call Stack`, so if any operation blocks this `Call Stack`,  ans that's called blocking the main thread. We should never block main thread i.e. we should never block this `Call Stack` is what we are referring to.

### Event Listeners

```html
<button id="clickMe">Click Me</button>
```

```js
	document.getElementById('clickMe).addEventListener('click', function xyz() {
->		console.log('Button Clicked');
	});
```

### Closure with Event Listener

```js
function attachEventListeners() {
let count = 0;
	document.getElementById('clickMe).addEventListener('click', function xyz(){
->		console.log('Button Clicked', ++count);
	});
}

attachEventListeners();
```


![[event_listener.png]]


### Why do we need to remove Event Listeners?

- Event Listeners are heavy. It takes memory. 
- It forms a closure and even if the call stack is empty, still Event Listeners free up the memory. 
- It's a good practise to remove them. 
- When we remove event listeners then all the variables which were held by the listeners will be collected by the garbage collector.