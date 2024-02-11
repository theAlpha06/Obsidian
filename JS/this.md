# `this`

```js
// this is global space

console.log(this); // global object
// this == window
```

- `this` keyword inside global space always points to the global object. 

```js
// this inside a function

function x() {
	console.log(this);
}
x();
```

- `window` object will be consoled if run in `non-strict` mode, and `undefined` if run inside `strict` mode.

```js
// this inside non-strict mode(this substitution)

// If the value of this keyword is undefined or null
// this keyword will be replaced with globalObject only in non-strict mode
```

```js
// this keyword value depends on how this is called (window)
"use strict";
x(); // this -> undefined
window.x(); // this -> window
```

```js
// this inside a object's method
const obj = {
	a: 10,
	// if a function inside an object it's a method
	x: function() {
		console.log(this);
	}
}

obj.x(); // this -> obj

```

```js
// call apply and bind methods(sharing methods)

const student = {
	name: "Shubham",
	printName: funciton() {
		console.log(this.name);
	}
}

student.printName();

const student2 = {
	name: "Sunny",
}

student.printName.call(student2); // .call takes the value of this keyword to overwrite it's value.

```

```js
// this inside arow function
// arrow functions don't have their own this bindingj(it retains the this value of the enclosing lexical content)

const obj = {
	a: 10,
	x: () => {
		console.log(this); // this will behave as if it's present inside the global object, coz it's lexical context is global
	}
}
console.log(this); // the same result.
obj.x();
```

```js
// this inside nested arrow function

const obj2 = {
	a: 20,
	x: funciton () {
		const y = () => {
			console.log(this); // this -> obj2
		}
		y();
	}
}
obj2.x();
```

```html

// this inside DOM -> the value is reference to the HTML elements
<button onclick="alert(this)">Click Me</button>

// this -> [object HTMLButtonElement]

```

```js
// this inside class, constructor
```