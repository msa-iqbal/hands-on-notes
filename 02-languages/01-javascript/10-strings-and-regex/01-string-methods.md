# JavaScript String Methods

JavaScript strings are immutable sequences of characters used to represent text.

JavaScript provides many built-in string methods for inspecting, searching, extracting, transforming, and manipulating strings.

String methods are available through the `String` primitive and its associated methods.

Common use cases include:

* Validating user input.
* Formatting text.
* Searching for specific content.
* Extracting portions of strings.
* Replacing text.
* Normalizing user input.
* Building dynamic messages.
* Processing API responses.

# 🗁 String Immutability

JavaScript strings are immutable.

Once a string is created, its contents cannot be changed directly. String methods return new strings rather than modifying the original string.

```javascript
const message = "Hello World";

const result = message.toUpperCase();

console.log(result);
// HELLO WORLD

console.log(message);
// Hello World
```

Even methods that appear to modify strings produce new string values.

# 🗁 String Length

### 🖆 `length`

The `length` property returns the number of UTF-16 code units in a string.

```javascript
const message = "JavaScript";

console.log(message.length);
// 10
```

It is a property, not a method. Do not use parentheses.

```javascript
console.log(message.length);
// Correct

console.log(message.length());
// TypeError
```

For many common characters, the length corresponds to the number of characters. However, some Unicode characters occupy multiple UTF-16 code units.

```javascript
const emoji = "😀";

console.log(emoji.length);
// 2
```

To count Unicode code points instead:

```javascript
console.log([...emoji].length);
// 1
```

# 🗁 Changing Letter Case

### 🖆 `toUpperCase()`

Converts all letters in a string to uppercase.

```javascript
const message = "Hello JavaScript";

console.log(message.toUpperCase());
// HELLO JAVASCRIPT
```

### 🖆 `toLowerCase()`

Converts all letters in a string to lowercase.

```javascript
const message = "HELLO JAVASCRIPT";

console.log(message.toLowerCase());
// hello javascript
```

### 🖆 `toLocaleUpperCase()`

Converts a string to uppercase using locale-specific casing rules.

```javascript
const text = "istanbul";

console.log(text.toLocaleUpperCase("tr"));
// ISTANBUL
```

Locale-specific casing can differ from ordinary Unicode casing.

### 🖆 `toLocaleLowerCase()`

Converts a string to lowercase using locale-specific casing rules.

```javascript
const text = "ISTANBUL";

console.log(text.toLocaleLowerCase("tr"));
// ıstanbul
```

# 🗁 Removing Whitespace

### 🖆 `trim()`

Removes whitespace from both ends of a string.

```javascript
const username = "   alice   ";

console.log(username.trim());
// alice
```

It does not remove whitespace between words.

```javascript
const message = "  Hello World  ";

console.log(message.trim());
// Hello World
```

### 🖆 `trimStart()`

Removes whitespace from the beginning of a string.

```javascript
const message = "   Hello World   ";

console.log(message.trimStart());
// Hello World   
```

### 🖆 `trimEnd()`

Removes whitespace from the end of a string.

```javascript
const message = "   Hello World   ";

console.log(message.trimEnd());
//    Hello World
```

`trimLeft()` and `trimRight()` are legacy aliases for `trimStart()` and `trimEnd()` respectively. Prefer the modern method names.

# 🗁 Extracting String Content

JavaScript provides several methods for extracting portions of strings.

### 🖆 `slice()`

Extracts a portion of a string and returns it as a new string.

Syntax:

```javascript
string.slice(start, end);
```

* `start`: Starting index, inclusive.
* `end`: Ending index, exclusive.
* Negative indexes count backward from the end.

```javascript
const language = "JavaScript";

console.log(language.slice(0, 4));
// Java

console.log(language.slice(4));
// Script

console.log(language.slice(-6));
// Script

console.log(language.slice(0, -6));
// Java
```

The original string remains unchanged.

### 🖆 `substring()`

Extracts characters between two indexes.

Syntax:

```javascript
string.substring(start, end);
```

Unlike `slice()`:

* Negative arguments are treated as `0`.
* If `start` is greater than `end`, the arguments are swapped.

```javascript
const language = "JavaScript";

console.log(language.substring(0, 4));
// Java

console.log(language.substring(4, 0));
// Java

console.log(language.substring(-3, 4));
// Java
```

### 🖆 `substr()`

Extracts a specified number of characters starting at an index.

```javascript
const language = "JavaScript";

console.log(language.substr(4, 6));
// Script
```

`substr()` is a legacy method and is not part of the current ECMAScript standard. Avoid using it in new code.

### 🖆 Comparing `slice()`, `substring()`, and `substr()`

