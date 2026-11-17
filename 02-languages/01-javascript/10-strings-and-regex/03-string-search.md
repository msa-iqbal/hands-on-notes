# JavaScript String Search

String searching is the process of finding specific characters, words, or patterns within a string.

JavaScript provides several built-in methods for searching strings, including:

* `includes()`
* `startsWith()`
* `endsWith()`
* `indexOf()`
* `lastIndexOf()`
* `search()`
* `match()`
* `matchAll()`

These methods are useful for validating input, locating text, checking file extensions, finding patterns, and processing user-provided content.

# 🗁 Searching for Substrings

### 🖆 `includes()`

The `includes()` method checks whether a string contains a specified substring.

It returns a boolean:

* `true` if the substring exists.
* `false` if the substring does not exist.

Syntax:

```javascript
string.includes(searchString, position);
```

Parameters:

* `searchString`: The substring to search for.
* `position`: Optional starting position for the search. Defaults to `0`.

Example:

```javascript
const message = "Learning JavaScript is interesting";

console.log(message.includes("JavaScript"));
// true

console.log(message.includes("Python"));
// false
```

### 🖆 Case-Sensitive Searching

`includes()` is case-sensitive.

```javascript
const message = "JavaScript Programming";

console.log(message.includes("JavaScript"));
// true

console.log(message.includes("javascript"));
// false
```

For case-insensitive searching, normalize both strings.

```javascript
const message = "JavaScript Programming";

const result = message
    .toLowerCase()
    .includes("javascript".toLowerCase());

console.log(result);
// true
```

### 🖆 Searching from a Specific Position

```javascript
const message = "JavaScript is powerful";

console.log(message.includes("JavaScript", 1));
// false

console.log(message.includes("is", 11));
// true
```

The search begins at the specified string index.

# 🗁 Checking String Prefixes

### 🖆 `startsWith()`

The `startsWith()` method checks whether a string begins with a specified substring.

Syntax:

```javascript
string.startsWith(searchString, position);
```

Parameters:

* `searchString`: The substring to search for.
* `position`: Optional position where the search begins. Defaults to `0`.

Example:

```javascript
const filename = "application.js";

console.log(filename.startsWith("application"));
// true

console.log(filename.startsWith("app"));
// true

console.log(filename.startsWith("js"));
// false
```

### 🖆 Searching from a Specific Position

```javascript
const message = "JavaScript Programming";

console.log(message.startsWith("Script", 4));
// true
```

The search begins at index `4`.

### 🖆 Practical Example: Checking URL Protocols

```javascript
function isSecureUrl(url) {
    return url.startsWith("https://");
}

console.log(isSecureUrl("https://example.com"));
// true

console.log(isSecureUrl("http://example.com"));
// false
```

For production URL validation, prefer the `URL` API and inspect its `protocol` property rather than relying only on a string prefix.

# 🗁 Checking String Suffixes

### 🖆 `endsWith()`

The `endsWith()` method checks whether a string ends with a specified substring.

Syntax:

```javascript
string.endsWith(searchString, endPosition);
```

Parameters:

* `searchString`: The substring to search for.
* `endPosition`: Optional position where the string is treated as ending. Defaults to the string's length.

Example:

```javascript
const filename = "application.js";

console.log(filename.endsWith(".js"));
// true

console.log(filename.endsWith(".ts"));
// false
```

### 🖆 Using the End Position

```javascript
const message = "JavaScript Programming";

console.log(message.endsWith("JavaScript", 10));
// true
```

The method checks whether the substring ends at or before the specified position.

### 🖆 Practical Example: File Extension Checking

```javascript
function isImageFile(filename) {
    const supportedExtensions = [
        ".jpg",
        ".jpeg",
        ".png",
        ".webp"
    ];

    return supportedExtensions.some(extension =>
        filename.toLowerCase().endsWith(extension)
    );
}

console.log(isImageFile("photo.png"));
// true

console.log(isImageFile("document.pdf"));
// false
```

# 🗁 Finding String Positions

### 🖆 `indexOf()`

The `indexOf()` method returns the index of the first occurrence of a specified substring.

If the substring is not found, it returns `-1`.

