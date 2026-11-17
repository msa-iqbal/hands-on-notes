# The `this` Keyword

The `this` keyword is a special JavaScript keyword that refers to the execution context in which a function is called.

Its value depends primarily on how a function is invoked, not where the function is defined.

In JavaScript, `this` can refer to:

* The object that invokes a method.
* The global object in certain function-call contexts.
* `undefined` inside strict-mode standalone function calls.
* A specific object supplied through `call()`, `apply()`, or `bind()`.
* The newly created instance when a constructor is called with `new`.

Example:

```javascript
const user = {
  name: "Muhammad",

  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

user.introduce();
```

Output:

```text
Hello, I am Muhammad
```

Here, `this` refers to the `user` object because `introduce()` is called as a method of `user`.

# 🗁 How JavaScript Determines `this`

The value of `this` is determined by the function's invocation pattern.

The most important invocation patterns are:

| Invocation Pattern | Value of `this` |
| --- | --- |
| Standalone function in strict mode | `undefined` |
| Object method call | The object before the dot |
| Constructor with `new` | Newly created instance |
| `call()` or `apply()` | Explicitly supplied value |
| `bind()` | Permanently bound value |
| Arrow function | Inherited from the surrounding lexical scope |

Understanding these rules is essential when working with objects, callbacks, classes, and event handlers.

# 🗁 Global Context

At the top level of a traditional browser script, `this` refers to the global object, which is `window`.

```javascript
console.log(this === window);
```

Output in a traditional browser script:

```text
true
```

In a Node.js CommonJS module, top-level `this` refers to `module.exports`, not the global object.

```javascript
console.log(this === module.exports);
```

Output in a Node.js CommonJS module:

```text
true
```

In an ECMAScript module, top-level `this` is `undefined`.

```javascript
console.log(this);
```

Output in an ES module:

```text
undefined
```

The top-level behavior of `this` depends on the JavaScript execution environment and module system.

# 🗁 `this` Inside Regular Functions

For a regular function, the value of `this` depends on how the function is called.

**Strict mode**

```javascript
"use strict";

function showThis() {
  console.log(this);
}

showThis();
```

Output:

```text
undefined
```

A standalone function call does not provide a `this` value in strict mode.

**Non-strict mode**

```javascript
function showThis() {
  console.log(this);
}

showThis();
```

In a traditional non-strict browser script, `this` refers to the global object.

```text
Window
```

In non-strict mode, JavaScript substitutes the global object when a regular function is called with `this` set to `null` or `undefined`.

# 🗁 `this` Inside Object Methods

When a function is called as an object method, `this` refers to the object used for that method call.

```javascript
const user = {
  name: "Muhammad",

  getName() {
    return this.name;
  }
};

console.log(user.getName());
```

Output:

```text
Muhammad
```

The expression `user.getName()` makes `user` the receiver of the method call.

**Method invocation determines the receiver**

```javascript
const user = {
  name: "Muhammad",

  introduce() {
    console.log(this.name);
  }
};

const anotherUser = {
  name: "Alex",
  introduce: user.introduce
};

user.introduce();
anotherUser.introduce();
```

Output:

```text
Muhammad
Alex
```

The same function produces different results because it is invoked through different objects.

# 🗁 Losing `this` Reference

Assigning an object method to a standalone variable can lose its original receiver.

```javascript
const user = {
  name: "Muhammad",

  introduce() {
    console.log(this.name);
  }
};

const introduce = user.introduce;

introduce();
```

In strict mode, `this` is `undefined`, so accessing `this.name` throws a `TypeError`.

The function is no longer being invoked through `user`.

**Preserving the method receiver**

You can use `bind()` to preserve the intended object.

```javascript
const introduce = user.introduce.bind(user);

introduce();
```

Output:

```text
Muhammad
```

# 🗁 `this` Inside Nested Objects

For nested object methods, `this` refers to the object immediately preceding the method call.

```javascript
const company = {
  name: "Tech Solutions",

  employee: {
    name: "Muhammad",

    introduce() {
      console.log(this.name);
    }
  }
};

company.employee.introduce();
```

Output:

```text
Muhammad
```

Here, `this` refers to `company.employee`, not `company`.

# 🗁 `this` Inside Nested Functions

A regular nested function does not automatically inherit `this` from its containing method.

```javascript
const user = {
  name: "Muhammad",

  introduce() {
    function showName() {
      console.log(this.name);
    }

    showName();
  }
};

user.introduce();
```

In strict mode, `this` inside `showName()` is `undefined`.

This happens because `showName()` is called as a standalone function.

**Solution: Use an Arrow Function**

Arrow functions inherit `this` from their surrounding lexical scope.

```javascript
const user = {
  name: "Muhammad",

  introduce() {
    const showName = () => {
      console.log(this.name);
    };

    showName();
  }
};

user.introduce();
```

Output:

```text
Muhammad
```

The arrow function inherits `this` from `introduce()`.

# 🗁 `this` Inside Arrow Functions

Arrow functions do not have their own `this`.

Instead, they capture `this` from the surrounding lexical environment when they are created.