| Feature | `slice()` | `substring()` | `substr()` |
| --- | --- | --- | --- |
| Second argument | Ending index | Ending index | Character count |
| Negative start | Counts from end | Treated as `0` | Counts from end |
| Negative end | Counts from end | Treated as `0` | Not applicable |
| Swaps reversed indexes | No | Yes | Not applicable |
| Modern usage | Recommended | Supported | Legacy |

# 🗁 Accessing Individual Characters

### 🖆 `charAt()`

Returns the UTF-16 code unit at the specified index as a string.

```javascript
const language = "JavaScript";

console.log(language.charAt(0));
// J

console.log(language.charAt(4));
// S
```

If the index is outside the string, it returns an empty string.

```javascript
console.log(language.charAt(100));
// ""
```

### 🖆 `charCodeAt()`

Returns the UTF-16 code unit value at the specified index.

```javascript
const letter = "A";

console.log(letter.charCodeAt(0));
// 65
```

For characters represented by surrogate pairs, `charCodeAt()` returns only the code unit at the requested index.

### 🖆 `codePointAt()`

Returns the Unicode code point at the specified position.

```javascript
const emoji = "😀";

console.log(emoji.codePointAt(0));
// 128512
```

This method handles Unicode code points beyond the Basic Multilingual Plane.

### 🖆 Bracket Notation

You can access characters using string indexes.

```javascript
const language = "JavaScript";

console.log(language[0]);
// J

console.log(language[4]);
// S
```

Unlike `charAt()`, an invalid index returns `undefined`.

```javascript
console.log(language[100]);
// undefined
```

# 🗁 Repeating Strings

### 🖆 `repeat()`

Returns a new string containing the original string repeated a specified number of times.

```javascript
const symbol = "*";

console.log(symbol.repeat(5));
// *****
```

Example:

```javascript
const separator = "-".repeat(20);

console.log(separator);
// --------------------
```

The repeat count must be a non-negative integer after conversion. Negative counts and infinite results throw a `RangeError`.

# 🗁 Padding Strings

### 🖆 `padStart()`

Pads the beginning of a string until it reaches the specified length.

```javascript
const number = "7";

console.log(number.padStart(3, "0"));
// 007
```

Syntax:

```javascript
string.padStart(targetLength, padString);
```

### 🖆 `padEnd()`

Pads the end of a string until it reaches the specified length.

```javascript
const number = "7";

console.log(number.padEnd(3, "0"));
// 700
```

### 🖆 Practical Example

Formatting a numeric identifier:

```javascript
function formatId(id) {
    return String(id).padStart(5, "0");
}

console.log(formatId(42));
// 00042

console.log(formatId(125));
// 00125
```

# 🗁 Searching Within Strings

### 🖆 `includes()`

Checks whether a string contains a specified substring.

Returns `true` or `false`.

```javascript
const message = "Learning JavaScript";

console.log(message.includes("JavaScript"));
// true

console.log(message.includes("Python"));
// false
```

It is case-sensitive.

```javascript
console.log(message.includes("javascript"));
// false
```

### 🖆 `startsWith()`

Checks whether a string starts with a specified substring.

```javascript
const filename = "app.js";

console.log(filename.startsWith("app"));
// true

console.log(filename.startsWith("js"));
// false
```

### 🖆 `endsWith()`

Checks whether a string ends with a specified substring.

```javascript
const filename = "app.js";

console.log(filename.endsWith(".js"));
// true

console.log(filename.endsWith(".ts"));
// false
```

### 🖆 `indexOf()`

Returns the index of the first occurrence of a specified substring.

If the substring is not found, it returns `-1`.

```javascript
const message = "JavaScript is powerful";

console.log(message.indexOf("Script"));
// 4

console.log(message.indexOf("Python"));
// -1
```

### 🖆 `lastIndexOf()`

Returns the index of the last occurrence of a specified substring.

```javascript
const message = "JavaScript is great. JavaScript is popular.";

console.log(message.lastIndexOf("JavaScript"));
// 21
```

### 🖆 `search()`

Searches a string using a regular expression and returns the index of the first match.

```javascript
const message = "JavaScript 2026";

console.log(message.search(/\d/));
// 11
```

If no match is found, it returns `-1`.

Unlike `indexOf()`, `search()` accepts regular expressions rather than ordinary substring search arguments.

# 🗁 Replacing String Content

### 🖆 `replace()`

Replaces the first matching occurrence of a substring or regular expression.

```javascript
const message = "I like JavaScript";

const result = message.replace("JavaScript", "TypeScript");

console.log(result);
// I like TypeScript
```

By default, a string search argument replaces only the first occurrence.

```javascript
const message = "apple apple apple";

console.log(message.replace("apple", "orange"));
// orange apple apple
```

### 🖆 `replaceAll()`

