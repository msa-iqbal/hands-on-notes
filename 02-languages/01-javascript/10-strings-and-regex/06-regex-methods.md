# JavaScript Regular Expression Methods

JavaScript provides several built-in methods for working with regular expressions and strings.

These methods allow developers to:

* Test whether a pattern exists in a string.
* Retrieve matching text.
* Find multiple matches.
* Replace matching text.
* Extract matched values.
* Split strings using patterns.

Regular expression operations mainly involve two built-in objects:

* `RegExp`: Provides methods for executing regular expressions.
* `String`: Provides methods that accept regular expressions as arguments.

# 🗁 RegExp Methods

The `RegExp` object provides methods for testing and executing patterns against strings.

### 🖆 `test()`

The `test()` method checks whether a regular expression matches a string.

**Syntax:**

```javascript
regex.test(string);
```

**Returns:** `true` or `false`.

```javascript
const pattern = /\d+/;

console.log(pattern.test("Order 123"));
// true

console.log(pattern.test("Order ABC"));
// false
```

The method returns `true` when at least one matching substring exists.

**Example: Validate a numeric string**

```javascript
const pattern = /^\d+$/;

console.log(pattern.test("12345"));
// true

console.log(pattern.test("123abc"));
// false
```

The `^` and `$` anchors ensure that the entire string contains only digits.

### 🖆 `exec()`

The `exec()` method executes a regular expression against a string and returns detailed information about the first match.

**Syntax:**

```javascript
regex.exec(string);
```

**Returns:**

* An array containing match information.
* `null` when no match is found.

```javascript
const pattern = /\d+/;

const result = pattern.exec("Order 123");

console.log(result[0]);
// 123

console.log(result.index);
// 6

console.log(result.input);
// Order 123
```

The returned array contains:

| Property | Description |
| --- | --- |
| `[0]` | Complete matched text |
| `[1]`, `[2]`, etc. | Captured groups |
| `index` | Starting position of the match |
| `input` | Original input string |
| `groups` | Named capturing groups, when present |

**Example: Capturing groups**

```javascript
const pattern = /(\d{4})-(\d{2})-(\d{2})/;

const result = pattern.exec("Date: 2026-11-14");

console.log(result[0]);
// 2026-11-14

console.log(result[1]);
// 2026

console.log(result[2]);
// 11

console.log(result[3]);
// 14
```

**Example: Named capturing groups**

```javascript
const pattern = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

const result = pattern.exec("2026-11-14");

console.log(result.groups.year);
// 2026

console.log(result.groups.month);
// 11

console.log(result.groups.day);
// 14
```

### 🖆 Global Matching with `exec()`

When a regular expression uses the global (`g`) flag, repeated calls to `exec()` return successive matches.

```javascript
const pattern = /\d+/g;

const text = "Order 123, Product 456, Quantity 789";

console.log(pattern.exec(text)[0]);
// 123

console.log(pattern.exec(text)[0]);
// 456

console.log(pattern.exec(text)[0]);
// 789

console.log(pattern.exec(text));
// null
```

The global regular expression maintains its position using `lastIndex`.

### 🖆 Sticky Matching with `exec()`

The sticky (`y`) flag requires each match to begin exactly at the current `lastIndex`.

```javascript
const text = "JavaScript";

const pattern = /Script/y;

pattern.lastIndex = 4;

const result = pattern.exec(text);

console.log(result[0]);
// Script
```

Unlike global matching, sticky matching does not search ahead for another possible match.

### 🖆 `lastIndex`

The `lastIndex` property stores the position from which the next global or sticky match begins.

```javascript
const pattern = /\d+/g;

const text = "123 456";

console.log(pattern.lastIndex);
// 0

console.log(pattern.exec(text)[0]);
// 123

console.log(pattern.lastIndex);
// 3

console.log(pattern.exec(text)[0]);
// 456

console.log(pattern.lastIndex);
// 7
```

