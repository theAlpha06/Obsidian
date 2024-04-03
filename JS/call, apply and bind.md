---
link: "[[async and defer]]"
---


In JavaScript, `call`, `apply`, and `bind` are methods that allow you to control the execution context of functions. They all serve similar purposes, but they have differences in how they're used and how they pass arguments to the function.

# `call()`

- Syntax: `function.call(thisArg, arg1, arg2, ...)`
- The call method is used to call a function with a given this value and arguments provided individually.
- It takes the context (thisArg) as its first parameter and passes arguments individually.
- It immediately invokes the function.
- Best suited for cases where you know the arguments you want to pass to the function upfront.

```js
let name = {
	firstname: "Shubham",
	lastname: "Singh",
	printFullName: function() {
		console.log(this.firstname+" "+this.lastname);
	}
}
name.printFullName();

let name2 = {
	firstname: "Sunny",
	lastname: "Singh"
}

// Function Borrowing
name.printFullName.call(name2);
```

```js
// How we actually write the code

let name = {
	firstname: "Shubham",
	lastname: "Singh"
}

let printFullName: function(hometown) {
	console.log(this.firstname+" "+this.lastname+" from "+hometown);
}

printFullName.call(name, "ballia");

let name2 = {
	firstname: "Sunny",
	lastname: "Singh"
}

// Function Borrowing
printFullName.call(name2, "mau");

```

# `apply()`

- Syntax: function.apply(thisArg, [argsArray])
- Similar to call, but it accepts arguments as an array.
- The apply method is used to call a function with a given this value and arguments provided as an array.
- It takes the context (thisArg) as its first parameter and an array of arguments.
- It immediately invokes the function.
- Best suited for cases where you have arguments in an array or you want to pass all arguments as an array.
- The only difference between `call()` and `apply()` is how the arguments are passed.

```js
let name = {
	firstname: "Shubham",
	lastname: "Singh"
}

let printFullName: function(hometown) {
	console.log(this.firstname+" "+this.lastname+" from "+hometown);
}

printFullName.apply(name, ["ballia"]);

let name2 = {
	firstname: "Sunny",
	lastname: "Singh"
}

// Function Borrowing
// in apply we pass the args as an array
printFullName.apply(name2, ["mau"]);
```

# `bind()`
- Syntax: function.bind(thisArg, arg1, arg2, ...)
- The bind method is used to create a new function that, when called, has its this value set to the provided thisArg, with a given sequence of arguments preceding any provided when the new function is called.
- Unlike call and apply, bind does not immediately invoke the function. Instead, it returns a new function with the specified context and arguments.
- Best suited for cases where you want to create a function with a specific context and possibly some pre-set arguments, but you don't want to execute the function immediately.
- The only difference between `call` and `bind` is, bind gives a copy which can be invoked later.

```js
let printMyName = printFullName.bind(name2, "Mau");
console.log(printMyName);
printMyName();
```
