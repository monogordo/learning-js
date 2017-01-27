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


### Functions
TBA

### Hoisting

Variable declarations are hoisted to the top of the function or scope. So a variable can be used before it is declared. *var* and *let* will produce different outcomes when it comes to hoisting.

Function declarations are hoisted as well, but function expressions are not. Both function declarations and variable declarations are hoisted. But functions are hoisted first, and then variables.

**Note:** Only the declarations themselves are hoisted, while any assignments or other executable logic are left in place for the execution phase.

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

```javascript
foo(); // not ReferenceError, but TypeError!
var foo = function bar() {
  // ...
};
```

### Immediately Invoked Function Expressions (IIFEs)
```javascript
(function IIFE(){
  console.log( "Hello!" );
})();
// "Hello!"
```

### Scope

**Lexical vs. Dynamic scope**

Lexical scope means that scope is defined by author-time decisions of where functions are declared. The lexing phase of compilation is essentially able to know where and how all identifiers are declared, and thus predict how they will be looked-up during execution.

Dynamic scope, by contrast, doesn't concern itself with how and where functions and scopes are declared, but rather where they are called from. In other words, the scope chain is based on the call-stack, not the nesting of scopes in code. Lexical scope is write-time, whereas dynamic scope (and `this`!) are runtime.

JavaScript which uses lesxical scoping outputs `2` when it executes the code below. But if JavaScript had dynamic scope, when foo() is executed, theoretically the code below would instead result in `3` as the output.

```javascript
function foo() {
  console.log( a );
}

function bar() {
  var a = 3;
  foo();
}

var a = 2;

bar();
```

**`eval()` and `with`**

Two mechanisms in JavaScript can "cheat" lexical scope: eval(..) and with. The former can modify existing lexical scope (at runtime) by evaluating a string of "code" which has one or more declarations in it. The latter essentially creates a whole new lexical scope (again, at runtime) by treating an object reference as a "scope" and that object's properties as scoped identifiers.

The downside to these mechanisms is that it defeats the Engine's ability to perform compile-time optimizations regarding scope look-up, because the Engine has to assume pessimistically that such optimizations will be invalid. Code will run slower as a result of using either feature. Don't use them.

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

There are two "requirements" for the module pattern to be exercised:

1. There must be an outer enclosing function, and it must be invoked at least once (each time creates a new module instance).

2. The enclosing function must return back at least one inner function, so that this inner function has closure over the private scope, and can access and/or modify that private state.

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

### Math object

**Math.random()**
The `Math.random()` function returns a floating-point, pseudo-random number in the range [0, 1)

Getting a random integer between two values, inclusive:

```javascript
function getRandomIntInclusive(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

### Prototypes
When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.

## DOM
### `window.pageXOffset` / `window.pageYOffset`

The `pageXOffset` property is an alias for the `scrollX` property. It returns the number of pixels that the document has already been scrolled horizontally.

For cross-browser compatibility, use `window.pageXOffset` instead of `window.scrollX`. Additionally, older versions of Internet Explorer (< 9) do not support either property

```javascript
window.addEventListener('scroll', function(){
    overlay.style.top = window.pageYOffset + 'px';
    overlay.style.left = window.pageXOffset + 'px';
});
```
### `Window.innerWidth` / `Window.innerHeight`

Width and height (in pixels) of the browser window viewport including, if rendered, the horizontal and vertical scrollbar.

```javascript
window.addEventListener('resize', function(){
    overlay.style.width = window.innerWidth + 'px';
    overlay.style.width = window.innerHeight + 'px';
});
```
















