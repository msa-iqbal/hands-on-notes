# Data Transformation

Data transformation is the process of converting data from one structure, format, or representation into another.

In JavaScript, data transformation is commonly used to:

* Convert API responses into application-friendly structures.
* Extract specific properties from objects.
* Filter and sort collections.
* Normalize inconsistent data.
* Convert strings into numbers or dates.
* Combine multiple datasets.
* Prepare data for UI components.
* Transform data before sending it to an API.

Example:

```javascript
const users = [
  { id: 1, firstName: "Alice", lastName: "Smith" },
  { id: 2, firstName: "Bob", lastName: "Johnson" }
];

const transformedUsers = users.map((user) => ({
  id: user.id,
  fullName: `${user.firstName} ${user.lastName}`
}));

console.log(transformedUsers);
```

Output:

```javascript
[
  { id: 1, fullName: "Alice Smith" },
  { id: 2, fullName: "Bob Johnson" }
]
```

The original data structure is transformed into a new structure that better fits the application's requirements.

# 🗁 Understanding Data Transformation

Applications frequently receive data in a format that differs from what their internal logic or UI requires.

For example, an API might return:

```json
{
  "user_id": 101,
  "first_name": "Alice",
  "last_name": "Smith",
  "account_status": "active"
}
```

The frontend might prefer:

```javascript
{
  id: 101,
  fullName: "Alice Smith",
  isActive: true
}
```

A transformation layer converts the API representation into the application's domain representation.

Typical workflow:

```text
External Data
     |
     v
Data Validation
     |
     v
Data Transformation
     |
     v
Application Data Model
     |
     v
UI / Business Logic
```

Separating transformation logic from presentation and business logic makes applications easier to maintain.

# 🗁 Mapping Data with map()

The `map()` method creates a new array by transforming every element of an existing array.

Syntax:

```javascript
array.map((element, index, array) => {
  return transformedElement;
});
```

Example:

```javascript
const prices = [100, 200, 300];

const discountedPrices = prices.map((price) => {
  return price * 0.9;
});

console.log(discountedPrices);
```

Output:

```javascript
[90, 180, 270]
```

The original array remains unchanged.

```javascript
console.log(prices); // [100, 200, 300]
```

Use `map()` when every input element should produce a corresponding output element.

# 🗁 Extracting Properties from Objects

Data transformation often involves selecting only the properties needed by an application.

Example:

```javascript
const users = [
  {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
    password: "secret"
  },
  {
    id: 2,
    name: "Bob",
    email: "bob@example.com",
    password: "secret"
  }
];

const publicUsers = users.map(({ id, name, email }) => ({
  id,
  name,
  email
}));

console.log(publicUsers);
```

Output:

```javascript
[
  {
    id: 1,
    name: "Alice",
    email: "alice@example.com"
  },
  {
    id: 2,
    name: "Bob",
    email: "bob@example.com"
  }
]
```

This creates a new representation containing only the selected properties.

Do not treat client-side property removal as a substitute for server-side access control.

# 🗁 Renaming Object Properties

An API may use naming conventions that differ from the application's conventions.

For example, an API might return snake_case properties while the application uses camelCase.

```javascript
const apiUser = {
  user_id: 101,
  first_name: "Alice",
  last_name: "Smith",
  email_address: "alice@example.com"
};

const user = {
  id: apiUser.user_id,
  firstName: apiUser.first_name,
  lastName: apiUser.last_name,
  email: apiUser.email_address
};

console.log(user);
```

Output:

```javascript
{
  id: 101,
  firstName: "Alice",
  lastName: "Smith",
  email: "alice@example.com"
}
```

Property renaming helps maintain consistent internal data models.

# 🗁 Transforming Nested Objects

Nested API responses often require restructuring before being used by an application.

Example:

```javascript
const apiResponse = {
  user: {
    id: 101,
    profile: {
      first_name: "Alice",
      last_name: "Smith"
    },
    location: {
      city: "Dhaka",
      country: "Bangladesh"
    }
  }
};

const user = {
  id: apiResponse.user.id,
  fullName: [
    apiResponse.user.profile.first_name,
    apiResponse.user.profile.last_name
  ].join(" "),
  location: apiResponse.user.location.city
};

console.log(user);
```

Output:

```javascript
{
  id: 101,
  fullName: "Alice Smith",
  location: "Dhaka"
}
```

For deeply nested structures, dedicated transformation functions help avoid repeating complex property access throughout the application.

# 🗁 Filtering Data with filter()

The `filter()` method creates a new array containing only elements that satisfy a condition.

Example:

```javascript
const users = [
  { id: 1, name: "Alice", active: true },
  { id: 2, name: "Bob", active: false },
  { id: 3, name: "Charlie", active: true }
];

const activeUsers = users.filter((user) => user.active);

console.log(activeUsers);
```

Output:

```javascript
[
  { id: 1, name: "Alice", active: true },
  { id: 3, name: "Charlie", active: true }
]
```

Use `filter()` when some elements should be excluded from the resulting collection.

# 🗁 Sorting Data with sort()

The `sort()` method sorts array elements.

Example:

```javascript
const products = [
  { name: "Keyboard", price: 120 },
  { name: "Mouse", price: 40 },
  { name: "Monitor", price: 250 }
];

const sortedProducts = [...products].sort(
  (a, b) => a.price - b.price
);

console.log(sortedProducts);
```

The spread operator creates a shallow copy before sorting because `sort()` mutates the original array.

For descending order:

```javascript
const descending = [...products].sort(
  (a, b) => b.price - a.price
);
```

# 🗁 Aggregating Data with reduce()

The `reduce()` method combines array elements into a single accumulated result.

Example:

```javascript
const orders = [
  { id: 1, amount: 100 },
  { id: 2, amount: 200 },
  { id: 3, amount: 150 }
];

const total = orders.reduce((sum, order) => {
  return sum + order.amount;
}, 0);

console.log(total); // 450
```

`reduce()` can also transform an array into an object.

Example:

```javascript
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];

const userById = users.reduce((result, user) => {
  result[user.id] = user;

  return result;
}, {});

console.log(userById);
```

Output:

```javascript
{
  1: { id: 1, name: "Alice" },
  2: { id: 2, name: "Bob" }
}
```

For this particular object-building use case, `Object.fromEntries()` is another useful option.

# 🗁 Combining Data with Spread Syntax

The spread syntax allows arrays and objects to be combined.

**Combining objects**

```javascript
const personalInfo = {
  firstName: "Alice",
  lastName: "Smith"
};

const accountInfo = {
  email: "alice@example.com",
  role: "Developer"
};

const user = {
  ...personalInfo,
  ...accountInfo
};

console.log(user);
```

**Combining arrays**

```javascript
const frontend = ["HTML", "CSS", "JavaScript"];
const backend = ["Node.js", "PostgreSQL"];

const technologies = [
  ...frontend,
  ...backend
];

console.log(technologies);
```

When object properties have the same key, the value from the later spread takes precedence.

```javascript
const defaults = {
  theme: "light",
  language: "en"
};

const preferences = {
  theme: "dark"
};

const settings = {
  ...defaults,
  ...preferences
};

console.log(settings);
```

Output:

```javascript
{
  theme: "dark",
  language: "en"
}
```

Spread syntax performs a shallow copy, not a deep copy.

# 🗁 Combining Data with Object.assign()

`Object.assign()` copies enumerable own properties from source objects into a target object.

Example:

```javascript
const defaults = {
  theme: "light",
  language: "en"
};

const preferences = {
  theme: "dark"
};

const settings = Object.assign({}, defaults, preferences);

console.log(settings);
```

Output:

```javascript
{
  theme: "dark",
  language: "en"
}
```

The first argument is the target object.

Unlike spreading into a new object, `Object.assign()` mutates the target object.

# 🗁 Transforming Strings into Numbers

