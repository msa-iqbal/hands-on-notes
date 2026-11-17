# TypeScript String Type

The `string` type represents textual data in TypeScript. It is used to store and manipulate characters, words, sentences, and other text-based values.

TypeScript supports JavaScript string functionality while providing static type checking to ensure that variables declared as `string` contain string values.

# 🗁 Declaring String Variables

Use the `string` keyword to explicitly declare a string variable.

```typescript
let firstName: string = "John";
let lastName: string = "Doe";
let country: string = "Bangladesh";

console.log(firstName);
console.log(lastName);
console.log(country);
```

TypeScript also supports type inference.

```typescript
let username = "John";

// TypeScript infers username as string.
username = "David";

// TypeScript error
// username = 100;
```

# 🗁 String Literals

JavaScript and TypeScript support three ways to define string literals.

**Single quotes:**

```typescript
const firstName: string = 'John';
```

**Double quotes:**

```typescript
const lastName: string = "Doe";
```

**Backticks (template literals):**

```typescript
const message: string = `Hello, TypeScript!`;
```

All three produce string values, but template literals provide additional functionality, including interpolation and multiline strings.

# 🗁 String Concatenation

String concatenation combines multiple strings using the `+` operator.

```typescript
const firstName: string = "John";
const lastName: string = "Doe";

const fullName: string = firstName + " " + lastName;

console.log(fullName); // John Doe
```

You can also use the `+=` assignment operator.

```typescript
let message: string = "Hello";

message += " ";
message += "TypeScript";

console.log(message); // Hello TypeScript
```

# 🗁 Template Literals

Template literals use backticks and allow expressions to be embedded using `${}`.

```typescript
const name: string = "John";
const age: number = 25;

const message: string = `My name is ${name} and I am ${age} years old.`;

console.log(message);
```

Template literals can contain calculations and function calls.

```typescript
const price: number = 100;
const quantity: number = 3;

const message: string = `Total: ${price * quantity}`;

console.log(message); // Total: 300
```

They also support multiline strings.

```typescript
const description: string = `
TypeScript is a programming language.
It builds on JavaScript.
It provides static type checking.
`;

console.log(description);
```

# 🗁 String Length

The `length` property returns the number of UTF-16 code units in a string.

```typescript
const message: string = "TypeScript";

console.log(message.length); // 10
```

The length property is useful for checking string sizes.

```typescript
const username: string = "john";

if (username.length >= 3) {
    console.log("Valid username");
}
```

Note that `length` does not always represent the number of user-perceived characters. Some Unicode characters use multiple UTF-16 code units.

# 🗁 Accessing String Characters

Strings support zero-based indexing.

```typescript
const language: string = "TypeScript";

console.log(language[0]); // T
console.log(language[1]); // y
console.log(language[2]); // p
console.log(language[9]); // t
```

You can also use the `charAt()` method.

```typescript
const language: string = "TypeScript";

console.log(language.charAt(0)); // T
console.log(language.charAt(4)); // S
```

An out-of-range index using bracket notation returns `undefined`, while `charAt()` returns an empty string.

```typescript
const text: string = "Hello";

console.log(text[10]);       // undefined
console.log(text.charAt(10)); // ""
```

# 🗁 String Immutability

Strings in JavaScript and TypeScript are immutable.

This means their characters cannot be modified directly after a string is created.

```typescript
let message: string = "Hello";

// This does not change the original string.
message[0] = "J";

console.log(message); // Hello
```

To create a modified string, assign a new string value.

```typescript
let message: string = "Hello";

message = "J" + message.slice(1);

console.log(message); // Jello
```

# 🗁 Common String Methods

TypeScript supports the standard JavaScript string methods.

| Method | Description |
| --- | --- |
| `toUpperCase()` | Converts text to uppercase |
| `toLowerCase()` | Converts text to lowercase |
| `trim()` | Removes whitespace from both ends |
| `trimStart()` | Removes whitespace from the beginning |
| `trimEnd()` | Removes whitespace from the end |
| `includes()` | Checks whether a substring exists |
| `startsWith()` | Checks whether a string starts with specified text |
| `endsWith()` | Checks whether a string ends with specified text |
| `indexOf()` | Returns the first matching position |
| `lastIndexOf()` | Returns the last matching position |
| `slice()` | Extracts part of a string |
| `substring()` | Extracts characters between two positions |
| `replace()` | Replaces the first matching occurrence |
| `replaceAll()` | Replaces all matching occurrences |
| `split()` | Splits a string into an array |
| `repeat()` | Repeats a string |
| `padStart()` | Adds padding to the beginning |
| `padEnd()` | Adds padding to the end |