Syntax:

```javascript
string.indexOf(searchValue, fromIndex);
```

Parameters:

* `searchValue`: The substring to find.
* `fromIndex`: Optional position where the search begins.

Example:

```javascript
const message = "JavaScript is powerful";

console.log(message.indexOf("Script"));
// 4

console.log(message.indexOf("is"));
// 11

console.log(message.indexOf("Python"));
// -1
```

### 🖆 Finding the First Occurrence

```javascript
const text = "apple orange apple banana";

console.log(text.indexOf("apple"));
// 0
```

### 🖆 Searching from a Specific Position

```javascript
const text = "apple orange apple banana";

console.log(text.indexOf("apple", 1));
// 13
```

The first occurrence is skipped because the search starts at index `1`.

### 🖆 Finding the Last Occurrence with `indexOf()`

```javascript
const text = "apple orange apple banana";

const firstIndex = text.indexOf("apple");
const secondIndex = text.indexOf("apple", firstIndex + 1);

console.log(firstIndex);
// 0

console.log(secondIndex);
// 13
```

This approach can be used to locate successive occurrences.

# 🗁 Finding the Last Occurrence

### 🖆 `lastIndexOf()`

The `lastIndexOf()` method returns the index of the last occurrence of a specified substring.

Syntax:

```javascript
string.lastIndexOf(searchValue, fromIndex);
```

Parameters:

* `searchValue`: The substring to find.
* `fromIndex`: Optional position from which the backward search begins.

Example:

```javascript
const text = "apple orange apple banana";

console.log(text.lastIndexOf("apple"));
// 13
```

### 🖆 Searching from a Specific Position

```javascript
const text = "JavaScript is powerful. JavaScript is popular.";

console.log(text.lastIndexOf("JavaScript", 20));
// 0
```

The search considers occurrences ending at or before the specified starting position.

### 🖆 Finding a File Extension Position

```javascript
const filename = "archive.backup.zip";

const extensionPosition = filename.lastIndexOf(".");

console.log(extensionPosition);
// 14

console.log(filename.slice(extensionPosition));
// .zip
```

# 🗁 Searching with Regular Expressions

### 🖆 `search()`

The `search()` method searches a string using a regular expression and returns the index of the first match.

Syntax:

```javascript
string.search(regexp);
```

Example:

```javascript
const message = "JavaScript 2026";

console.log(message.search(/\d/));
// 11
```

The regular expression `/\d/` matches the first digit.

### 🖆 Searching for Words

```javascript
const message = "Learning JavaScript is fun";

console.log(message.search(/JavaScript/));
// 9
```

### 🖆 Searching Case-Insensitively

Use the `i` flag to perform a case-insensitive search.

```javascript
const message = "Learning JavaScript";

console.log(message.search(/javascript/i));
// 9
```

### 🖆 Searching for Multiple Character Types

```javascript
const message = "Order number: 12345";

console.log(message.search(/\d+/));
// 15
```

The `\d+` pattern matches one or more digits.

### 🖆 No Match Found

```javascript
const message = "JavaScript";

console.log(message.search(/Python/));
// -1
```

# 🗁 Extracting Search Matches

### 🖆 `match()`

The `match()` method retrieves matches from a string using a regular expression.

Syntax:

```javascript
string.match(regexp);
```

Example:

```javascript
const message = "Order number: 12345";

const result = message.match(/\d+/);

console.log(result);
// ["12345"]
```

The returned value is an array when a match exists, or `null` when no match is found.

### 🖆 Matching a Single Occurrence

```javascript
const text = "apple orange apple";

const result = text.match(/apple/);

console.log(result);
// ["apple"]

console.log(result.index);
// 0
```

Without the global flag, `match()` returns information about the first match.

### 🖆 Matching All Occurrences with the Global Flag

```javascript
const text = "apple orange apple banana apple";

const result = text.match(/apple/g);

console.log(result);
// ["apple", "apple", "apple"]
```

With the global flag, `match()` returns an array containing all matched substrings.

### 🖆 Case-Insensitive Matching

