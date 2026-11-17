# Object Destructuring

Object destructuring is a JavaScript feature that allows you to extract property values from an object and assign them to variables.

It provides a concise way to access object properties without repeatedly using dot notation or bracket notation.

Example:

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  profession: "Software Engineer"
};

const { name, age, profession } = user;

console.log(name);
console.log(age);
console.log(profession);
```

Output:

```text
Muhammad
25
Software Engineer
```

Without destructuring, you would write:

```javascript
const name = user.name;
const age = user.age;
const profession = user.profession;
```

Destructuring makes extracting multiple properties easier and more readable.

# 🗁 Basic Object Destructuring

The basic syntax uses curly braces on the left side of an assignment.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const { name, age } = user;
```

JavaScript matches the variable names with the corresponding object property names.

The order of properties does not matter.

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  city: "Dhaka"
};

const { city, name, age } = user;

console.log(name);
console.log(age);
console.log(city);
```

Output:

```text
Muhammad
25
Dhaka
```

# 🗁 Renaming Destructured Variables

You can assign an object property to a variable with a different name using an alias.

Syntax:

```javascript
const { propertyName: variableName } = object;
```

Example:

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const { name: userName, age: userAge } = user;

console.log(userName);
console.log(userAge);
```

Output:

```text
Muhammad
25
```

Here:

* `name` is the original object property.
* `userName` is the new variable name.
* `age` is the original object property.
* `userAge` is the new variable name.

The original object remains unchanged.

# 🗁 Default Values

Destructuring supports default values when a property is missing or its value is `undefined`.

```javascript
const user = {
  name: "Muhammad"
};

const { name, age = 25 } = user;

console.log(name);
console.log(age);
```

Output:

```text
Muhammad
25
```

The default value is used because `age` does not exist.

Default values are also used when a property explicitly contains `undefined`.

```javascript
const user = {
  name: "Muhammad",
  age: undefined
};

const { age = 25 } = user;

console.log(age);
```

Output:

```text
25
```

However, `null` does not trigger the default value.

```javascript
const user = {
  age: null
};

const { age = 25 } = user;

console.log(age);
```

Output:

```text
null
```

# 🗁 Renaming Properties with Default Values

You can combine aliases and default values.

```javascript
const user = {
  name: "Muhammad"
};

const {
  name: userName,
  age: userAge = 25
} = user;

console.log(userName);
console.log(userAge);
```

Output:

```text
Muhammad
25
```

# 🗁 Nested Object Destructuring

Nested destructuring allows you to extract properties from objects inside other objects.

```javascript
const user = {
  name: "Muhammad",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

const {
  address: { city, country }
} = user;

console.log(city);
console.log(country);
```

Output:

```text
Dhaka
Bangladesh
```

You can also extract the nested object itself.

```javascript
const user = {
  name: "Muhammad",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

const { address } = user;

console.log(address.city);
console.log(address.country);
```

Output:

```text
Dhaka
Bangladesh
```

# 🗁 Nested Destructuring with Renaming

Nested properties can also be renamed.

```javascript
const user = {
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

const {
  address: {
    city: userCity,
    country: userCountry
  }
} = user;

console.log(userCity);
console.log(userCountry);
```

Output:

```text
Dhaka
Bangladesh
```

# 🗁 Rest Properties

The rest property (`...`) collects the remaining enumerable own properties that were not selected during destructuring.

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  email: "muhammad@example.com",
  profession: "Software Engineer"
};

const { name, age, ...otherDetails } = user;

console.log(name);
console.log(age);
console.log(otherDetails);
```

Output:

```javascript
Muhammad
25

{
  email: "muhammad@example.com",
  profession: "Software Engineer"
}
```

The rest property must appear last in the destructuring pattern.

```javascript
const { name, ...details } = user;
```

# 🗁 Destructuring Existing Variables

When assigning destructured values to existing variables, wrap the assignment expression in parentheses.

```javascript
let name;
let age;

const user = {
  name: "Muhammad",
  age: 25
};

({ name, age } = user);

console.log(name);
console.log(age);
```

Output:

```text
Muhammad
25
```

Without parentheses, JavaScript interprets the opening curly brace as the beginning of a block rather than an object destructuring assignment.

# 🗁 Computed Property Names

You can use computed property names to extract properties whose keys are stored in variables.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const property = "name";

const { [property]: value } = user;

console.log(value);
```

Output:

```text
Muhammad
```

The computed key is evaluated, and its value is assigned to the variable `value`.

# 🗁 Destructuring Function Parameters

Object destructuring is commonly used in function parameters.

```javascript
function displayUser({ name, age }) {
  console.log(`Name: ${name}`);
  console.log(`Age: ${age}`);
}

const user = {
  name: "Muhammad",
  age: 25
};

displayUser(user);
```

Output:

```text
Name: Muhammad
Age: 25
```

This allows functions to receive objects and directly access the properties they need.

# 🗁 Default Values in Function Parameters

You can combine parameter destructuring with default values.

