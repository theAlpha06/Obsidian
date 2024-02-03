---
link: "[[Functions]]"
---
- A function together bundled with its lexical scope is called closure.
- In other words, a closure gives you access to another function's scope from an outer function. In JS, closure is created every time a function gets executed.

```js
	function x() {
		var a = 7;
		function y() {
->			console.log(a);
		}
		y();
	}
	x();
```

```js
function x() {
	var a = 7;
	function y() {
		console.log(a);
	}
	return y;
}
var z = x();

// y function still remembers its lexical scope -> closure was retured.
```

### Use Cases

- Module Design Patterns
- Currying
- Function like once()
- Memoize
- Maintaining state in async world
- setTimeouts
- Iterations
- Data Hiding and Encapsulation

## Closures with `setTimeout()`

```js
function x() {
	var i = 1;
	setTimeout(function (){ ---
		console.log(i);        |-> returns a closure
	}, 3000);               ---
	console.log("Shubham Singh");
}
x();
```

```js
function x() {
	for(var i = 1; i <= 5; i++) {
		setTimeout(function() {
			console.log(i);
		}, i * 1000);
	}
	console.log("Shubham Singh");
}
x();
```

```console
6
6
6
6
6
```

-> Solution
- use `let`(block scope)
- Each function call has a separate reference to different variable.
