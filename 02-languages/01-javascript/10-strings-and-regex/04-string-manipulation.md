# JavaScript String Manipulation

String manipulation is the process of modifying, transforming, extracting, and combining string values to produce the desired text.

JavaScript provides built-in string methods and operators that make text manipulation straightforward.

Common string manipulation tasks include:

* Removing unwanted whitespace.
* Changing letter casing.
* Replacing characters and words.
* Extracting substrings.
* Combining strings.
* Reversing strings.
* Removing duplicate characters.
* Formatting text.
* Normalizing user input.
* Processing Unicode characters.

# 🗁 String Immutability

JavaScript strings are immutable.

This means their contents cannot be changed directly after creation. String manipulation methods return new strings instead of modifying the original string.

```javascript
const message = "Hello World";

const result = message.toUpperCase();

console.log(result);
// HELLO WORLD

console.log(message);
// Hello World
```

To preserve a transformation, assign the returned value to a variable.

```javascript
let username = "  ALICE  ";

username = username.trim().toLowerCase();

console.log(username);
// alice
```

# 🗁 Changing Letter Case

### 🖆 `toUpperCase()`

Converts all letters in a string to uppercase.

```javascript
const message = "hello javascript";

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

Converts text to uppercase according to locale-specific casing rules.

```javascript
const text = "istanbul";

console.log(text.toLocaleUpperCase("tr"));
// ISTANBUL
```

### 🖆 `toLocaleLowerCase()`

Converts text to lowercase according to locale-specific casing rules.

```javascript
const text = "ISTANBUL";

console.log(text.toLocaleLowerCase("tr"));
// ıstanbul
```

# 🗁 Removing Whitespace

### 🖆 `trim()`

Removes whitespace from both ends of a string.

```javascript
const username = "   Alice   ";

console.log(username.trim());
// Alice
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

### 🖆 Removing All Whitespace

`trim()` only removes whitespace from the beginning and end.

To remove all whitespace characters, use `replace()` with a regular expression.

```javascript
const message = "JavaScript is powerful";

const result = message.replace(/\s/g, "");

console.log(result);
// JavaScriptispowerful
```

To replace consecutive whitespace with a single space:

```javascript
const message = "JavaScript    is   powerful";

const result = message.replace(/\s+/g, " ").trim();

console.log(result);
// JavaScript is powerful
```

# 🗁 Extracting Substrings

### 🖆 `slice()`

Extracts a portion of a string.

```javascript
const language = "JavaScript";

console.log(language.slice(0, 4));
// Java

console.log(language.slice(4));
// Script

console.log(language.slice(-6));
// Script
```

The ending index is exclusive.

### 🖆 `substring()`

Extracts characters between two indexes.

```javascript
const language = "JavaScript";

console.log(language.substring(0, 4));
// Java

console.log(language.substring(4, 10));
// Script
```

Unlike `slice()`, negative arguments are treated as zero, and reversed indexes are swapped.

### 🖆 Selecting the First and Last Characters

```javascript
const language = "JavaScript";

const first = language.slice(0, 1);
const last = language.slice(-1);

console.log(first);
// J

console.log(last);
// t
```

# 🗁 Replacing String Content

### 🖆 `replace()`

Replaces the first occurrence of a matching substring or regular expression.

```javascript
const message = "I like JavaScript";

const result = message.replace("JavaScript", "TypeScript");

console.log(result);
// I like TypeScript
```

### 🖆 `replaceAll()`

Replaces all occurrences of a matching substring.

```javascript
const message = "apple apple apple";

const result = message.replaceAll("apple", "orange");

console.log(result);
// orange orange orange
```

### 🖆 Removing Specific Characters

```javascript
const phone = "+880-1712-345678";

const result = phone.replaceAll("-", "");

console.log(result);
// +8801712345678
```

### 🖆 Removing Special Characters

```javascript
const username = "alice@developer#2026!";

const result = username.replace(/[^a-zA-Z0-9]/g, "");

console.log(result);
// alicedeveloper2026
```

The pattern `[^a-zA-Z0-9]` matches characters other than English letters and digits.

This example intentionally excludes spaces and non-English characters.

### 🖆 Replacing Multiple Spaces