API and form data frequently contain numeric values represented as strings.

Example:

```javascript
const formData = {
  quantity: "5",
  price: "99.50"
};

const product = {
  quantity: Number(formData.quantity),
  price: Number(formData.price)
};

console.log(product);
```

Output:

```javascript
{
  quantity: 5,
  price: 99.5
}
```

Other conversion methods include:

```javascript
parseInt("42", 10);   // 42
parseFloat("42.75");  // 42.75
Number("42");         // 42
```

Validate converted values because invalid input may produce `NaN`.

```javascript
const quantity = Number("invalid");

console.log(Number.isNaN(quantity)); // true
```

# 🗁 Transforming Dates

Date values received from APIs are often represented as strings.

Example:

```javascript
const apiResponse = {
  createdAt: "2026-11-14T10:30:00.000Z"
};

const user = {
  createdAt: new Date(apiResponse.createdAt)
};

console.log(user.createdAt instanceof Date); // true
```

A Date object provides methods for formatting and manipulating date values.

```javascript
const formattedDate = user.createdAt.toLocaleDateString();

console.log(formattedDate);
```

Use explicit date formats and timezone assumptions when transforming dates across systems.

# 🗁 Normalizing Data

Data normalization is the process of organizing data into a consistent structure.

It is particularly useful when dealing with large datasets and relational information.

Consider this nested structure:

```javascript
const response = {
  users: [
    {
      id: 1,
      name: "Alice",
      posts: [
        {
          id: 101,
          title: "JavaScript Basics"
        }
      ]
    },
    {
      id: 2,
      name: "Bob",
      posts: [
        {
          id: 102,
          title: "TypeScript Guide"
        }
      ]
    }
  ]
};
```

The nested representation duplicates relationships inside each user.

A normalized structure separates entities:

```javascript
const normalizedData = {
  users: {
    1: {
      id: 1,
      name: "Alice"
    },
    2: {
      id: 2,
      name: "Bob"
    }
  },

  posts: {
    101: {
      id: 101,
      userId: 1,
      title: "JavaScript Basics"
    },
    102: {
      id: 102,
      userId: 2,
      title: "TypeScript Guide"
    }
  }
};
```

Benefits include:

* Easier entity lookup.
* Reduced data duplication.
* Simpler updates.
* Consistent relationships.
* More predictable state management.

Normalization is especially useful in complex frontend applications.

# 🗁 Denormalizing Data

Denormalization transforms normalized data into a structure that is convenient for a particular use case.

Example:

```javascript
const data = {
  users: {
    1: {
      id: 1,
      name: "Alice"
    }
  },

  posts: {
    101: {
      id: 101,
      userId: 1,
      title: "JavaScript Basics"
    }
  }
};

const posts = Object.values(data.posts).map((post) => ({
  ...post,
  author: data.users[post.userId]
}));

console.log(posts);
```

Output:

```javascript
[
  {
    id: 101,
    userId: 1,
    title: "JavaScript Basics",
    author: {
      id: 1,
      name: "Alice"
    }
  }
]
```

Denormalization is useful when a UI needs related data in one convenient structure.

# 🗁 Grouping Data

Grouping organizes records according to a shared property.

Example:

```javascript
const products = [
  { name: "Laptop", category: "Electronics" },
  { name: "Phone", category: "Electronics" },
  { name: "Desk", category: "Furniture" }
];

const groupedProducts = Object.groupBy(
  products,
  (product) => product.category
);

console.log(groupedProducts);
```

Output:

```javascript
{
  Electronics: [
    { name: "Laptop", category: "Electronics" },
    { name: "Phone", category: "Electronics" }
  ],
  Furniture: [
    { name: "Desk", category: "Furniture" }
  ]
}
```

`Object.groupBy()` is a modern JavaScript method that returns an object of groups.

For Map-based grouping, JavaScript also provides `Map.groupBy()`.

# 🗁 Removing Duplicate Data

Data received from different sources may contain duplicate values.

