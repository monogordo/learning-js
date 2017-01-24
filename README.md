# JavaScript


### Static vs. Dynamic typing

**Static typing**

A variable holds a specific type of value, such as number or string. 

**Dynamic typing**

A variable can hold any type of value at any time. JavaScript uses dynamic typing. 

### Types ###

JavaScript has seven built-in types:
* string
* number
* boolean
* null
* undefined
* object
* symbol (ES6)

We can use `typeof` operator to determine the type of a value:
```javascript
var a;
typeof a;               // "undefined"

a = "hello world";
typeof a;               // "string"

a = { b: "c" };
typeof a;               // "object"
```
*Note:*  Only values have types in JavaScript not variables. Variables are just containers for those values.

### Variable declaration

**var**

Declares a variable attached to the enclosing function (or global, if top level) scope.

**let**

Declares a block scope local variable.

**const**

Declares a constant value for a variable. The scope rules for constants are the same as those for let block scope variables. By convention, JavaScript variables as constants are usually capitalized, with underscores _ between multiple words. E.g. `const TAX_RATE = 0.05;`


```javascript
let x = 1;
{
  let x = 2;
}
console.log(x); // outputs 1
```

```javascript
var x = 1;
{
  var x = 2;
}
console.log(x); // outputs 2
```

```javascript
if (true) {
  var x = 5;
}
console.log(x);  // x is 5
```

```javascript
if (true) {
  let y = 5;
}
console.log(y);  // ReferenceError: y is not defined
```

### Hoisting

Variable declarations are hoisted to the top of the function or scope. So a variable can be used before it is declared. *var* and *let* will produce different outcomes when it comes to hoisting.

**Note:** Only the declarations themselves are hoisted, while any assignments or other executable logic are left in place

```javascript
x = 2;
var x;
console.log(x); // outputs 2
```

```javascript
console.log(x); // undefined
var x = 3;
```

```javascript
console.log(x); // ReferenceError
let x = 3;
```

### Truthy vs. Falsy


**"falsy" values in JavaScript**:

* `""` (empty string)
* `0`, `-0`, `NaN` (invalid number)
* `null`, `undefined`
* `false`

**"truthy" values in JavaScript**:

Any value that's not on this "falsy" list is "truthy." Here are some examples of those:

* `"hello"`
* `42`
* `true`
* `[ ]`, `[ 1, "2", 3 ]` (arrays)
* `{ }`, `{ a: 42 }` (objects)
* `function foo() { .. }` (functions)


### Arrays

An array is an object that holds values (of any type) not particularly in named properties/keys, but rather in numerically indexed positions.


### Immediately Invoked Function Expressions (IIFEs)
```javascript
(function IIFE(){
  console.log( "Hello!" );
})();
// "Hello!"
```

### Closure
A way to "remember" and continue to access a function's scope (its variables) even once the function has finished running.
```javascript
function makeAdder(x) {
  // parameter `x` is an inner variable

  // inner function `add()` uses `x`, so
  // it has a "closure" over it
  function add(y) {
      return y + x;
  };

  return add;
}

// `plusOne` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusOne = makeAdder( 1 );

// `plusTen` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusTen = makeAdder( 10 );

plusOne( 3 );       // 4  <-- 1 + 3
plusOne( 41 );      // 42 <-- 1 + 41
plusTen( 13 );      // 23 <-- 10 + 13
```

### Modules
The most common usage of closure in JavaScript is the module pattern. Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as a public API that is accessible from the outside.

```javascript
function User(){
  var username, password;

  function doLogin(user,pw) {
    username = user;
    password = pw;

    // do the rest of the login work
  }

  var publicAPI = {
    login: doLogin
  };

  return publicAPI;
}

// create a `User` module instance
var fred = User();

fred.login( "fred", "12Battery34!" );
```

### `this` Identifier

```javascript
function foo() {
  console.log( this.bar );
}

var bar = "global";

var obj1 = {
  bar: "obj1",
  foo: foo
};

var obj2 = {
  bar: "obj2"
};

// --------

foo();              // "global"
obj1.foo();         // "obj1"
foo.call( obj2 );   // "obj2"
new foo();          // undefined
```

### Objects

Object are compound values that can contain properties that each hold their own values of any type. Properties can either be accessed with dot notation (e.g. `obj.a`) or bracket notation (e.g. `obj["a"]`). 

```javascript
var obj = {
  a: "hello world",
  b: 42,
  c: true
};

obj.a;      // "hello world"
obj.b;      // 42
obj.c;      // true

obj["a"];   // "hello world"
obj["b"];   // 42
obj["c"];   // true
```

### Prototypes
When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.