```javascript
const message = "JavaScript     is    awesome";

const result = message.replace(/\s+/g, " ");

console.log(result);
// JavaScript is awesome
```

# 🗁 Splitting and Joining Strings

### 🖆 `split()`

Splits a string into an array using a separator.

```javascript
const technologies = "HTML,CSS,JavaScript";

const result = technologies.split(",");

console.log(result);
// ["HTML", "CSS", "JavaScript"]
```

### 🖆 `join()`

The `join()` method belongs to arrays. It combines array elements into a string using a separator.

```javascript
const technologies = ["HTML", "CSS", "JavaScript"];

const result = technologies.join(", ");

console.log(result);
// HTML, CSS, JavaScript
```

### 🖆 Splitting and Joining

```javascript
const message = "JavaScript is powerful";

const result = message
    .split(" ")
    .join("-");

console.log(result);
// JavaScript-is-powerful
```

### 🖆 Replacing Spaces with Underscores

```javascript
const title = "JavaScript String Manipulation";

const slug = title
    .trim()
    .toLowerCase()
    .split(/\s+/)
    .join("_");

console.log(slug);
// javascript_string_manipulation
```

# 🗁 Combining Strings

### 🖆 Using the `+` Operator

```javascript
const firstName = "Muhammad";
const lastName = "Iqbal";

const fullName = firstName + " " + lastName;

console.log(fullName);
// Muhammad Iqbal
```

### 🖆 Using the `concat()` Method

```javascript
const firstName = "Muhammad";
const lastName = "Iqbal";

const fullName = firstName.concat(" ", lastName);

console.log(fullName);
// Muhammad Iqbal
```

### 🖆 Using Template Literals

```javascript
const firstName = "Muhammad";
const lastName = "Iqbal";

const fullName = `${firstName} ${lastName}`;

console.log(fullName);
// Muhammad Iqbal
```

Template literals are generally the most readable option when combining multiple dynamic values.

# 🗁 Reversing Strings

JavaScript does not provide a built-in `reverse()` method for strings.

A common approach is to convert the string into an array, reverse it, and join the elements.

### 🖆 Using `split()`, `reverse()`, and `join()`

```javascript
const message = "JavaScript";

const reversed = message
    .split("")
    .reverse()
    .join("");

console.log(reversed);
// tpircSavaJ
```

This simple technique works for many ordinary strings, but it can break Unicode characters represented by surrogate pairs.

### 🖆 Unicode-Aware Reversal

Use the spread operator to iterate over Unicode code points.

```javascript
const message = "Hello 😀";

const reversed = [...message]
    .reverse()
    .join("");

console.log(reversed);
// 😀 olleH
```

This handles Unicode code points, but it does not preserve every multi-code-point grapheme cluster, such as some emoji sequences.

### 🖆 Grapheme-Aware Reversal

For user-perceived characters, use `Intl.Segmenter`.

```javascript
function reverseString(text) {
    const segmenter = new Intl.Segmenter(
        undefined,
        { granularity: "grapheme" }
    );

    const characters = [...segmenter.segment(text)]
        .map(segment => segment.segment);

    return characters.reverse().join("");
}

console.log(reverseString("Hello 😀"));
// 😀 olleH
```

This approach handles grapheme clusters more accurately.

# 🗁 Padding and Formatting Strings

### 🖆 `padStart()`

Adds padding to the beginning of a string until it reaches the specified length.

```javascript
const number = "7";

console.log(number.padStart(3, "0"));
// 007
```

### 🖆 `padEnd()`

Adds padding to the end of a string.

```javascript
const number = "7";

console.log(number.padEnd(3, "0"));
// 700
```

### 🖆 Formatting Identifiers

```javascript
function formatId(id) {
    return String(id).padStart(6, "0");
}

console.log(formatId(42));
// 000042

console.log(formatId(125));
// 000125
```

### 🖆 Formatting Text with `repeat()`

```javascript
const title = "JavaScript";

const result = `${title}\n${"=".repeat(title.length)}`;

console.log(result);
// JavaScript
// ==========
```

# 🗁 Capitalizing Strings

JavaScript does not provide a dedicated built-in method for capitalizing the first letter of a string.

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

### 🖆 Converting Text to Title Case