Replaces all matching occurrences.

```javascript
const message = "apple apple apple";

console.log(message.replaceAll("apple", "orange"));
// orange orange orange
```

It can also work with regular expressions.

```javascript
const message = "apple apple apple";

console.log(message.replaceAll(/apple/g, "orange"));
// orange orange orange
```

When using a regular expression with `replaceAll()`, the global flag is required.

### 🖆 Using a Replacement Function

The replacement argument can be a callback function.

```javascript
const message = "Price: 100";

const result = message.replace(/\d+/, (match) => {
    return Number(match) * 2;
});

console.log(result);
// Price: 200
```

The callback receives information about the match and can calculate a replacement dynamically.

# 🗁 Splitting Strings

### 🖆 `split()`

Splits a string into an array using a specified separator.

Syntax:

```javascript
string.split(separator, limit);
```

Example:

```javascript
const technologies = "HTML,CSS,JavaScript";

const result = technologies.split(",");

console.log(result);
// ["HTML", "CSS", "JavaScript"]
```

### 🖆 Splitting by Whitespace

```javascript
const message = "JavaScript is powerful";

console.log(message.split(" "));
// ["JavaScript", "is", "powerful"]
```

### 🖆 Splitting into Individual UTF-16 Code Units

```javascript
const language = "JavaScript";

console.log(language.split(""));
// ["J", "a", "v", "a", "S", "c", "r", "i", "p", "t"]
```

For Unicode code points, use the spread operator:

```javascript
const emoji = "😀";

console.log([...emoji]);
// ["😀"]
```

### 🖆 Using the Limit Argument

The optional `limit` argument controls the maximum number of array elements.

```javascript
const message = "HTML,CSS,JavaScript,TypeScript";

console.log(message.split(",", 2));
// ["HTML", "CSS"]
```

# 🗁 Concatenating Strings

### 🖆 `concat()`

Combines strings and returns a new string.

```javascript
const firstName = "Muhammad";
const lastName = "Iqbal";

const fullName = firstName.concat(" ", lastName);

console.log(fullName);
// Muhammad Iqbal
```

Multiple values can be provided.

```javascript
const result = "Hello".concat(" ", "World", "!");

console.log(result);
// Hello World!
```

In modern JavaScript, template literals and the `+` operator are often more convenient.

```javascript
const firstName = "Muhammad";
const lastName = "Iqbal";

const fullName = `${firstName} ${lastName}`;
```

# 🗁 Comparing Strings

### 🖆 `localeCompare()`

Compares two strings according to locale-specific collation rules.

It returns:

* A negative number if the reference string comes before the compared string.
* `0` if the strings are equivalent under the selected comparison rules.
* A positive number if the reference string comes after the compared string.

```javascript
console.log("apple".localeCompare("banana"));
// Negative number

console.log("banana".localeCompare("apple"));
// Positive number

console.log("apple".localeCompare("apple"));
// 0
```

The exact negative or positive number is not guaranteed to be `-1` or `1`.

### 🖆 Locale-Aware Comparison

```javascript
const names = ["Zara", "Alice", "Bob"];

names.sort((a, b) => a.localeCompare(b));

console.log(names);
// ["Alice", "Bob", "Zara"]
```

Locale-aware comparison is useful for sorting human-readable text.

# 🗁 Unicode Normalization

### 🖆 `normalize()`

Returns a Unicode-normalized version of a string.

Unicode allows visually identical text to be represented using different underlying character sequences.

For example, an accented character can be represented as:

* A single precomposed character.
* A base character followed by a combining accent.

These representations may look identical but contain different code points.

```javascript
const first = "\u00E9";
const second = "\u0065\u0301";

console.log(first === second);
// false
```

Normalize both strings:

```javascript
console.log(first.normalize() === second.normalize());
// true
```

The supported normalization forms are:

| Form | Description |
| --- | --- |
| `NFC` | Canonical decomposition followed by composition |
| `NFD` | Canonical decomposition |
| `NFKC` | Compatibility decomposition followed by composition |
| `NFKD` | Compatibility decomposition |

Example:

```javascript
const text = "é";

console.log(text.normalize("NFD"));
```

Normalization is useful when comparing or processing Unicode text consistently.

# 🗁 String Conversion Methods

### 🖆 `toString()`

Returns the string representation of a value.

```javascript
const value = 123;

console.log(value.toString());
// "123"
```

For a string primitive:

```javascript
const message = "JavaScript";

console.log(message.toString());
// JavaScript
```

### 🖆 `String()`

The global `String()` function converts a value to a string.

```javascript
console.log(String(123));
// "123"

console.log(String(true));
// "true"

console.log(String(null));
// "null"

console.log(String(undefined));
// "undefined"
```

