---
link: "[[Prototype and it's inheritance]]"
---

# `map`, `filter` and `reduce`

> The `map()` method creates a new array with the results of calling a provided function on every element in the calling array.

## `map`

```js
const arr = [5, 1, 3, 2, 6];

// Double arr
function double(x) {
	return x * 2;
}

function triple(x) {
	return x * 3;
}
const output = arr.map(double);
```

- <mark>map is used when you have an array of stuff( scientific term ) and you want to do something( another scientific term ) for every item in that array.</mark>

## `filter`

- <mark>The `filter()` method creates a new array with all elements that pass the test implemented by the provided function.</mark>

```js
//filter odd values
function isOdd(x) {
	return x % 2;
}
const output = arr.filter(isOdd);
```

## `reduce`

```js
// Take all the values of array and come up with single value
// Ex. to find sum of arr

function findSum(arr) {
	let sum = 0;
	for(let i = 0; i < arr.length(); i++) {
		sum = sum + arr[i];
	}
	return sum;
}

// using reduce function
const output = arr.reduce(function(accumulator, current) {
	accumulator = accumulator + current;
	return accumulator;
}, 0); // initial value of accumulator

// max value of arr
const maxi = arr.reduce(function(acc, curr) {
	if(acc < curr) acc = curr;
	return acc;
}, -1);
```

###  Examples

```js
const users = [
	{
		firstName: "Sunny", lastName: "Singh", age: 22
	},
		{
		firstName: "Shubham", lastName: "Singh", age: 21
	},
		{
		firstName: "Kritagyay", lastName: "Upadhayay", age: 200
	},
		{
		firstName: "Shudhanshu", lastName: "Arya", age: 22
	},
];

// list of full names
const output = users.map(user => `${user.firstName} ${user.lastName}`);

// first name of all users whose age is 23 or greater
const output = users.filter(user => user.age >= 23).map((x) => x.firstName);

//with reduce
const output = users.reduce(function (acc, curr) {
	if(curr.age >= 23) {
		acc.push(curr.firstName);
	}
	return acc;
}, []);

// users with particular age
const output = users.reduce(function (acc, curr) {
	if(acc[curr.age]) {
		acc[curr.age] = ++acc[curr.age];
	} else {
		acc[curr.age] = 1;
	}
	return acc;
}, {})
```