```javascript
const text = "JavaScript javascript JAVASCRIPT";

const result = text.match(/javascript/gi);

console.log(result);
// ["JavaScript", "javascript", "JAVASCRIPT"]
```

### 🖆 Handling No Matches

```javascript
const text = "JavaScript";

const result = text.match(/Python/);

console.log(result);
// null
```

Handle the possibility of `null` before accessing match results.

```javascript
const matches = text.match(/Python/);

if (matches) {
    console.log(matches[0]);
} else {
    console.log("No matches found");
}
```

# 🗁 Iterating Over All Matches

### 🖆 `matchAll()`

The `matchAll()` method returns an iterator containing all matches of a regular expression.

It is particularly useful when you need match positions, captured groups, and other match details for every occurrence.

Syntax:

```javascript
string.matchAll(regexp);
```

Example:

```javascript
const text = "apple orange apple banana apple";

const matches = text.matchAll(/apple/g);

for (const match of matches) {
    console.log(match[0], match.index);
}
```

Output:

```text
apple 0
apple 13
apple 27
```

### 🖆 Accessing Captured Groups

```javascript
const text = "User: Alice, Age: 25";

const matches = text.matchAll(/(\w+): (\w+)/g);

for (const match of matches) {
    console.log(match[0]);
    console.log(match[1]);
    console.log(match[2]);
}
```

The capturing groups provide additional information about each match.

### 🖆 Converting the Iterator into an Array

```javascript
const text = "apple orange apple";

const matches = [...text.matchAll(/apple/g)];

console.log(matches.length);
// 2

console.log(matches[1].index);
// 13
```

`matchAll()` requires a regular expression with the global (`g`) flag. Without it, JavaScript throws a `TypeError`.

Unlike `match()`, `matchAll()` returns an iterator rather than a regular array.

# 🗁 String Search Method Comparison

| Method | Purpose | Return Value |
| --- | --- | --- |
| `includes()` | Checks whether a substring exists | Boolean |
| `startsWith()` | Checks the beginning of a string | Boolean |
| `endsWith()` | Checks the ending of a string | Boolean |
| `indexOf()` | Finds the first occurrence | Number |
| `lastIndexOf()` | Finds the last occurrence | Number |
| `search()` | Finds the first regular expression match | Number |
| `match()` | Retrieves regular expression matches | Array or `null` |
| `matchAll()` | Retrieves all matches with details | Iterator |

### 🖆 Choosing the Right Method

Use `includes()` when you only need to know whether text exists.

```javascript
const hasKeyword = message.includes("JavaScript");
```

Use `startsWith()` when checking a prefix.

```javascript
const isApiPath = "/api/users".startsWith("/api");
```

Use `endsWith()` when checking a suffix.

```javascript
const isJavaScript = "app.js".endsWith(".js");
```

Use `indexOf()` when you need the position of a substring.

```javascript
const position = message.indexOf("JavaScript");
```

Use `search()` when you need the position of a regular expression match.

```javascript
const position = message.search(/\d+/);
```

Use `match()` when you need matching text.

```javascript
const numbers = "Order 123".match(/\d+/);
```

Use `matchAll()` when you need every match and its associated details.

```javascript
const matches = [..."a1 b2 c3".matchAll(/\d/g)];
```

# 🗁 Practical String Search Examples

### 🖆 Searching for a Keyword

```javascript
function containsKeyword(text, keyword) {
    return text
        .toLowerCase()
        .includes(keyword.toLowerCase());
}

console.log(
    containsKeyword("Learning JavaScript", "javascript")
);
// true
```

### 🖆 Finding All Occurrences of a Word

```javascript
function findOccurrences(text, word) {
    const matches = [...text.matchAll(
        new RegExp(word, "gi")
    )];

    return matches.map(match => match.index);
}

console.log(
    findOccurrences("JavaScript is great. JavaScript is popular.", "JavaScript")
);
// [0, 21]
```

This example treats `word` as a regular expression pattern. If the input is arbitrary user-provided text, escape regular expression metacharacters before constructing the expression.

### 🖆 Extracting Email Addresses

```javascript
const message = `
    Contact alice@example.com
    or support@example.org
`;

const emails = message.match(
    /[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g
);

console.log(emails);
```

