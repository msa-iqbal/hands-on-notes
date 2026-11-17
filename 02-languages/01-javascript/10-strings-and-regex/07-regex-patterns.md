# JavaScript Regular Expression Patterns

Regular expression patterns define the rules used to search, match, validate, extract, and transform text.

JavaScript supports many pattern combinations through character classes, quantifiers, anchors, groups, lookarounds, and Unicode property escapes.

This document focuses on practical regular expression patterns that developers commonly use when working with JavaScript applications.

# 🗁 Text Matching Patterns

### 🖆 Match a Specific Word

```javascript
const pattern = /javascript/i;

console.log(pattern.test("I love JavaScript"));
// true
```

The `i` flag makes matching case-insensitive.

### 🖆 Match a Word at the Beginning

```javascript
const pattern = /^Hello/;

console.log(pattern.test("Hello World"));
// true

console.log(pattern.test("Say Hello"));
// false
```

### 🖆 Match a Word at the End

```javascript
const pattern = /World$/;

console.log(pattern.test("Hello World"));
// true

console.log(pattern.test("World Hello"));
// false
```

### 🖆 Match an Entire Word

```javascript
const pattern = /\bcat\b/i;

console.log(pattern.test("The cat is sleeping"));
// true

console.log(pattern.test("category"));
// false
```

### 🖆 Match a Word Containing Specific Characters

```javascript
const pattern = /j\w+a/;

console.log(pattern.test("javascript"));
// true
```

### 🖆 Match Multiple Alternatives

```javascript
const pattern = /javascript|typescript|python/i;

console.log(pattern.test("Learning TypeScript"));
// true
```

### 🖆 Match a Word with Optional Characters

```javascript
const pattern = /colou?r/;

console.log(pattern.test("color"));
// true

console.log(pattern.test("colour"));
// true
```

# 🗁 Number Matching Patterns

### 🖆 Match Any Digit

```javascript
const pattern = /\d/;

console.log(pattern.test("Order 123"));
// true
```

### 🖆 Match a Sequence of Digits

```javascript
const pattern = /\d+/;

console.log("Order 12345".match(pattern)[0]);
// 12345
```

### 🖆 Match Exactly Four Digits

```javascript
const pattern = /^\d{4}$/;

console.log(pattern.test("2026"));
// true

console.log(pattern.test("12345"));
// false
```

### 🖆 Match Between Two and Five Digits

```javascript
const pattern = /^\d{2,5}$/;

console.log(pattern.test("123"));
// true

console.log(pattern.test("123456"));
// false
```

### 🖆 Match Positive Integers

```javascript
const pattern = /^[1-9]\d*$/;

console.log(pattern.test("123"));
// true

console.log(pattern.test("0"));
// false

console.log(pattern.test("0123"));
// false
```

### 🖆 Match Non-Negative Integers

```javascript
const pattern = /^(0|[1-9]\d*)$/;

console.log(pattern.test("0"));
// true

console.log(pattern.test("123"));
// true

console.log(pattern.test("-123"));
// false
```

### 🖆 Match Signed Integers

```javascript
const pattern = /^[+-]?\d+$/;

console.log(pattern.test("123"));
// true

console.log(pattern.test("-123"));
// true

console.log(pattern.test("+123"));
// true
```

### 🖆 Match Decimal Numbers

```javascript
const pattern = /^[+-]?\d+(\.\d+)?$/;

console.log(pattern.test("123"));
// true

console.log(pattern.test("123.45"));
// true

console.log(pattern.test("-123.45"));
// true

console.log(pattern.test("123."));
// false
```

### 🖆 Match Numbers with Optional Decimal Digits

```javascript
const pattern = /^[+-]?\d+(\.\d*)?$/;

console.log(pattern.test("123"));
// true

console.log(pattern.test("123.45"));
// true

console.log(pattern.test("123."));
// true
```

### 🖆 Match Numbers with Comma Separators

```javascript
const pattern = /^-?\d{1,3}(,\d{3})*(\.\d+)?$/;

console.log(pattern.test("1,000"));
// true

console.log(pattern.test("12,500.75"));
// true

console.log(pattern.test("1000"));
// false
```

This pattern requires comma grouping for numbers larger than three digits. It is not a universal number-format validator.

# 🗁 Email and Contact Patterns

### 🖆 Basic Email Pattern

```javascript
const pattern = /^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$/;

console.log(pattern.test("user@example.com"));
// true

console.log(pattern.test("invalid-email"));
// false
```

This is a practical format check, not a complete implementation of the email-address standard.

### 🖆 Match a Phone Number

A simple pattern for an 11-digit phone number beginning with `01`:

```javascript
const pattern = /^01\d{9}$/;

console.log(pattern.test("01712345678"));
// true

console.log(pattern.test("12345678901"));
// false
```

This checks only the specified structure. It does not verify whether a number is assigned or active.

### 🖆 Match an International Phone Format

```javascript
const pattern = /^\+\d{1,3}[- ]?\d{6,14}$/;

console.log(pattern.test("+8801712345678"));
// true

console.log(pattern.test("+1 2025550123"));
// true
```

This is a simplified format check, not full international phone-number validation.

### 🖆 Extract Email Addresses

```javascript
const text = `
Contact: support@example.com
Sales: sales@example.org
`;

const pattern = /[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}/g;

console.log(text.match(pattern));
// ["support@example.com", "sales@example.org"]
```

### 🖆 Extract Phone-Like Numbers

```javascript
const text = "Call 01712345678 or 01898765432";

const pattern = /\b01\d{9}\b/g;

console.log(text.match(pattern));
// ["01712345678", "01898765432"]
```

# 🗁 URL and Web Patterns

### 🖆 Match HTTP and HTTPS URLs

```javascript
const pattern = /^https?:\/\/.+$/i;

console.log(pattern.test("https://example.com"));
// true

console.log(pattern.test("http://example.com"));
// true

console.log(pattern.test("ftp://example.com"));
// false
```

### 🖆 Extract URLs from Text

```javascript
const text = `
Visit https://example.com
or http://developer.mozilla.org
`;

const pattern = /https?:\/\/[^\s]+/g;

console.log(text.match(pattern));
// ["https://example.com", "http://developer.mozilla.org"]
```

### 🖆 Match a Domain-Like String

```javascript
const pattern = /^[A-Za-z0-9-]+(\.[A-Za-z0-9-]+)+$/;

console.log(pattern.test("example.com"));
// true

console.log(pattern.test("sub.example.com"));
// true

console.log(pattern.test("invalid"));
// false
```

This checks a simplified domain-like structure, not all DNS hostname rules.

### 🖆 Match a File Extension

```javascript
const pattern = /\.(jpg|jpeg|png|gif|webp)$/i;

console.log(pattern.test("photo.jpg"));
// true

console.log(pattern.test("document.pdf"));
// false
```

### 🖆 Extract File Names and Extensions

```javascript
const pattern = /^(?<filename>.+)\.(?<extension>[^.]+)$/;

const result = "profile-image.png".match(pattern);

console.log(result.groups.filename);
// profile-image

console.log(result.groups.extension);
// png
```

# 🗁 Password Patterns

Password validation often combines several independent requirements.

### 🖆 Minimum Length

```javascript
const pattern = /^.{8,}$/;

console.log(pattern.test("password123"));
// true

console.log(pattern.test("short"));
// false
```

This counts UTF-16 code units, not necessarily user-perceived characters.

### 🖆 At Least One Uppercase Letter

```javascript
const pattern = /[A-Z]/;

console.log(pattern.test("Hello123"));
// true

console.log(pattern.test("hello123"));
// false
```

### 🖆 At Least One Lowercase Letter

```javascript
const pattern = /[a-z]/;

console.log(pattern.test("Hello123"));
// true
```

### 🖆 At Least One Digit

```javascript
const pattern = /\d/;

console.log(pattern.test("password123"));
// true
```

### 🖆 At Least One Special Character

```javascript
const pattern = /[^A-Za-z0-9]/;

console.log(pattern.test("Password@123"));
// true
```

### 🖆 Combined Password Pattern

Require:

* At least eight characters.
* One uppercase letter.
* One lowercase letter.
* One digit.
* One special character.

```javascript
const pattern = /^(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9]).{8,}$/;

console.log(pattern.test("Password@123"));
// true

console.log(pattern.test("password123"));
// false

console.log(pattern.test("PASSWORD@123"));
// false
```

The lookaheads check each requirement without consuming characters.

# 🗁 Date and Time Patterns

Regular expressions can check date and time formats, but they do not automatically validate calendar correctness.

### 🖆 Match YYYY-MM-DD Format

```javascript
const pattern = /^\d{4}-\d{2}-\d{2}$/;

console.log(pattern.test("2026-11-14"));
// true

console.log(pattern.test("14-11-2026"));
// false
```

This validates the format only. For example, `2026-99-99` also matches.

### 🖆 Match DD/MM/YYYY Format

```javascript
const pattern = /^\d{2}\/\d{2}\/\d{4}$/;

console.log(pattern.test("14/11/2026"));
// true
```

### 🖆 Match 24-Hour Time

