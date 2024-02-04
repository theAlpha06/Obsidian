---
link: "[[let and const]]"
---


> Where you can access a variable / function from.

- Lexical Environment: Whenever an execution context is created LE is created.

```js
function a() {
	function c() {
		console.log("Lexical");	
	}
}
```

> `c()` is lexical inside `a()`.

- Global execution context also has lexical environment of parent which points to NULL.
- This way of finding is called scope chain, where if variable is not found in local then it goes to parent then to his parent until the variable is not found.