When a global or sticky match fails, `lastIndex` resets to `0`.

You can manually reset it:

```javascript
const pattern = /\d+/g;

pattern.exec("123 456");

pattern.lastIndex = 0;

console.log(pattern.exec("123 456")[0]);
// 123
```

**Important:** Global and sticky regular expressions are stateful. Reusing them can produce unexpected results if their `lastIndex` is not considered.

# 🗁 String Methods with Regular Expressions

JavaScript string methods can accept regular expressions to perform advanced text operations.

### 🖆 `match()`

The `match()` method retrieves matches from a string using a regular expression.

**Syntax:**

```javascript
string.match(regex);
```

**Returns:**

* An array containing match information.
* `null` when no match is found.

**Example: Without the global flag**

```javascript
const text = "JavaScript is powerful";

const result = text.match(/JavaScript/);

console.log(result[0]);
// JavaScript

console.log(result.index);
// 0
```

Without the global flag, `match()` returns information about the first match, including capturing groups.

**Example: With the global flag**

```javascript
const text = "apple banana apple orange apple";

const result = text.match(/apple/g);

console.log(result);
// ["apple", "apple", "apple"]
```

With the global flag, `match()` returns an array of all matched strings without the detailed properties of an individual match.

**Example: Extract numbers**

```javascript
const text = "Products: 120, 250, 500";

const numbers = text.match(/\d+/g);

console.log(numbers);
// ["120", "250", "500"]
```

### 🖆 `matchAll()`

The `matchAll()` method returns an iterator containing all matches and their associated information.

**Syntax:**

```javascript
string.matchAll(regex);
```

It requires a global regular expression.

```javascript
const text = "JavaScript 101, TypeScript 202";

const pattern = /(\w+)\s(\d+)/g;

const matches = text.matchAll(pattern);

for (const match of matches) {
  console.log(match[0]);
  console.log(match[1]);
  console.log(match[2]);
}
```

Output:

```text
JavaScript 101
JavaScript
101
TypeScript 202
TypeScript
202
```

Each match includes:

* The complete matched text.
* Captured groups.
* The match index.
* The original input.
* Named groups, when available.

**Example: Convert matches into an array**

```javascript
const text = "Product 100, Product 200, Product 300";

const matches = [...text.matchAll(/Product (\d+)/g)];

const productIds = matches.map(match => match[1]);

console.log(productIds);
// ["100", "200", "300"]
```

`matchAll()` is particularly useful when you need multiple matches and their captured groups.

### 🖆 `search()`

The `search()` method returns the index of the first match.

**Syntax:**

```javascript
string.search(regex);
```

**Returns:**

* The starting index of the first match.
* `-1` when no match exists.

```javascript
const text = "Learn JavaScript today";

console.log(text.search(/JavaScript/));
// 6

console.log(text.search(/Python/));
// -1
```

The `search()` method returns only the first matching position, even if the regular expression has the global flag.

```javascript
const text = "apple banana apple";

console.log(text.search(/apple/g));
// 0
```

### 🖆 `replace()`

The `replace()` method replaces the first matching occurrence when using a string or a non-global regular expression.

**Syntax:**

```javascript
string.replace(searchValue, replaceValue);
```

**Example: Replace the first match**

```javascript
const text = "apple banana apple";

const result = text.replace(/apple/, "orange");

console.log(result);
// orange banana apple
```

**Example: Replace all matches**

```javascript
const text = "apple banana apple";

const result = text.replace(/apple/g, "orange");

console.log(result);
// orange banana orange
```

**Example: Case-insensitive replacement**

```javascript
const text = "JavaScript javascript JAVASCRIPT";

const result = text.replace(/javascript/gi, "TypeScript");

console.log(result);
// TypeScript TypeScript TypeScript
```

**Example: Replace using a callback function**

The replacement value can be a function.