```javascript
const pattern = /^([01]\d|2[0-3]):[0-5]\d$/;

console.log(pattern.test("14:30"));
// true

console.log(pattern.test("23:59"));
// true

console.log(pattern.test("25:70"));
// false
```

### 🖆 Match 12-Hour Time with AM/PM

```javascript
const pattern = /^(0?[1-9]|1[0-2]):[0-5]\d\s?(AM|PM)$/i;

console.log(pattern.test("02:30 PM"));
// true

console.log(pattern.test("12:45 AM"));
// true

console.log(pattern.test("14:30 PM"));
// false
```

### 🖆 Match ISO-Like Date and Time

```javascript
const pattern = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}$/;

console.log(pattern.test("2026-11-14T15:30:00"));
// true
```

This checks the shape of the string, not the validity of its date or time values.

# 🗁 HTML and Text Processing Patterns

### 🖆 Remove HTML Tags

```javascript
const html = "<h1>Hello</h1><p>JavaScript</p>";

const result = html.replace(/<[^>]*>/g, "");

console.log(result);
// HelloJavaScript
```

This simple pattern is not a reliable HTML parser. Use an HTML parser when processing real HTML documents.

### 🖆 Extract Hashtags

```javascript
const text = "Learning #JavaScript and #WebDevelopment";

const pattern = /#[A-Za-z0-9_]+/g;

console.log(text.match(pattern));
// ["#JavaScript", "#WebDevelopment"]
```

### 🖆 Extract Mentions

```javascript
const text = "Hello @alice and @bob";

const pattern = /@[A-Za-z0-9_]+/g;

console.log(text.match(pattern));
// ["@alice", "@bob"]
```

### 🖆 Remove Extra Whitespace

```javascript
const text = "JavaScript    is   powerful";

const result = text.replace(/\s+/g, " ");

console.log(result);
// JavaScript is powerful
```

### 🖆 Remove Leading and Trailing Whitespace

```javascript
const text = "   Hello JavaScript   ";

const result = text.replace(/^\s+|\s+$/g, "");

console.log(result);
// Hello JavaScript
```

In ordinary JavaScript code, `trim()` is simpler for this particular operation.

### 🖆 Extract Words

```javascript
const text = "JavaScript makes web development powerful.";

const words = text.match(/[A-Za-z]+/g);

console.log(words);
// ["JavaScript", "makes", "web", "development", "powerful"]
```

This pattern extracts ASCII alphabetic words. It does not fully support words from all writing systems.

### 🖆 Convert Text into a Slug

```javascript
function slugify(value) {
  return value
    .toLowerCase()
    .trim()
    .replace(/[^\w\s-]/g, "")
    .replace(/\s+/g, "-")
    .replace(/-+/g, "-");
}

console.log(slugify("JavaScript: Regular Expressions!"));
// javascript-regular-expressions
```

# 🗁 Unicode Regular Expression Patterns

JavaScript supports Unicode property escapes for matching characters by their Unicode properties.

These patterns require Unicode-aware mode using the `u` or `v` flag.

### 🖆 Match Unicode Letters

```javascript
const pattern = /^\p{Letter}+$/u;

console.log(pattern.test("JavaScript"));
// true

console.log(pattern.test("বাংলা"));
// true
```

### 🖆 Match Unicode Numbers

```javascript
const pattern = /^\p{Number}+$/u;

console.log(pattern.test("12345"));
// true
```

### 🖆 Match Unicode Whitespace

```javascript
const pattern = /\p{White_Space}+/u;

console.log(pattern.test("Hello World"));
// true
```

### 🖆 Match Letters from Multiple Writing Systems

```javascript
const pattern = /\p{Letter}+/gu;

const text = "Hello বাংলা 世界";

console.log(text.match(pattern));
// ["Hello", "বাংলা", "世界"]
```

### 🖆 Match Emoji Symbols

```javascript
const pattern = /\p{Extended_Pictographic}/u;

console.log(pattern.test("Hello 😀"));
// true
```

Unicode properties provide more reliable character matching for international applications than ASCII-only character classes.

# 🗁 Practical Pattern Reference

| Requirement | Pattern |
| --- | --- |
| Any digit | `\d` |
| Any non-digit | `\D` |
| One or more digits | `\d+` |
| Exactly four digits | `^\d{4}$` |
| Only letters | `^[A-Za-z]+$` |
| Letters and digits | `^[A-Za-z0-9]+$` |
| Optional whitespace | `\s*` |
| One or more spaces | `\s+` |
| Word boundary | `\bword\b` |
| Starts with a letter | `^[A-Za-z]` |
| Ends with a digit | `\d$` |
| HTTP or HTTPS | `^https?:\/\/` |
| Basic email-like format | `^[^\s@]+@[^\s@]+\.[^\s@]+$` |
| File extension | `\.[A-Za-z0-9]+$` |
| HTML-like tag | `<[^>]*>` |
| Hashtag | `#[A-Za-z0-9_]+` |
| Username-like string | `^[A-Za-z0-9_]{3,20}$` |