```javascript
function titleCase(text) {
    return text
        .toLowerCase()
        .split(" ")
        .map(word => {
            return word.charAt(0).toUpperCase() + word.slice(1);
        })
        .join(" ");
}

console.log(titleCase("javascript string manipulation"));
// Javascript String Manipulation
```

This is a basic English-oriented implementation. Production title casing may require language-specific rules, punctuation handling, and Unicode-aware processing.

# 🗁 Removing Duplicate Characters

A Set can remove duplicate characters while preserving their first-occurrence order.

### 🖆 Using `Set`

```javascript
const message = "programming";

const result = [...new Set(message)].join("");

console.log(result);
// progamin
```

### 🖆 Using a Loop

```javascript
function removeDuplicates(text) {
    let result = "";

    for (const character of text) {
        if (!result.includes(character)) {
            result += character;
        }
    }

    return result;
}

console.log(removeDuplicates("programming"));
// progamin
```

The Set-based approach is generally more efficient for larger inputs.

# 🗁 Repeating Strings

### 🖆 `repeat()`

Repeats a string a specified number of times.

```javascript
const separator = "-";

console.log(separator.repeat(10));
// ----------
```

### 🖆 Generating a Simple Pattern

```javascript
function createPattern(character, count) {
    return character.repeat(count);
}

console.log(createPattern("*", 5));
// *****
```

The repetition count must be non-negative and finite.

# 🗁 Unicode String Manipulation

JavaScript strings use UTF-16 encoding.

This means some visible characters occupy more than one UTF-16 code unit.

### 🖆 Understanding String Length

```javascript
const emoji = "😀";

console.log(emoji.length);
// 2
```

The length property counts UTF-16 code units rather than user-perceived characters.

### 🖆 Using the Spread Operator

```javascript
const emoji = "😀";

console.log([...emoji].length);
// 1
```

The spread operator iterates over Unicode code points.

### 🖆 Counting User-Perceived Characters

Use `Intl.Segmenter` to count grapheme clusters.

```javascript
function countCharacters(text) {
    const segmenter = new Intl.Segmenter(
        undefined,
        { granularity: "grapheme" }
    );

    return [...segmenter.segment(text)].length;
}

console.log(countCharacters("Hello"));
// 5

console.log(countCharacters("😀"));
// 1
```

This is useful when counting characters in text fields, messages, and other user-facing interfaces.

### 🖆 Unicode Normalization

Visually identical characters can have different underlying Unicode representations.

Use `normalize()` when consistent Unicode representation is needed.

```javascript
const first = "\u00E9";
const second = "\u0065\u0301";

console.log(first === second);
// false

console.log(first.normalize("NFC") === second.normalize("NFC"));
// true
```

# 🗁 Practical String Manipulation Examples

### 🖆 Generating URL Slugs

A slug is a URL-friendly representation of a title.

```javascript
function createSlug(title) {
    return title
        .trim()
        .toLowerCase()
        .replace(/[^a-z0-9\s-]/g, "")
        .replace(/\s+/g, "-")
        .replace(/-+/g, "-");
}

console.log(createSlug("JavaScript String Manipulation!"));
// javascript-string-manipulation
```

This basic implementation supports English letters and digits. Multilingual slug generation requires a more inclusive Unicode-aware approach.

### 🖆 Formatting Phone Numbers

```javascript
function formatPhoneNumber(phone) {
    const digits = phone.replace(/\D/g, "");

    return digits.replace(
        /^(\d{3})(\d{4})(\d{4})$/,
        "$1-$2-$3"
    );
}

console.log(formatPhoneNumber("01712345678"));
// 017-1234-5678
```

This example assumes an 11-digit local number with the specified grouping.

### 🖆 Removing HTML Tags

```javascript
function removeHtmlTags(html) {
    return html.replace(/<[^>]*>/g, "");
}

console.log(
    removeHtmlTags("<h1>Hello</h1><p>JavaScript</p>")
);
// HelloJavaScript
```

This is a basic demonstration, not a reliable HTML parser or security sanitizer. Use an HTML parser when processing real HTML documents.

### 🖆 Extracting Initials

```javascript
function getInitials(fullName) {
    return fullName
        .trim()
        .split(/\s+/)
        .map(name => name.charAt(0).toUpperCase())
        .join("");
}

console.log(getInitials("Muhammad S A Iqbal"));
// MSAI
```

