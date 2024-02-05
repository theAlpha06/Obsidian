---
link: "[[map, filter and reduce]]"
---
# Higher Order Functions

- A function which takes function as a parameter or return a function is called `higher order function`.
```js
function x() {
	console.log("Shubham");
}
function y(x) {
	x();
}

// y is the higher order function.
// x is the callback function.
```

```js

const radii = [ 3, 1, 2, 4 ];

// Calculate the area of the circle

// Most common solution
const calculateArea = function (radius) {
	const output = [];
	for(let i = 0; i < radius.length(); i++) {
		output.push(Math.PI * radius[i] * radius[i]);
	}
	return output;
}

// What if you have to calculate the circumference
const calculateCircumference = function(radii) {
	const output = [];
	for(let i = 0; i < radii.length(); i++) {
		output.push(2 * Math.PI * radii[i]);
	}
	return output;
}

// What if diameter is also required
const calculateDiameter = function(radii) {
	const output = [];
	for(let i = 0; i < radii.length(); i++) {
		output.push(2 * radii[i]);
	}
	return output;
}
```

<span style="color: red;">Note: What about `DRY`</span>

```js

const area = function (radius) {
	return Math.PI * radius * radius;
}

const calculate = function(radii, logic) {
	const output = [];
	for(let i = 0; i < radii.length(); i++) {
		output.push(logic(radii[i]));
	}
	return output;
}

const areaOfCircle = calculate(radii, area);
```

- The `map()` function of JS is somewhat similar with the above `calculate` function which we have written. 
```js
const area = radii.map(area);
// This gives the same result as calculate function

// Pollyfill for map
Array.prototype.calculate = function(logic) {
	const output = [];
	for(let i = 0; i < this.length(); i++) {
		output.push(logic(this[i]));
	}
	return output;
}

radii.calculate(area);

// Once we add Array.prototype, it is available to all the arrays.
```

> This is the beauty of functional programming.