These are starting points, not universal validation rules. Adapt each pattern to the actual requirements of the application.

# 🗁 Testing Regular Expression Patterns

A pattern should be tested against valid, invalid, and boundary inputs.

### 🖆 Example: Username Validation

Requirements:

* Three to twenty characters.
* Letters, digits, and underscores only.
* Must start with a letter.

```javascript
const usernamePattern = /^[A-Za-z][A-Za-z0-9_]{2,19}$/;

const usernames = [
  "alice",
  "user_123",
  "1alice",
  "ab",
  "this_username_is_too_long",
  "john-doe"
];

for (const username of usernames) {
  console.log(username, usernamePattern.test(username));
}
```

Expected results:

| Username | Valid |
| --- | --- |
| `alice` | Yes |
| `user_123` | Yes |
| `1alice` | No |
| `ab` | No |
| `this_username_is_too_long` | No |
| `john-doe` | No |

### 🖆 Example: Reusable Validation Function

```javascript
function validate(value, pattern) {
  return pattern.test(value);
}

const patterns = {
  username: /^[A-Za-z][A-Za-z0-9_]{2,19}$/,
  email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
  phone: /^01\d{9}$/,
  postalCode: /^\d{4}$/
};

console.log(validate("alice_123", patterns.username));
// true

console.log(validate("user@example.com", patterns.email));
// true

console.log(validate("01712345678", patterns.phone));
// true

console.log(validate("1200", patterns.postalCode));
// true
```

The patterns above provide basic format checks. Production validation may require additional business rules and authoritative validation libraries.

# 🗁 Common Mistakes and Limitations

### 🖆 Treating Regex as a Complete Validator

A pattern can verify structure without proving that a value is meaningful or valid.

For example:

```javascript
const pattern = /^\d{4}-\d{2}-\d{2}$/;

console.log(pattern.test("2026-99-99"));
// true
```

The pattern accepts the string because it matches the expected shape.

For calendar validation, use JavaScript date handling or a dedicated date library with explicit rules.

### 🖆 Forgetting to Escape Special Characters

Incorrect:

```javascript
const pattern = /file.txt/;
```

The dot matches almost any character.

Correct:

```javascript
const pattern = /file\.txt/;
```

### 🖆 Using `\w` for International Text

The `\w` shorthand is ASCII-oriented.

For international letters, use Unicode property escapes:

```javascript
const pattern = /^\p{Letter}+$/u;

console.log(pattern.test("বাংলা"));
// true
```

### 🖆 Creating Patterns from Untrusted Input

Dynamic patterns can behave unexpectedly when user-provided text contains regex metacharacters.

Use `RegExp.escape()` where supported to escape text intended to be matched literally.

```javascript
const userInput = "price.100";

const escaped = RegExp.escape(userInput);

const pattern = new RegExp(escaped);

console.log(pattern.test("price.100"));
// true

console.log(pattern.test("priceX100"));
// false
```

For environments without `RegExp.escape()`, use a well-tested escaping utility.

### 🖆 Writing Excessively Complex Patterns

Complex regular expressions can be difficult to understand and maintain. Some patterns can also cause catastrophic backtracking, leading to severe performance problems.

Prefer smaller patterns, clear validation steps, and dedicated parsers for structured formats.

# 🗁 Best Practices

* Define the exact input requirements before writing a pattern.
* Use anchors when the entire input must match.
* Prefer readable character classes and quantifiers.
* Use named capturing groups when extracting structured values.
* Use Unicode property escapes for international text.
* Test valid, invalid, empty, and boundary inputs.
* Escape dynamic user input when it must be treated literally.
* Avoid using regular expressions as full HTML, JSON, or programming-language parsers.
* Keep complex validation logic maintainable.
* Consider performance when processing large or untrusted input.

# 🗁 Summary

Regular expression patterns are useful for common JavaScript text-processing tasks.

Important pattern categories include:

* Text matching and word boundaries.
* Numeric and decimal matching.
* Email, phone, and URL-like extraction.
* Password requirements.
* Date and time format checks.
* HTML-like and general text processing.
* Unicode character matching.
* Reusable validation patterns.

The most important skill is not memorizing hundreds of patterns. It is understanding how to combine character classes, quantifiers, anchors, groups, and lookarounds to build patterns that precisely match a requirement.