Unlike calling `.toString()` directly on `null` or `undefined`, `String()` handles both values.

### 🖆 `valueOf()`

Returns the primitive value represented by a String object.

```javascript
const message = new String("JavaScript");

console.log(message.valueOf());
// JavaScript

console.log(typeof message);
// object

console.log(typeof message.valueOf());
// string
```

String objects created with `new String()` are different from string primitives. Prefer primitive strings in normal application code.

# 🗁 String Iteration

Strings are iterable, allowing them to be processed with `for...of`.

```javascript
const language = "JavaScript";

for (const character of language) {
    console.log(character);
}
```

Unlike indexing by UTF-16 code units, string iteration processes Unicode code points.

```javascript
const emoji = "😀";

for (const character of emoji) {
    console.log(character);
}

// 😀
```

# 🗁 Practical String Processing Examples

### 🖆 Normalizing User Input

```javascript
function normalizeUsername(username) {
    return username
        .trim()
        .toLowerCase();
}

console.log(normalizeUsername("  ALICE  "));
// alice
```

### 🖆 Formatting a Full Name

```javascript
function formatName(firstName, lastName) {
    const first = firstName.trim();
    const last = lastName.trim();

    return `${first} ${last}`;
}

console.log(formatName(" Muhammad ", " Iqbal "));
// Muhammad Iqbal
```

### 🖆 Checking a File Extension

```javascript
function isJavaScriptFile(filename) {
    return filename.toLowerCase().endsWith(".js");
}

console.log(isJavaScriptFile("app.js"));
// true

console.log(isJavaScriptFile("APP.JS"));
// true

console.log(isJavaScriptFile("app.ts"));
// false
```

### 🖆 Counting a Character's Occurrences

```javascript
function countOccurrences(text, character) {
    return text.split(character).length - 1;
}

console.log(countOccurrences("banana", "a"));
// 3
```

This simple approach works for non-empty separators. For more complex matching requirements, regular expressions may be more appropriate.

### 🖆 Capitalizing the First Letter

```javascript
function capitalize(text) {
    if (!text) {
        return text;
    }

    return text.charAt(0).toUpperCase() + text.slice(1);
}

console.log(capitalize("javascript"));
// Javascript
```

This example handles ordinary English text. Full Unicode-aware capitalization can require locale-sensitive and language-specific processing.

# 🗁 Best Practices

* Remember that strings are immutable.
* Use `trim()` to remove unwanted surrounding whitespace.
* Use `toLowerCase()` or `toUpperCase()` for simple case normalization.
* Use `slice()` for extracting string portions.
* Prefer `includes()` when checking whether a substring exists.
* Use `startsWith()` and `endsWith()` for prefix and suffix checks.
* Use `replaceAll()` when every matching occurrence must be replaced.
* Use `split()` when converting delimited text into an array.
* Prefer template literals for readable string interpolation.
* Use `localeCompare()` for locale-aware sorting.
* Use `normalize()` when Unicode normalization is required.
* Be aware that `length` counts UTF-16 code units, not necessarily user-perceived characters.
* Avoid legacy methods such as `substr()` in new code.
* Prefer primitive strings over `new String()` objects.

# 🗁 Summary

JavaScript provides a rich collection of string methods for working with text.

| Method or Property | Purpose |
| --- | --- |
| `length` | Returns the number of UTF-16 code units |
| `toUpperCase()` | Converts to uppercase |
| `toLowerCase()` | Converts to lowercase |
| `trim()` | Removes whitespace from both ends |
| `trimStart()` | Removes leading whitespace |
| `trimEnd()` | Removes trailing whitespace |
| `slice()` | Extracts a substring |
| `substring()` | Extracts characters between indexes |
| `charAt()` | Returns a character at an index |
| `charCodeAt()` | Returns a UTF-16 code unit |
| `codePointAt()` | Returns a Unicode code point |
| `repeat()` | Repeats a string |
| `padStart()` | Pads the beginning |
| `padEnd()` | Pads the end |
| `includes()` | Checks for a substring |
| `startsWith()` | Checks a prefix |
| `endsWith()` | Checks a suffix |
| `indexOf()` | Finds the first occurrence |
| `lastIndexOf()` | Finds the last occurrence |
| `search()` | Searches using a regular expression |
| `replace()` | Replaces the first match |
| `replaceAll()` | Replaces all matches |
| `split()` | Splits a string into an array |
| `concat()` | Combines strings |
| `localeCompare()` | Performs locale-aware comparison |
| `normalize()` | Normalizes Unicode text |
| `toString()` | Returns a string representation |
| `valueOf()` | Returns the primitive string value |

**Remember:** String methods provide powerful text-processing capabilities, but they do not modify the original string. Always use the returned value when applying a transformation.
