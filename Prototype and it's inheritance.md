# Prototype

- Inheritance is one object trying to access the methods and properties of other objects.

```js
let arr = [ "Shubham", "Sunny" ];
let object = {
	name: "Shubham",
	city: "Ballia",
	getIntro: function() {
		console.log(`${this.name} from ${this.city}`);
	}
}
```

- Whenever we create a JS object, JS engine automatically attaches object with some hidden properties(object which has some properties) and function which can be accessed by `object.toString()` and what not. These comes via `prototype`.

> We can access that hidden object using `Array.prototype` for arrays.



![[image.png]]

```js
arr.__proto__ = Array.prototype

// The prototype of Array
Array.prototype = Object.prototype

// The prototype of Object
Object.prototype = null

// This is prototype chain
```

- <mark>Everything in JS is Object.</mark>

### Inheritance

```js

let object2 = {
	name: "Sunny"
}

// Never do this, causes performance issue
object2.__proto__ = object;

// This will point to the object created before.
// Now we can access the object methods and properties in object2.
console.log(object2.name, object2.city);
console.log(object.getIntro(), object2.getIntro());
```

- `object2` is inheriting the property `city` from parent, this is called `prototypal inheritance`.