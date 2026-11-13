# JavaScript String

A string is a primitive data type in JavaScript used to represent textual data.

Strings can contain letters, numbers, symbols, whitespace, and other characters.

JavaScript provides several ways to create strings and many built-in methods for searching, extracting, modifying, and manipulating text.

**Example:**

```javascript
const firstName = "Alice";
const message = "Hello, World!";

console.log(firstName);
console.log(message);
```

# 🗁 Creating Strings

JavaScript supports three primary ways to create string values.

**Single Quotes**

Use single quotes (`'`) to create a string.

```javascript
const name = 'Alice';
const language = 'JavaScript';

console.log(name);
console.log(language);
```

**Double Quotes**

Use double quotes (`"`) to create a string.

```javascript
const message = "Hello, World!";
const country = "Bangladesh";

console.log(message);
console.log(country);
```

**Template Literals**

Use backticks (`` ` ``) to create template literals.

```javascript
const name = "Alice";
const message = `Hello, ${name}!`;

console.log(message); // Hello, Alice!
```

Template literals support string interpolation and multiline strings.

# 🗁 String Length

The `length` property returns the number of UTF-16 code units in a string.

```javascript
const message = "Hello";

console.log(message.length); // 5
```

It is a property, not a method. Therefore, do not use parentheses.

```javascript
const language = "JavaScript";

console.log(language.length); // 10
```

For characters represented by multiple UTF-16 code units, `length` may not equal the number of user-perceived characters.

```javascript
const emoji = "😀";

console.log(emoji.length); // 2
console.log([...emoji].length); // 1
```

# 🗁 Accessing String Characters

Strings are zero-indexed, meaning the first character has index `0`.

You can access individual characters using bracket notation.

```javascript
const language = "JavaScript";

console.log(language[0]); // J
console.log(language[1]); // a
console.log(language[2]); // v
```

You can also use the `at()` method.

```javascript
const language = "JavaScript";

console.log(language.at(0));  // J
console.log(language.at(-1)); // t
console.log(language.at(-2)); // p
```

Bracket notation does not support negative indexing:

```javascript
const language = "JavaScript";

console.log(language[-1]); // undefined
```

# 🗁 String Immutability

JavaScript strings are immutable.

This means individual characters cannot be changed after a string is created.

```javascript
let message = "Hello";

message[0] = "Y";

console.log(message); // Hello
```

To create a modified string, assign a new string value.

```javascript
let message = "Hello";

message = "Y" + message.slice(1);

console.log(message); // Yello
```

String methods generally return new strings instead of modifying the original string.

# 🗁 Escape Characters

Escape characters allow special characters to be represented inside strings.

They begin with a backslash (`\`).

| Escape Sequence | Description |
| --- | --- |
| `\n` | New line |
| `\t` | Horizontal tab |
| `\r` | Carriage return |
| `\b` | Backspace |
| `\f` | Form feed |
| `\v` | Vertical tab |
| `\\` | Backslash |
| `\'` | Single quote |
| `\"` | Double quote |
| `\`` | Backtick |
| `\uXXXX` | Unicode code point using four hexadecimal digits |
| `\u{XXXX}` | Unicode code point using code point notation |
| `\xXX` | Character represented by two hexadecimal digits |

**New Line:**

```javascript
const message = "Hello\nWorld";

console.log(message);
```

Output:

```text
Hello
World
```

**Tab:**

```javascript
const message = "Name:\tAlice";

console.log(message);
```

Output:

```text
Name:   Alice
```

**Quotes:**

```javascript
const message = "She said, \"Hello!\"";

console.log(message);
```

**Unicode:**

```javascript
const heart = "\u2764";
const smile = "\u{1F600}";

console.log(heart);
console.log(smile);
```

# 🗁 String Concatenation

String concatenation combines multiple strings into one string.

**Using the `+` Operator:**

```javascript
const firstName = "Alice";
const lastName = "Smith";

const fullName = firstName + " " + lastName;

console.log(fullName); // Alice Smith
```

**Using the `+=` Operator:**

```javascript
let message = "Hello";

message += " ";
message += "World";

console.log(message); // Hello World
```

**Using Template Literals:**

```javascript
const firstName = "Alice";
const lastName = "Smith";

const fullName = `${firstName} ${lastName}`;

console.log(fullName);
```

Template literals are often easier to read when combining variables and text.

# 🗁 String Comparison

JavaScript supports comparison operators for strings.

```javascript
console.log("apple" === "apple"); // true
console.log("apple" === "Apple"); // false
console.log("apple" !== "orange"); // true
```

String comparisons are case-sensitive.

The relational operators compare strings lexicographically based on UTF-16 code units.

```javascript
console.log("apple" < "banana"); // true
console.log("A" < "a");          // true
```

For language-aware comparisons, use `localeCompare()`.

```javascript
const result = "apple".localeCompare("banana");

console.log(result);
```

Its result is negative, zero, or positive depending on the comparison.

# 🗁 Converting Values to Strings

The `String()` function converts values into strings.

```javascript
console.log(String(100));       // "100"
console.log(String(true));      // "true"
console.log(String(null));      // "null"
console.log(String(undefined)); // "undefined"
```

You can also use the `toString()` method on values that provide it.

```javascript
const number = 100;

console.log(number.toString()); // "100"
```

However, `null` and `undefined` do not provide a `toString()` method.

```javascript
// Throws TypeError
// null.toString();

// Throws TypeError
// undefined.toString();
```

# 🗁 String Type Checking

Use `typeof` to check whether a value is a string.

```javascript
const name = "Alice";
const age = 25;

console.log(typeof name); // string
console.log(typeof age);  // number
```

You can also use `typeof` in a condition:

```javascript
function printMessage(value) {
    if (typeof value === "string") {
        console.log(value);
    } else {
        console.log("A string value is required.");
    }
}

printMessage("Hello");
printMessage(100);
```

# 🗁 Unicode and UTF-16

JavaScript strings are sequences of UTF-16 code units.

A character may occupy one or two UTF-16 code units.

**Basic Multilingual Plane (BMP)**

Many commonly used characters are represented using one UTF-16 code unit.

```javascript
const letter = "A";

console.log(letter.length); // 1
```

**Supplementary Characters**

Some characters, including many emoji, require two UTF-16 code units. These are represented using a surrogate pair.

```javascript
const emoji = "😀";

console.log(emoji.length); // 2
console.log(emoji.codePointAt(0)); // 128512
```

The spread operator iterates strings by Unicode code point:

```javascript
const text = "A😀B";

console.log([...text]); // ["A", "😀", "B"]
console.log([...text].length); // 3
```

A Unicode code point is not always equivalent to a user-perceived character. For example, a combined emoji or a letter with a combining accent may contain multiple code points.

For grapheme-aware operations, use `Intl.Segmenter`.

```javascript
const text = "Hello 👨‍👩‍👧‍👦";

const segmenter = new Intl.Segmenter("en", {
    granularity: "grapheme"
});

const characters = [...segmenter.segment(text)];

console.log(characters.length);
```

# 🗁 Common String Methods

JavaScript provides many built-in string methods.

| Method | Description |
| --- | --- |
| `at()` | Returns a character at an index |
| `charAt()` | Returns a character at an index |
| `charCodeAt()` | Returns a UTF-16 code unit |
| `codePointAt()` | Returns a Unicode code point |
| `concat()` | Combines strings |
| `includes()` | Checks whether a string contains text |
| `indexOf()` | Returns the first matching index |
| `lastIndexOf()` | Returns the last matching index |
| `startsWith()` | Checks the beginning of a string |
| `endsWith()` | Checks the ending of a string |
| `slice()` | Extracts part of a string |
| `substring()` | Extracts part of a string |
| `split()` | Splits a string into an array |
| `replace()` | Replaces the first matching occurrence |
| `replaceAll()` | Replaces all matching occurrences |
| `toLowerCase()` | Converts to lowercase |
| `toUpperCase()` | Converts to uppercase |
| `trim()` | Removes whitespace from both ends |
| `trimStart()` | Removes whitespace from the beginning |
| `trimEnd()` | Removes whitespace from the end |
| `padStart()` | Pads the beginning of a string |
| `padEnd()` | Pads the end of a string |
| `repeat()` | Repeats a string |
| `match()` | Matches a string against a regular expression |
| `matchAll()` | Returns all regular expression matches |
| `search()` | Searches using a regular expression |
| `localeCompare()` | Compares strings according to locale rules |

Detailed coverage of these methods is available in the `10-STRINGS-AND-REGEX/` section of the JavaScript curriculum.

# 🗁 Practical Examples

**Formatting a User's Full Name:**

```javascript
const firstName = "Alice";
const lastName = "Smith";

const fullName = `${firstName} ${lastName}`;

console.log(fullName);
```

**Creating a Multiline Message:**

```javascript
const message = `
    Welcome to JavaScript!

    Learn programming step by step.
    Practice every day.
`;

console.log(message);
```

**Checking Whether a String Contains Text:**

```javascript
const email = "alice@example.com";

if (email.includes("@")) {
    console.log("Email contains @");
}
```

**Removing Unnecessary Whitespace:**

```javascript
const username = "   alice123   ";

const cleanedUsername = username.trim();

console.log(cleanedUsername); // alice123
```

**Formatting a Number as Text:**

```javascript
const invoiceNumber = 42;

const formattedInvoice = String(invoiceNumber).padStart(5, "0");

console.log(formattedInvoice); // 00042
```

# 🗁 Best Practices

* Use consistent quotation marks throughout a project.
* Prefer template literals when interpolating variables.
* Remember that strings are immutable.
* Use `===` for exact string comparisons.
* Use `String()` when converting potentially nullish values into strings.
* Use `trim()` when cleaning user-provided text.
* Consider Unicode and UTF-16 behavior when processing international text.
* Use `Intl.Segmenter` when user-perceived character boundaries matter.
* Avoid repeated string concatenation inside performance-critical loops when a more suitable approach is available.
* Validate and normalize user input according to the application's requirements.
* Never assume that string length always equals the number of visible characters.

# 🗁 Common Mistakes

**Using Parentheses with the `length` Property**

Incorrect:

```javascript
const message = "Hello";

console.log(message.length()); // TypeError
```

Correct:

```javascript
console.log(message.length); // 5
```

**Trying to Modify a Character Directly**

Incorrect:

```javascript
let name = "Alice";

name[0] = "M";

console.log(name); // Alice
```

Correct:

```javascript
let name = "Alice";

name = "M" + name.slice(1);

console.log(name); // Mlice
```

**Forgetting Case Sensitivity**

```javascript
const password = "Secret123";

console.log(password === "secret123"); // false
```

String comparisons are case-sensitive.

**Assuming `length` Counts Visible Characters**

```javascript
const emoji = "😀";

console.log(emoji.length); // 2
```

The string contains one Unicode code point but two UTF-16 code units.

**Calling `toString()` on Nullish Values**

Incorrect:

```javascript
const value = null;

// Throws TypeError
// console.log(value.toString());
```

Correct:

```javascript
console.log(String(value)); // "null"
```

# 🗁 Summary

* Strings represent textual data in JavaScript.
* Strings can use single quotes, double quotes, or template literals.
* The `length` property returns the number of UTF-16 code units.
* String indexing starts at zero.
* The `at()` method supports negative indexes.
* Strings are immutable.
* Escape sequences represent special characters.
* String concatenation combines multiple strings.
* Template literals support interpolation and multiline text.
* JavaScript strings use UTF-16 encoding.
* `typeof` returns `"string"` for string values.
* JavaScript provides many built-in methods for string manipulation.

# 🗁 Practice

**Exercise: Create Strings**

Create three variables using:

* Single quotes.
* Double quotes.
* Template literals.

Print all three values.

**Exercise: String Length**

Predict the output:

```javascript
const first = "JavaScript";
const second = "Hello World";
const third = "😀";

console.log(first.length);
console.log(second.length);
console.log(third.length);
```

Explain why the third result is different from the number of visible characters.

**Exercise: Character Access**

Predict the output:

```javascript
const language = "JavaScript";

console.log(language[0]);
console.log(language[4]);
console.log(language.at(-1));
console.log(language.at(-3));
```

**Exercise: String Immutability**

Predict the output:

```javascript
let message = "hello";

message.toUpperCase();

console.log(message);

message = message.toUpperCase();

console.log(message);
```

**Exercise: String Concatenation**

Create variables for a first name and last name. Combine them into a full name using:

* The `+` operator.
* The `+=` operator.
* Template literals.

**Exercise: Escape Characters**

Create a string containing:

* A double quote.
* A single quote.
* A new line.
* A tab.
* A backslash.

Print the result.

**Exercise: Unicode**

Predict the output:

```javascript
const text = "A😀B";

console.log(text.length);
console.log([...text].length);
console.log(text.codePointAt(1));
```

Explain the difference between UTF-16 code units and Unicode code points.

**Exercise: Practical String Processing**

Create a function named `formatUsername` that:

* Accepts a username.
* Removes whitespace from both ends.
* Converts it to lowercase.
* Returns the cleaned username.

Example:

```javascript
console.log(formatUsername("  ALICE123  "));

// alice123
```