### 🖆 Formatting Currency

Use `Intl.NumberFormat` instead of manually manipulating decimal separators and currency symbols.

```javascript
const amount = 12500.5;

const formatter = new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD"
});

console.log(formatter.format(amount));
// $12,500.50
```

For Bangladeshi taka:

```javascript
const amount = 12500.5;

const formatter = new Intl.NumberFormat("en-BD", {
    style: "currency",
    currency: "BDT"
});

console.log(formatter.format(amount));
```

Locale-aware formatting handles currency conventions more reliably than manual string construction.

### 🖆 Normalizing User Input

```javascript
function normalizeInput(value) {
    return value
        .trim()
        .replace(/\s+/g, " ")
        .toLowerCase();
}

console.log(normalizeInput("  HELLO    JAVASCRIPT  "));
// hello javascript
```

# 🗁 Common Mistakes

### 🖆 Expecting String Methods to Modify the Original

Incorrect:

```javascript
let message = "hello";

message.toUpperCase();

console.log(message);
// hello
```

Correct:

```javascript
let message = "hello";

message = message.toUpperCase();

console.log(message);
// HELLO
```

### 🖆 Using `reverse()` Directly on a String

Incorrect:

```javascript
const message = "hello";

message.reverse();
// TypeError
```

Correct:

```javascript
const reversed = [...message].reverse().join("");
```

### 🖆 Removing All Spaces with `trim()`

Incorrect:

```javascript
const message = "JavaScript is powerful";

console.log(message.trim());
// JavaScript is powerful
```

`trim()` only removes whitespace from the beginning and end.

Correct:

```javascript
console.log(message.replace(/\s/g, ""));
// JavaScriptispowerful
```

### 🖆 Ignoring Unicode Characters

Incorrect for general Unicode code-point reversal:

```javascript
const emoji = "😀";

console.log(emoji.split("").reverse().join(""));
```

Use code-point-aware iteration:

```javascript
console.log([...emoji].reverse().join(""));
// 😀
```

For complete grapheme-cluster handling, use `Intl.Segmenter`.

### 🖆 Building Complex Strings with Repeated Concatenation

Less readable:

```javascript
const name = "Alice";
const age = 25;

const message = "Name: " + name + ", Age: " + age;
```

More readable:

```javascript
const message = `Name: ${name}, Age: ${age}`;
```

# 🗁 Best Practices

* Remember that strings are immutable.
* Use `trim()` to remove surrounding whitespace.
* Use regular expressions for pattern-based replacement.
* Prefer `replaceAll()` when all literal occurrences must be replaced.
* Use `split()` and `join()` for delimiter-based transformations.
* Prefer template literals for readable dynamic strings.
* Use `Set` to remove duplicate characters while preserving order.
* Use the spread operator for Unicode code-point iteration.
* Use `Intl.Segmenter` when user-perceived character boundaries matter.
* Use `normalize()` for consistent Unicode representation.
* Use `Intl.NumberFormat` for locale-aware numeric and currency formatting.
* Avoid treating regular-expression replacement as a substitute for an HTML parser or sanitizer.
* Avoid unnecessarily complicated chained transformations; extract them into reusable functions.

# 🗁 Summary

JavaScript provides several techniques for manipulating strings.

| Technique | Purpose |
| --- | --- |
| `toUpperCase()` | Convert to uppercase |
| `toLowerCase()` | Convert to lowercase |
| `trim()` | Remove surrounding whitespace |
| `replace()` | Replace matching content |
| `replaceAll()` | Replace all matching content |
| `slice()` | Extract a portion of a string |
| `split()` | Convert a string into an array |
| `join()` | Combine array elements into a string |
| `concat()` | Combine strings |
| `padStart()` | Add padding at the beginning |
| `padEnd()` | Add padding at the end |
| `repeat()` | Repeat a string |
| `normalize()` | Normalize Unicode representation |
| `Intl.Segmenter` | Segment text into grapheme clusters |
| `Intl.NumberFormat` | Format numbers and currencies |

**Remember:** Effective string manipulation combines built-in methods, regular expressions, array operations, and Unicode-aware APIs. Choose the appropriate technique based on the actual structure and requirements of the text.