```javascript
const text = "Price: 100, Discount: 20";

const result = text.replace(/\d+/g, match => {
  return Number(match) * 2;
});

console.log(result);
// Price: 200, Discount: 40
```

The callback receives information about each match, including captured groups and the match position.

### 🖆 Replacement Patterns

The replacement string supports special replacement patterns.

| Pattern | Description |
| --- | --- |
| `$$` | Inserts a literal `$` |
| `$&` | Inserts the complete matched text |
| `` $` `` | Inserts the portion before the match |
| `$'` | Inserts the portion after the match |
| `$1`, `$2`, etc. | Inserts captured groups |
| `$<name>` | Inserts a named captured group |

**Example: `$&`**

```javascript
const text = "JavaScript";

const result = text.replace(/Script/, "[$&]");

console.log(result);
// Java[Script]
```

**Example: Captured groups**

```javascript
const text = "2026-11-14";

const result = text.replace(
  /(\d{4})-(\d{2})-(\d{2})/,
  "$3/$2/$1"
);

console.log(result);
// 14/11/2026
```

**Example: Named captured groups**

```javascript
const text = "2026-11-14";

const result = text.replace(
  /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/,
  "$<day>/$<month>/$<year>"
);

console.log(result);
// 14/11/2026
```

### 🖆 `replaceAll()`

The `replaceAll()` method replaces every occurrence of a matching value.

**Syntax:**

```javascript
string.replaceAll(searchValue, replaceValue);
```

**Example: Replace all matching text**

```javascript
const text = "apple banana apple orange apple";

const result = text.replaceAll("apple", "mango");

console.log(result);
// mango banana mango orange mango
```

**Example: Using a regular expression**

```javascript
const text = "apple banana apple";

const result = text.replaceAll(/apple/g, "orange");

console.log(result);
// orange banana orange
```

When a regular expression is passed to `replaceAll()`, it must have the global (`g`) flag.

### 🖆 `split()`

The `split()` method divides a string into an array using a separator.

The separator can be a regular expression.

**Syntax:**

```javascript
string.split(separator);
```

**Example: Split using whitespace**

```javascript
const text = "JavaScript   TypeScript Python";

const words = text.split(/\s+/);

console.log(words);
// ["JavaScript", "TypeScript", "Python"]
```

**Example: Split using multiple delimiters**

```javascript
const text = "apple,banana;orange mango";

const result = text.split(/[,;\s]+/);

console.log(result);
// ["apple", "banana", "orange", "mango"]
```

**Example: Split while preserving captured delimiters**

Captured groups in the separator are included in the resulting array.

```javascript
const text = "apple,banana;orange";

const result = text.split(/([,;])/);

console.log(result);
// ["apple", ",", "banana", ";", "orange"]
```

# 🗁 Comparing Regular Expression Methods

Different methods solve different text-processing problems.

| Method | Called On | Purpose | Result |
| --- | --- | --- | --- |
| `test()` | RegExp | Check for a match | Boolean |
| `exec()` | RegExp | Execute a pattern | Array or `null` |
| `match()` | String | Retrieve matches | Array or `null` |
| `matchAll()` | String | Retrieve all detailed matches | Iterator |
| `search()` | String | Find first match position | Number |
| `replace()` | String | Replace matching text | String |
| `replaceAll()` | String | Replace all occurrences | String |
| `split()` | String | Divide text using a pattern | Array |

# 🗁 Practical Examples

### 🖆 Extract Email-Like Strings

```javascript
const text = `
Contact us at support@example.com
or sales@example.org.
`;

const pattern = /[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}/g;

const emails = text.match(pattern);

console.log(emails);
// ["support@example.com", "sales@example.org"]
```

This pattern extracts email-like strings but is not a complete implementation of email-address validation.

### 🖆 Extract URLs

```javascript
const text = `
Visit https://example.com
or http://developer.mozilla.org
`;

const pattern = /https?:\/\/[^\s]+/g;

const urls = text.match(pattern);

console.log(urls);
// ["https://example.com", "http://developer.mozilla.org"]
```