A `Set` can remove duplicate primitive values.

```javascript
const languages = [
  "JavaScript",
  "TypeScript",
  "JavaScript",
  "Python"
];

const uniqueLanguages = [...new Set(languages)];

console.log(uniqueLanguages);
```

Output:

```javascript
[
  "JavaScript",
  "TypeScript",
  "Python"
]
```

For objects, duplicates must be identified using a meaningful property.

Example:

```javascript
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 1, name: "Alice" }
];

const uniqueUsers = [
  ...new Map(users.map((user) => [user.id, user])).values()
];

console.log(uniqueUsers);
```

This keeps one record per ID. If duplicate IDs contain different data, the later record replaces the earlier one.

# 🗁 Flattening Nested Arrays

Flattening converts nested arrays into arrays with fewer levels.

The `flat()` method creates a new array with nested elements flattened.

Example:

```javascript
const numbers = [
  [1, 2],
  [3, 4],
  [5, 6]
];

console.log(numbers.flat());
```

Output:

```javascript
[1, 2, 3, 4, 5, 6]
```

For deeper nesting:

```javascript
const nested = [1, [2, [3, [4]]]];

console.log(nested.flat(Infinity));
```

Output:

```javascript
[1, 2, 3, 4]
```

# 🗁 Flat Mapping Data

`flatMap()` combines mapping and one-level flattening.

Example:

```javascript
const users = [
  {
    name: "Alice",
    skills: ["JavaScript", "React"]
  },
  {
    name: "Bob",
    skills: ["Node.js", "PostgreSQL"]
  }
];

const allSkills = users.flatMap((user) => user.skills);

console.log(allSkills);
```

Output:

```javascript
[
  "JavaScript",
  "React",
  "Node.js",
  "PostgreSQL"
]
```

It is useful when each input element can produce zero, one, or multiple output elements.

# 🗁 Transforming API Responses

A dedicated transformation function can convert an API response into an application-friendly model.

Example:

```javascript
function transformUser(apiUser) {
  return {
    id: apiUser.user_id,
    fullName: `${apiUser.first_name} ${apiUser.last_name}`,
    email: apiUser.email_address,
    isActive: apiUser.status === "active"
  };
}

const apiUser = {
  user_id: 101,
  first_name: "Alice",
  last_name: "Smith",
  email_address: "alice@example.com",
  status: "active"
};

const user = transformUser(apiUser);

console.log(user);
```

This pattern separates external API contracts from internal application models.

For production code, validate the incoming object before accessing its properties.

# 🗁 Immutable Data Transformation

Immutable transformation means producing new data without modifying the original input.

Example:

```javascript
const user = {
  id: 1,
  name: "Alice",
  preferences: {
    theme: "light"
  }
};

const updatedUser = {
  ...user,
  preferences: {
    ...user.preferences,
    theme: "dark"
  }
};

console.log(user.preferences.theme);        // light
console.log(updatedUser.preferences.theme); // dark
```

The nested object is copied separately because spreading the outer object alone would only create a shallow copy.

Immutable transformations are useful for predictable state management and change detection.

# 🗁 Pure Transformation Functions

A pure transformation function:

* Produces the same output for the same input.
* Does not modify external state.
* Does not mutate its input.
* Does not depend on hidden side effects.

Example:

```javascript
function calculateDiscount(price, percentage) {
  return price - (price * percentage) / 100;
}

console.log(calculateDiscount(100, 10)); // 90
```

Pure functions are easier to test, reuse, and reason about.

A transformation function that mutates its input is less predictable:

```javascript
function addFullName(user) {
  user.fullName = `${user.firstName} ${user.lastName}`;

  return user;
}
```

An immutable alternative:

```javascript
function addFullName(user) {
  return {
    ...user,
    fullName: `${user.firstName} ${user.lastName}`
  };
}
```

# 🗁 Practical Example: Transforming an API Product List

Suppose an API returns products in this format:

```javascript
const apiProducts = [
  {
    product_id: 1,
    product_name: "Laptop",
    unit_price: "1200",
    stock_status: "available"
  },
  {
    product_id: 2,
    product_name: "Mouse",
    unit_price: "40",
    stock_status: "unavailable"
  },
  {
    product_id: 3,
    product_name: "Keyboard",
    unit_price: "80",
    stock_status: "available"
  }
];
```

Transform the response into an application-friendly structure:

```javascript
const products = apiProducts.map((product) => ({
  id: product.product_id,
  name: product.product_name,
  price: Number(product.unit_price),
  inStock: product.stock_status === "available"
}));

console.log(products);
```

Filter available products:

```javascript
const availableProducts = products.filter(
  (product) => product.inStock
);
```

Sort them by price:

```javascript
const sortedProducts = [...availableProducts].sort(
  (a, b) => a.price - b.price
);
```

Calculate the total value:

```javascript
const totalValue = sortedProducts.reduce(
  (total, product) => total + product.price,
  0
);

console.log(totalValue);
```

This example combines mapping, filtering, sorting, and aggregation into a practical transformation pipeline.

# 🗁 Data Transformation Pipeline

A transformation pipeline combines multiple operations to process data step by step.

Example:

```javascript
const orders = [
  { id: 1, amount: 100, status: "completed" },
  { id: 2, amount: 200, status: "pending" },
  { id: 3, amount: 300, status: "completed" },
  { id: 4, amount: 50, status: "cancelled" }
];

const result = orders
  .filter((order) => order.status === "completed")
  .map((order) => ({
    orderId: order.id,
    amount: order.amount
  }))
  .reduce((total, order) => total + order.amount, 0);

console.log(result); // 400
```

Each operation has a specific responsibility:

* `filter()` selects relevant records.
* `map()` transforms their structure.
* `reduce()` combines them into a single result.

This makes the processing logic easier to understand and maintain.

# 🗁 Common Mistakes

**Mutating the original array while sorting**

Incorrect:

```javascript
const sorted = products.sort(
  (a, b) => a.price - b.price
);
```

`sort()` modifies the original array.

Prefer:

```javascript
const sorted = [...products].sort(
  (a, b) => a.price - b.price
);
```

**Assuming spread syntax performs a deep copy**

Object and array spread create shallow copies. Nested references remain shared unless copied separately.

**Converting invalid strings into numbers without validation**

```javascript
const price = Number("invalid");

console.log(price); // NaN
```

Validate numeric conversions before using the result.

**Assuming API responses always have the expected structure**

External data should be validated before transformation.

**Using reduce() when another method is clearer**

Use `map()` for mapping, `filter()` for filtering, and `reduce()` when an accumulated result is actually needed.

**Mixing transformation logic with UI rendering**

Keep data transformation separate from rendering when possible. This improves testability and reuse.

# 🗁 Best Practices

* Use `map()` for transforming every array element.
* Use `filter()` for selecting records.
* Use `reduce()` for aggregation.
* Use immutable transformations when working with application state.
* Normalize data when relationships and repeated entities become complex.
* Use pure functions for reusable transformations.
* Validate external data before processing it.
* Convert data types explicitly.
* Keep API response models separate from internal application models.
* Avoid unnecessary transformations and repeated iterations.
* Use meaningful function names that describe the transformation.
* Test edge cases such as missing properties, empty arrays, and invalid values.

# 🗁 Summary

Data transformation converts information into structures and formats that better fit application requirements.

Important concepts include:

* Mapping and filtering.
* Property extraction and renaming.
* Nested object transformation.
* Sorting and aggregation.
* Combining objects and arrays.
* Type and date conversion.
* Data normalization and denormalization.
* Grouping and removing duplicates.
* Flattening and flat mapping.
* Immutable and pure transformations.
* API response transformation.
* Transformation pipelines.

Data transformation is a fundamental skill for building maintainable JavaScript applications, especially when integrating APIs, managing application state, and preparing data for user interfaces.