```javascript
const user = {
  name: "Muhammad",

  introduce: () => {
    console.log(this.name);
  }
};

user.introduce();
```

This does not work as a normal object method.

The arrow function does not receive `user` as its `this` value.

In a strict module environment, the surrounding top-level `this` is `undefined`.

**Correct usage**

Use a regular method when you need the calling object as `this`.

```javascript
const user = {
  name: "Muhammad",

  introduce() {
    console.log(this.name);
  }
};

user.introduce();
```

Arrow functions are particularly useful for callbacks that need to preserve the surrounding method's `this`.

# 🗁 `this` with `call()`

The `call()` method invokes a function with an explicitly supplied `this` value.

Syntax:

```javascript
functionName.call(thisArg, arg1, arg2, ...);
```

Example:

```javascript
function introduce() {
  console.log(`Hello, I am ${this.name}`);
}

const user = {
  name: "Muhammad"
};

introduce.call(user);
```

Output:

```text
Hello, I am Muhammad
```

The `call()` method immediately invokes the function using `user` as its `this` value.

It also allows arguments to be passed individually.

```javascript
function introduce(city, country) {
  console.log(
    `${this.name} lives in ${city}, ${country}`
  );
}

const user = {
  name: "Muhammad"
};

introduce.call(user, "Dhaka", "Bangladesh");
```

Output:

```text
Muhammad lives in Dhaka, Bangladesh
```

# 🗁 `this` with `apply()`

The `apply()` method works similarly to `call()`, but accepts function arguments as an array or array-like object.

Syntax:

```javascript
functionName.apply(thisArg, [arg1, arg2, ...]);
```

Example:

```javascript
function introduce(city, country) {
  console.log(
    `${this.name} lives in ${city}, ${country}`
  );
}

const user = {
  name: "Muhammad"
};

introduce.apply(user, ["Dhaka", "Bangladesh"]);
```

Output:

```text
Muhammad lives in Dhaka, Bangladesh
```

**Difference between call() and apply()**

| `call()` | `apply()` |
| --- | --- |
| Accepts arguments individually | Accepts arguments as an array or array-like object |
| `fn.call(obj, a, b)` | `fn.apply(obj, [a, b])` |
| Invokes immediately | Invokes immediately |

# 🗁 `this` with `bind()`

The `bind()` method creates a new function with a permanently bound `this` value.

Unlike `call()` and `apply()`, it does not invoke the function immediately.

Syntax:

```javascript
const newFunction = functionName.bind(thisArg);
```

Example:

```javascript
const user = {
  name: "Muhammad"
};

function introduce() {
  console.log(`Hello, I am ${this.name}`);
}

const boundIntroduce = introduce.bind(user);

boundIntroduce();
```

Output:

```text
Hello, I am Muhammad
```

The returned function retains the bound `this` value when called normally.

**Binding arguments**

You can also provide initial arguments when calling `bind()`.

```javascript
function introduce(city, country) {
  console.log(
    `${this.name} lives in ${city}, ${country}`
  );
}

const user = {
  name: "Muhammad"
};

const boundIntroduce = introduce.bind(
  user,
  "Dhaka"
);

boundIntroduce("Bangladesh");
```

Output:

```text
Muhammad lives in Dhaka, Bangladesh
```

The first argument is pre-filled, while the remaining argument is supplied later.

# 🗁 `this` with Constructors

When a regular function is invoked with `new`, JavaScript creates a new object and binds `this` to that object.

```javascript
function User(name, age) {
  this.name = name;
  this.age = age;
}

const user = new User("Muhammad", 25);

console.log(user);
```

Output:

```javascript
User {
  name: "Muhammad",
  age: 25
}
```

The constructor function initializes the newly created object through `this`.

The `new` operator performs several operations:

* Creates a new object.
* Links the new object's prototype to the constructor's `prototype` property.
* Calls the constructor with the new object as `this`.
* Returns the new object unless the constructor explicitly returns another object.

# 🗁 `this` Inside Classes

Inside class constructors and regular instance methods, `this` refers to the current instance.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
}

const user = new User("Muhammad");

user.introduce();
```

Output:

```text
Hello, I am Muhammad
```

Each instance has its own property values.

```javascript
const user1 = new User("Muhammad");
const user2 = new User("Alex");

user1.introduce();
user2.introduce();
```

Output:

```text
Hello, I am Muhammad
Hello, I am Alex
```

Class methods run in strict mode automatically.

# 🗁 `this` Inside Static Methods

Static methods belong to the class itself rather than its instances.

When called through the class, `this` refers to the class.

```javascript
class User {
  static role = "Administrator";

  static getRole() {
    return this.role;
  }
}

console.log(User.getRole());
```

Output:

```text
Administrator
```

If a static method is inherited and invoked through a subclass, `this` refers to that subclass.

```javascript
class User {
  static getClassName() {
    return this.name;
  }
}

class Admin extends User {}

console.log(Admin.getClassName());
```

Output:

```text
Admin
```

# 🗁 `this` Inside Event Handlers

In traditional DOM event listeners, a regular function used as an event handler receives the element to which the listener is attached as `this`.

```javascript
const button = document.querySelector("button");