### 🖆 Remove Extra Whitespace

```javascript
const text = "  JavaScript    is   powerful  ";

const result = text
  .trim()
  .replace(/\s+/g, " ");

console.log(result);
// JavaScript is powerful
```

### 🖆 Convert a String to Slug Format

```javascript
function createSlug(value) {
  return value
    .toLowerCase()
    .trim()
    .replace(/[^\w\s-]/g, "")
    .replace(/\s+/g, "-")
    .replace(/-+/g, "-");
}

console.log(createSlug("JavaScript Regular Expressions!"));
// javascript-regular-expressions
```

This example uses ASCII-oriented word characters. More advanced slug generation may require Unicode normalization and language-specific character handling.

### 🖆 Extract Hashtags

```javascript
const text = "Learning #JavaScript and #TypeScript";

const hashtags = text.match(/#[A-Za-z0-9_]+/g);

console.log(hashtags);
// ["#JavaScript", "#TypeScript"]
```

### 🖆 Mask Sensitive Numbers

```javascript
const phone = "01712345678";

const masked = phone.replace(/\d(?=\d{4})/g, "*");

console.log(masked);
// *******5678
```

The lookahead ensures that the final four digits remain visible.

# 🗁 Important Considerations

### 🖆 Global and Sticky Regular Expressions Are Stateful

Methods such as `test()` and `exec()` update `lastIndex` when the regular expression has the `g` or `y` flag.

```javascript
const pattern = /apple/g;

console.log(pattern.test("apple"));
// true

console.log(pattern.test("apple"));
// false
```

Use a fresh regular expression or reset `lastIndex` when independent matching operations are required.

### 🖆 `match()` and `matchAll()` Behave Differently

```javascript
const text = "ID 123, ID 456";

console.log(text.match(/ID (\d+)/g));
// ["ID 123", "ID 456"]

console.log([...text.matchAll(/ID (\d+)/g)].map(match => match[1]));
// ["123", "456"]
```

Use `match()` for a simple list of matched strings. Use `matchAll()` when captured groups and match positions are important.

### 🖆 `replace()` Does Not Mutate the Original String

JavaScript strings are immutable. String methods return new strings.

```javascript
const original = "Hello World";

const updated = original.replace(/World/, "JavaScript");

console.log(original);
// Hello World

console.log(updated);
// Hello JavaScript
```

### 🖆 Prefer `replaceAll()` for Explicit Global Replacement

```javascript
const text = "one one one";

console.log(text.replaceAll("one", "two"));
// two two two
```

This makes the intention to replace every occurrence explicit.

# 🗁 Best Practices

* Use `test()` when you only need a boolean result.
* Use `exec()` when you need detailed information about a match.
* Use `match()` for simple extraction.
* Use `matchAll()` when multiple matches and their captured groups are required.
* Use `search()` when only the first matching index matters.
* Use `replace()` for targeted replacement.
* Use `replaceAll()` when every occurrence must be replaced.
* Use `split()` when dividing text with flexible delimiters.
* Be careful with `lastIndex` when using global and sticky patterns.
* Prefer named capturing groups for complex extraction.
* Avoid unnecessarily complex regular expressions.
* Test patterns with empty strings, unexpected characters, and boundary cases.

# 🗁 Summary

JavaScript provides two main categories of regular expression methods.

**RegExp methods:**

* `test()` checks whether a match exists.
* `exec()` retrieves detailed match information.
* `lastIndex` tracks the next matching position for global and sticky expressions.

**String methods:**

* `match()` retrieves matching text.
* `matchAll()` retrieves all matches with detailed information.
* `search()` finds the first matching position.
* `replace()` replaces matching text.
* `replaceAll()` replaces every occurrence.
* `split()` divides strings using a pattern.

Understanding these methods makes it easier to build reliable text-searching, extraction, validation, and transformation logic in JavaScript.