# 🗁 Changing Letter Case

The `toUpperCase()` and `toLowerCase()` methods change the letter case.

```typescript
const message: string = "Hello TypeScript";

console.log(message.toUpperCase());
// HELLO TYPESCRIPT

console.log(message.toLowerCase());
// hello typescript
```

These methods return new strings and do not modify the original string.

# 🗁 Removing Whitespace

The `trim()` method removes whitespace from both ends.

```typescript
const username: string = "   john   ";

console.log(username.trim()); // john
```

To remove whitespace from only one side:

```typescript
const text: string = "   Hello   ";

console.log(text.trimStart()); // "Hello   "
console.log(text.trimEnd());   // "   Hello"
```

# 🗁 Searching Within Strings

The `includes()` method checks whether a string contains specific text.

```typescript
const message: string = "Welcome to TypeScript";

console.log(message.includes("TypeScript")); // true
console.log(message.includes("JavaScript")); // false
```

The `startsWith()` and `endsWith()` methods check the beginning and ending of a string.

```typescript
const filename: string = "document.pdf";

console.log(filename.startsWith("document")); // true
console.log(filename.endsWith(".pdf"));       // true
```

The `indexOf()` method returns the first position of a matching substring.

```typescript
const message: string = "Hello TypeScript";

console.log(message.indexOf("TypeScript")); // 6
console.log(message.indexOf("JavaScript")); // -1
```

The `lastIndexOf()` method returns the last matching position.

```typescript
const message: string = "apple orange apple";

console.log(message.lastIndexOf("apple")); // 13
```

# 🗁 Extracting Substrings

The `slice()` method extracts part of a string.

```typescript
const language: string = "TypeScript";

console.log(language.slice(0, 4)); // Type
console.log(language.slice(4));    // Script
console.log(language.slice(-6));   // Script
```

The `substring()` method also extracts characters between positions.

```typescript
const language: string = "TypeScript";

console.log(language.substring(0, 4)); // Type
console.log(language.substring(4));    // Script
```

Unlike `slice()`, `substring()` treats negative indexes as zero and swaps its arguments when the first is greater than the second.

# 🗁 Replacing String Content

The `replace()` method replaces the first matching occurrence.

```typescript
const message: string = "Hello World";

const updated: string = message.replace("World", "TypeScript");

console.log(updated); // Hello TypeScript
```

The `replaceAll()` method replaces every matching occurrence.

```typescript
const message: string = "apple apple apple";

const updated: string = message.replaceAll("apple", "orange");

console.log(updated); // orange orange orange
```

Both methods return new strings.

# 🗁 Splitting Strings

The `split()` method divides a string into an array of substrings.

```typescript
const fullName: string = "John Doe";

const names: string[] = fullName.split(" ");

console.log(names); // ["John", "Doe"]
```

Splitting a comma-separated string:

```typescript
const technologies: string = "HTML,CSS,JavaScript,TypeScript";

const technologyList: string[] = technologies.split(",");

console.log(technologyList);
```

The separator determines where the string is divided.

# 🗁 Padding Strings

The `padStart()` and `padEnd()` methods add characters until a string reaches the specified length.

```typescript
const number: string = "5";

console.log(number.padStart(3, "0")); // 005
console.log(number.padEnd(3, "0"));   // 500
```

These methods are useful for formatting identifiers, numbers, and display values.

# 🗁 Repeating Strings

The `repeat()` method creates a new string containing repeated copies of the original string.

```typescript
const separator: string = "-";

console.log(separator.repeat(10));
// ----------
```

Example:

```typescript
const star: string = "*";

console.log(star.repeat(5)); // *****
```

# 🗁 String Comparison

Strings can be compared using equality and relational operators.