button.addEventListener("click", function () {
  console.log(this);
});
```

When the button is clicked, `this` refers to the button element.

It is also available through `event.currentTarget`.

```javascript
button.addEventListener("click", function (event) {
  console.log(this === event.currentTarget);
});
```

Output:

```text
true
```

**Arrow function event handler**

Arrow functions do not receive their own `this`.

```javascript
button.addEventListener("click", (event) => {
  console.log(event.currentTarget);
});
```

Use `event.currentTarget` when you need the element associated with the listener and want to use an arrow function.

# 🗁 `this` with `setTimeout()`

Regular functions passed to `setTimeout()` do not automatically retain the `this` value of the surrounding object.

```javascript
const user = {
  name: "Muhammad",

  introduce() {
    setTimeout(function () {
      console.log(this.name);
    }, 1000);
  }
};

user.introduce();
```

The callback is invoked without `user` as its receiver. In strict mode, `this` inside the callback is `undefined`.

**Solution: Use an Arrow Function**

```javascript
const user = {
  name: "Muhammad",

  introduce() {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  }
};

user.introduce();
```

Output after one second:

```text
Muhammad
```

The arrow function inherits `this` from the `introduce()` method.

# 🗁 Explicit and Implicit Binding

JavaScript method calls can be understood through binding rules.

**Implicit binding**

The object before the dot determines `this`.

```javascript
const user = {
  name: "Muhammad",

  showName() {
    console.log(this.name);
  }
};

user.showName();
```

**Explicit binding**

The programmer explicitly supplies `this` using `call()`, `apply()`, or `bind()`.

```javascript
function showName() {
  console.log(this.name);
}

const user = {
  name: "Muhammad"
};

showName.call(user);
```

**Constructor binding**

The `new` operator binds `this` to the newly created instance.

```javascript
function User(name) {
  this.name = name;
}

const user = new User("Muhammad");

console.log(user.name);
```

**Lexical binding**

Arrow functions inherit `this` from their surrounding scope.

```javascript
const user = {
  name: "Muhammad",

  showName() {
    const callback = () => {
      console.log(this.name);
    };

    callback();
  }
};

user.showName();
```

# 🗁 Important Rules About `this`

* Regular functions receive `this` based on how they are called.
* Object methods use the object that invokes them as their `this` value.
* Assigning a method to a standalone variable can lose its original receiver.
* Arrow functions do not have their own `this`.
* Arrow functions inherit `this` lexically.
* `call()` and `apply()` invoke a function with an explicit `this` value.
* `bind()` creates a new function with a bound `this` value.
* Constructor calls with `new` bind `this` to the new instance.
* Class bodies execute in strict mode.
* Static methods receive the class or subclass as `this` when invoked through it.
* Top-level `this` differs between browser scripts, CommonJS modules, and ES modules.

# 🗁 Common Mistakes

**Using an arrow function as an object method when you need the object as `this`**

```javascript
const user = {
  name: "Muhammad",

  showName: () => {
    console.log(this.name);
  }
};
```

Use a regular method instead.

```javascript
const user = {
  name: "Muhammad",

  showName() {
    console.log(this.name);
  }
};
```

**Assuming `this` is determined by where a function is defined**

Regular function `this` is determined by the invocation pattern, not its definition location.

**Assuming nested regular functions inherit `this`**

Regular nested functions have their own `this` determined by how they are called.

**Confusing `call()`, `apply()`, and `bind()`**

* `call()` invokes immediately with individual arguments.
* `apply()` invokes immediately with an array-like argument list.
* `bind()` returns a new function with a bound `this` value.

**Assuming a bound function can always be rebound**

Calling `bind()` on an already bound function does not replace its existing bound `this` value.

**Assuming `this` always refers to the object containing a method**

A method can be extracted and called independently, changing its `this` value.

# 🗁 Best Practices

* Use regular methods when an object method needs access to the calling object through `this`.
* Use arrow functions for callbacks that should inherit the surrounding `this`.
* Use `bind()` when passing object methods as callbacks and preserving their receiver.
* Prefer `event.currentTarget` when working with DOM event handlers.
* Avoid relying on global `this` in application code.
* Understand whether your code runs as a browser script, CommonJS module, or ES module.
* Use `call()` and `apply()` when you need immediate invocation with an explicit receiver.
* Do not use arrow functions as constructors.
* Keep method invocation patterns clear to avoid unexpected `this` behavior.

# 🗁 Key Takeaways

* `this` is a special keyword whose value depends on the execution context and invocation pattern.
* Regular function `this` is determined when the function is called.
* Object method calls implicitly bind `this` to the calling object.
* Arrow functions inherit `this` from their surrounding lexical scope.
* `call()` and `apply()` invoke functions with an explicit receiver.
* `bind()` creates a new function with a bound receiver.
* The `new` operator binds `this` to a newly created instance.
* Class methods use strict-mode behavior.
* Understanding `this` is essential for object-oriented JavaScript, callbacks, event handling, and asynchronous programming.