This is a basic extraction pattern, not a complete email validation rule.

### 🖆 Finding Numbers in Text

```javascript
const message = "Products cost 100, 250, and 500 dollars.";

const numbers = message.match(/\d+/g);

console.log(numbers);
// ["100", "250", "500"]
```

### 🖆 Checking Whether a String Contains Only Digits

```javascript
function containsOnlyDigits(value) {
    return /^\d+$/.test(value);
}

console.log(containsOnlyDigits("12345"));
// true

console.log(containsOnlyDigits("123abc"));
// false
```

The `^` and `$` anchors require the entire string to match the pattern.

### 🖆 Finding the Last File Extension

```javascript
function getExtension(filename) {
    const position = filename.lastIndexOf(".");

    if (position === -1) {
        return "";
    }

    return filename.slice(position + 1);
}

console.log(getExtension("image.png"));
// png

console.log(getExtension("archive.backup.zip"));
// zip

console.log(getExtension("README"));
// ""
```

# 🗁 Common Mistakes

### 🖆 Confusing `indexOf()` with `includes()`

```javascript
const text = "JavaScript";

console.log(text.includes("Java"));
// true

console.log(text.indexOf("Java"));
// 0
```

`includes()` returns a boolean, while `indexOf()` returns a position or `-1`.

### 🖆 Incorrectly Checking `indexOf()` Results

Incorrect:

```javascript
if (text.indexOf("Java")) {
    console.log("Found");
}
```

This fails when the match starts at index `0`, because `0` is falsy.

Correct:

```javascript
if (text.indexOf("Java") !== -1) {
    console.log("Found");
}
```

Alternatively:

```javascript
if (text.includes("Java")) {
    console.log("Found");
}
```

### 🖆 Forgetting That Search Is Case-Sensitive

```javascript
const text = "JavaScript";

console.log(text.includes("javascript"));
// false
```

Normalize the strings when case-insensitive matching is required.

### 🖆 Accessing a Missing Match

Incorrect:

```javascript
const result = "Hello World".match(/Python/);

console.log(result[0]);
// TypeError
```

Correct:

```javascript
const result = "Hello World".match(/Python/);

if (result !== null) {
    console.log(result[0]);
}
```

### 🖆 Using `matchAll()` Without the Global Flag

Incorrect:

```javascript
const matches = "a1 b2".matchAll(/\d/);
```

Correct:

```javascript
const matches = "a1 b2".matchAll(/\d/g);
```

### 🖆 Assuming `search()` Returns the Matched Text

```javascript
const text = "Order 12345";

console.log(text.search(/\d+/));
// 6
```

`search()` returns the match position, not the matched text. Use `match()` or `matchAll()` when the actual matched content is needed.

# 🗁 Best Practices

* Use `includes()` for simple substring existence checks.
* Use `startsWith()` and `endsWith()` for prefix and suffix validation.
* Use `indexOf()` and `lastIndexOf()` when positions matter.
* Use `search()` for locating regular expression matches.
* Use `match()` to retrieve matched content.
* Use `matchAll()` when every match and its details are required.
* Remember that string searches are generally case-sensitive unless explicitly configured otherwise.
* Check for `null` before accessing the result of `match()`.
* Use the global flag with `matchAll()`.
* Escape dynamic input before using it as a regular expression pattern.
* Use regular expressions when the search requirement involves patterns rather than fixed substrings.

# 🗁 Summary

JavaScript provides several methods for searching strings and retrieving matching content.

Key points:

* `includes()` checks whether a substring exists.
* `startsWith()` checks whether a string begins with specific text.
* `endsWith()` checks whether a string ends with specific text.
* `indexOf()` finds the first occurrence.
* `lastIndexOf()` finds the last occurrence.
* `search()` locates a regular expression match.
* `match()` retrieves regular expression matches.
* `matchAll()` retrieves all matches with their details.
* Most string searches are case-sensitive by default.
* Search methods return different types of values depending on their purpose.

**Remember:** Choose the simplest method that satisfies the search requirement. Use ordinary string methods for fixed text and regular expressions when you need pattern-based matching.