```typescript
const first: string = "apple";
const second: string = "banana";

console.log(first === second); // false
console.log(first !== second); // true
console.log(first < second);   // true
```

String relational comparisons use lexicographical ordering based on UTF-16 code units.

For language-aware comparisons, use `localeCompare()`.

```typescript
const first: string = "apple";
const second: string = "banana";

console.log(first.localeCompare(second)); // Negative number
```

The exact result depends on the comparison, but a negative number indicates that the first string sorts before the second, zero indicates equality, and a positive number indicates that it sorts after the second.

# 🗁 String Type and Unicode

JavaScript strings use UTF-16 encoding.

A string can contain ordinary characters, Unicode symbols, and emoji.

```typescript
const english: string = "Hello";
const bengali: string = "বাংলা";
const emoji: string = "😀";

console.log(english);
console.log(bengali);
console.log(emoji);
```

Some Unicode characters require two UTF-16 code units.

```typescript
const emoji: string = "😀";

console.log(emoji.length); // 2
```

The spread operator iterates strings by Unicode code points rather than individual UTF-16 code units.

```typescript
const emoji: string = "😀";

console.log([...emoji].length); // 1
```

For counting user-perceived characters, including combined emoji sequences, use `Intl.Segmenter` where supported.

```typescript
const text: string = "Hello 😀";

const segmenter = new Intl.Segmenter("en", {
    granularity: "grapheme"
});

const characters = [...segmenter.segment(text)];

console.log(characters.length);
```

# 🗁 String Type with Type Aliases

Type aliases can give meaningful names to string-based types.

```typescript
type Username = string;

const username: Username = "john";
```

You can also restrict strings using union types.

```typescript
type Status = "pending" | "approved" | "rejected";

let orderStatus: Status = "pending";

orderStatus = "approved";

// TypeScript error
// orderStatus = "completed";
```

# 🗁 String Type with Template Literal Types

Template literal types allow developers to construct string types from other types.

```typescript
type Direction = "top" | "bottom" | "left" | "right";

type Position = `margin-${Direction}`;
```

The resulting `Position` type accepts:

* `margin-top`
* `margin-bottom`
* `margin-left`
* `margin-right`

Example:

```typescript
let position: Position = "margin-top";

position = "margin-left";

// TypeScript error
// position = "padding-top";
```

Template literal types are useful for defining structured string values, event names, API keys, and configuration options.

# 🗁 Type Conversion to String

JavaScript provides several ways to convert other values into strings.

**Using `String()`:**

```typescript
const age: number = 25;

const result: string = String(age);

console.log(result); // "25"
```

**Using `toString()`:**

```typescript
const number: number = 100;

const result: string = number.toString();

console.log(result); // "100"
```

**Using template literals:**

```typescript
const price: number = 250;

const result: string = `${price}`;

console.log(result); // "250"
```

**Using concatenation:**

```typescript
const age: number = 25;

const result: string = age + "";

console.log(result); // "25"
```

`String()` is generally a clear choice for explicit conversion, especially when handling values that may be `null` or `undefined`.

# 🗁 Best Practices

* Use the `string` type for textual values.
* Prefer meaningful variable names.
* Use template literals for readable string interpolation.
* Remember that strings are immutable.
* Use `trim()` when cleaning user input.
* Use `includes()`, `startsWith()`, and `endsWith()` for common string checks.
* Use `slice()` for extracting string portions.
* Use `replaceAll()` when every matching occurrence should be replaced.
* Consider Unicode behavior when counting or processing characters.
* Use `localeCompare()` for language-aware string comparisons.
* Avoid unnecessary explicit type annotations when TypeScript can infer the type.
* Use literal types and template literal types when a string must follow a specific pattern.

# 🗁 Summary

The TypeScript `string` type represents textual data and supports JavaScript's built-in string operations.

Important concepts include:

* String declarations and type inference.
* Single quotes, double quotes, and template literals.
* String concatenation and interpolation.
* String indexing and length.
* String immutability.
* Common string methods.
* String comparison and Unicode handling.
* String conversion.
* Literal types and template literal types.

A strong understanding of strings is essential for handling user input, displaying information, processing API responses, validating data, and building web applications.
