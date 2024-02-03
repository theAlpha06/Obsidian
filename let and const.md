---
link: "[[Closures]]"
---

- `let` and `const` declarations are hoisted.
- These are in the temporal dead zone.
- They are stored in a separable memory called `temporal dead zone`, this is the place where `let` and `const` declarations are present between hoisting and initialisation.
- Whenever you try to access variable from temporal dead zone, it gives <span style="color:red">`Reference Error`</span>.
- Re-declaration gives <span style="color:red">`SyntaxError`</span>.
- <span style="color:red">TypeError</span>: Assignment const.

## Scope

- `let` and `const` are blocked scoped.

```js
{
	// compound statement
}
var a = 100;     --------
{                        | -> Both are refering to same memory space. This is called `shadowing`;
	var a = 10;  --------
	let b = 20;
	const c = 30;
	console.log(a, b, c);
}
console.log(a);
```

### Illegal Shadowing

```js
let a = 20; 
{
	let a = 100;
}
```

```js
let a = 20;
{
	var a = 100;
}
// This is wrong.
// vice-versa is also wrong.
```


-> `var` is function scoped

```js

let a = 20;

function x() {
	var a = 100;
}
console.log(a);

```

- Block also follows lexical scope.
- Scope rules applies to arrow function the same way.
- 