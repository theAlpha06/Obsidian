### `async` and `await`

- What is `async`?
		This is a keyword which is used to create `async` function.
		
```js
async function getData() {}
```

> This function always returns a Promise.

```js
async function getData() {
	return "Shubham";
}
```

> If we don't return a `Promise`, then this function will automatically wrap it inside a `Promise` and return it.

```js
const data = getData();
console.log(data);
```

> This will return a `Promise`.

```js
const dataPromise = getData();

dataPromise
.then(res => console.log(res));
```

> This will log the value returned.

```js
const p = new Promise((resolve, reject) => {
	resolve("Promise resolved value");
})

async function getData() {
	return p;
}

const dataPromise = getData();

dataPromise
.then(res => console.log(res));
```

### `async` with `await`

- Both are used to handle `Promises`.

```js
const p = new Promise((resolve, reject) => {
	resolve("Promise resolved value");
})

function getData() {
	p.then(res => console.log(res));
}
getData();
```

> This is how `Promise` was handled before `async` and `await`.

```js
const p = new Promise((resolve, reject) => {
	resolve("Promise resolved value");
})

async function handlePromise() {
	const val = await p;
	console.log(val);
}
handlePromise();
```

> This is how `Promise` is handled with `async` and `await`.

- `await` can only be used inside an `async` function.
### Difference b/w `async` and `await` and older ways

```js

const p = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve("Promise resolved value!!");
	}, 10000)
});

function getData() {
	p.then(res => console.log(res));
	console.log("What will be printed first?");// This will be printed first, then the Promise value will be printed.
}

getData();

async function handlePromise() {
	// JS Engine appears to be waiting over here for promise to resolve
	const val = await p;
	// The code will not proceed wntill the Promise is resolved.
	console.log("What will be printed first?");
	console.log(val);
}
```

```js
async function handlePromise() {
	const val = await p;
	console.log("What will be printed first?");
	console.log(val);
	const val2 = await p;
	console.log("What will be printed first2?");
	console.log(val2);
}
```

<kbd>Console</kbd> After 10s

```console
What will be printed first?
Promise value resolved!
What will be printed first2?
Promise value resolved!
```

```js
	const p1 = new Promise((resolve, reject)=> {
		setTimeout(() => {
			resolve("Promise resolved value");
		}, 10000)
	})
	
	const p2 = new Promise((resolve, reject)=> {
		setTimeout(() => {
			resolve("Promise resolved value");
		}, 5000)
	})
	
	async function handlePromise() {
->		console.log("Promise");
		const val = await p1;
->		console.log("What will be printed first?");
		console.log(val);
		const val2 = await p2;
->		console.log("What will be printed first2?");
		console.log(val2);
	}
```

<kbd>Console</kbd> After 10s

```console
Promise
What will be printed first?
Promise value resolved!
What will be printed first2?
Promise value resolved!
```

### Real life scenario

```js
async function handlePromise() {
	const data = await fetch("https://api.github.com/users/theAlpha06");
	//fetch() => returns Response object and this is a readable stream
	// fetch() => Response.json() -> this is also a Promise
	const user = await data.json();
	console.log(user);
}
```

### Error Handling

- Use `try` and `catch`