```javascript
function createUser({
  name = "Unknown",
  age = 18,
  country = "Bangladesh"
} = {}) {
  return {
    name,
    age,
    country
  };
}

console.log(createUser({
  name: "Muhammad",
  age: 25
}));

console.log(createUser());
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25,
  country: "Bangladesh"
}

{
  name: "Unknown",
  age: 18,
  country: "Bangladesh"
}
```

The `= {}` default allows the function to be called without passing an argument.

Without it, calling the function without an argument would cause a `TypeError` during destructuring.

# 🗁 Destructuring in Loops

Object destructuring can simplify iteration over arrays of objects.

```javascript
const users = [
  { id: 1, name: "Muhammad" },
  { id: 2, name: "Alex" },
  { id: 3, name: "John" }
];

for (const { id, name } of users) {
  console.log(`${id}: ${name}`);
}
```

Output:

```text
1: Muhammad
2: Alex
3: John
```

You can also destructure key-value pairs returned by `Object.entries()`.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}
```

Output:

```text
name: Muhammad
age: 25
```

# 🗁 Destructuring with Optional Chaining

Optional chaining (`?.`) and destructuring solve different problems.

Destructuring extracts properties, while optional chaining safely accesses properties when an intermediate value may be `null` or `undefined`.

You cannot directly use optional chaining inside a destructuring pattern.

Instead, provide a safe fallback object.

```javascript
const user = {
  name: "Muhammad",
  address: null
};

const { city } = user.address ?? {};

console.log(city);
```

Output:

```text
undefined
```

For nested properties:

```javascript
const user = {
  profile: null
};

const {
  profile: {
    city
  } = {}
} = user;

console.log(city);
```

This still throws a `TypeError` because `profile` exists with the value `null`, rather than being `undefined`.

A safer approach is:

```javascript
const user = {
  profile: null
};

const {
  city
} = user.profile ?? {};

console.log(city);
```

Output:

```text
undefined
```

# 🗁 Destructuring and Object References

Destructuring does not create a deep copy of nested objects.

When you extract an object-valued property, the new variable references the same nested object.

```javascript
const user = {
  name: "Muhammad",
  address: {
    city: "Dhaka"
  }
};

const { address } = user;

address.city = "Chittagong";

console.log(user.address.city);
```

Output:

```text
Chittagong
```

Both `address` and `user.address` reference the same nested object.

Primitive values, on the other hand, are assigned as values.

```javascript
const user = {
  age: 25
};

const { age } = user;

user.age = 30;

console.log(age);
console.log(user.age);
```

Output:

```text
25
30
```

# 🗁 Common Mistakes

**Using incorrect property names**

```javascript
const user = {
  name: "Muhammad"
};

const { username } = user;

console.log(username);
```

Output:

```text
undefined
```

Destructuring matches property names exactly.

**Destructuring null or undefined**

```javascript
const user = null;

const { name } = user;
```

This throws a `TypeError` because `null` cannot be destructured.

**Forgetting parentheses during reassignment**

```javascript
let name;

const user = {
  name: "Muhammad"
};

// Correct
({ name } = user);
```

**Assuming default values replace null**

```javascript
const user = {
  age: null
};

const { age = 25 } = user;

console.log(age);
```

Output:

```text
null
```

Defaults apply only when the property value is `undefined`.

**Assuming destructuring creates independent nested objects**

Extracting a nested object preserves its reference rather than creating a deep copy.

# 🗁 Practical Example: API Response Handling

```javascript
const response = {
  status: "success",
  data: {
    user: {
      id: 101,
      name: "Muhammad",
      email: "muhammad@example.com"
    },
    permissions: ["read", "write"]
  }
};

const {
  status,
  data: {
    user: {
      id,
      name,
      email
    },
    permissions
  }
} = response;

console.log(status);
console.log(id);
console.log(name);
console.log(email);
console.log(permissions);
```

Output:

```text
success
101
Muhammad
muhammad@example.com
["read", "write"]
```

This pattern is useful when working with structured API responses.

# 🗁 Best Practices

* Use destructuring when extracting multiple properties.
* Rename variables when the original property names are unclear or conflict with existing variables.
* Use default values for optional properties.
* Use nested destructuring when it improves readability.
* Avoid deeply nested destructuring patterns that become difficult to understand.
* Use rest properties when collecting remaining object properties.
* Use parameter destructuring to make function dependencies explicit.
* Provide fallback objects when destructuring potentially missing values.
* Remember that nested objects retain their original references.

# 🗁 Key Takeaways

* Object destructuring extracts properties into variables.
* Property order does not matter.
* Aliases allow properties to be assigned to differently named variables.
* Default values apply when a property is `undefined`.
* Nested destructuring extracts properties from nested objects.
* Rest properties collect remaining enumerable own properties.
* Function parameters can be destructured directly.
* Computed property names support dynamic extraction.
* Destructuring nested objects does not create independent copies.
* Object destructuring improves readability when working with structured data.
