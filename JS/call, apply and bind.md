
# `call()`

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

- The only difference between `call` and `bind` is, bind gives a copy which can be invoked later.

```js
let printMyName = printFullName.bind(name2, "Mau");
console.log(printMyName);
printMyName();
```