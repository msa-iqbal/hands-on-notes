# JavaScript Regular Expression Basics

Regular expressions (RegEx or RegExp) are patterns used to search, match, validate, and manipulate text.

JavaScript provides built-in regular expression support through the `RegExp` object and regular expression literal syntax.

Regular expressions are useful for:

* Searching for specific text patterns.
* Validating email-like and phone-number formats.
* Extracting numbers and words.
* Replacing unwanted characters.
* Checking passwords against formatting rules.
* Splitting strings using patterns.
* Processing structured text.

# 🗁 What Is a Regular Expression?

A regular expression describes a pattern that JavaScript can use to find matching text.

For example:

```javascript
const pattern = /javascript/i;

const message = "I am learning JavaScript";

console.log(pattern.test(message));
// true
```

The pattern `/javascript/i` searches for the word `javascript` without considering letter casing.

# 🗁 Creating Regular Expressions

JavaScript supports two primary ways to create regular expressions.

### 🖆 Regular Expression Literal

A regular expression literal is written between forward slashes.

```javascript
const pattern = /hello/;

console.log(pattern.test("hello world"));
// true
```

This is the most common approach when the pattern is known in advance.

### 🖆 RegExp Constructor

The `RegExp` constructor creates a regular expression from a string.

```javascript
const pattern = new RegExp("hello");

console.log(pattern.test("hello world"));
// true
```

This approach is useful when the pattern needs to be constructed dynamically.

### 🖆 Using Dynamic Patterns

```javascript
const keyword = "JavaScript";

const pattern = new RegExp(keyword, "i");

console.log(pattern.test("Learning javascript"));
// true
```

When constructing patterns from arbitrary user input, escape regular expression metacharacters first if the input should be treated as literal text.

### 🖆 Literal vs Constructor

| Feature | Literal | Constructor |
| --- | --- | --- |
| Syntax | `/pattern/flags` | `new RegExp(pattern, flags)` |
| Static patterns | Convenient | Supported |
| Dynamic patterns | Not directly supported | Supported |
| Escaping | JavaScript regex literal rules | JavaScript string escaping plus regex rules |
| Common usage | Known patterns | Runtime-generated patterns |

# 🗁 Regular Expression Flags

Flags modify how a regular expression behaves.

Flags are written after the closing slash in a regular expression literal or passed as the second argument to the `RegExp` constructor.

### 🖆 Global Flag (`g`)

The global flag finds all matches instead of stopping after the first match in methods such as `match()`.

```javascript
const text = "apple banana apple";

console.log(text.match(/apple/));
// ["apple"]

console.log(text.match(/apple/g));
// ["apple", "apple"]
```

### 🖆 Case-Insensitive Flag (`i`)

The `i` flag ignores letter casing.

```javascript
const pattern = /javascript/i;

console.log(pattern.test("JavaScript"));
// true

console.log(pattern.test("JAVASCRIPT"));
// true
```

### 🖆 Multiline Flag (`m`)

The `m` flag changes how the `^` and `$` anchors behave.

They match the beginning and ending of individual lines rather than only the beginning and ending of the entire input.

```javascript
const text = `JavaScript
TypeScript
Python`;

console.log(text.match(/^TypeScript$/m));
// ["TypeScript"]
```

### 🖆 Dot-All Flag (`s`)

The `s` flag allows the dot (`.`) to match line terminator characters.

```javascript
const text = "Hello\nWorld";

console.log(/Hello.World/.test(text));
// false

console.log(/Hello.World/s.test(text));
// true
```

### 🖆 Unicode Flag (`u`)

The `u` flag enables Unicode-aware regular expression behavior.

```javascript
const emoji = "😀";

console.log(/^.$/.test(emoji));
// false

console.log(/^.$/u.test(emoji));
// true
```

### 🖆 Unicode Sets Flag (`v`)

The `v` flag enables Unicode sets mode, which provides advanced character-set operations and improved Unicode string properties.

```javascript
const pattern = /^\p{Letter}+$/v;

console.log(pattern.test("JavaScript"));
// true
```

The `v` flag is supported by modern JavaScript runtimes.

The `u` and `v` flags cannot be used together.

### 🖆 Sticky Flag (`y`)

The sticky flag requires matching to begin exactly at the regular expression's `lastIndex` position.

```javascript
const text = "JavaScript";

const pattern = /Script/y;

pattern.lastIndex = 4;

console.log(pattern.test(text));
// true
```

Unlike the global flag, sticky matching does not search ahead for a match after the specified position.

### 🖆 Has Indices Flag (`d`)

The `d` flag includes the start and end indexes of matches and capturing groups in the `indices` property.

```javascript
const text = "Order 12345";

const pattern = /\d+/d;

const result = text.match(pattern);

console.log(result[0]);
// 12345

console.log(result.indices[0]);
// [6, 11]
```

### 🖆 Available Flags

| Flag | Meaning |
| --- | --- |
| `g` | Global matching |
| `i` | Case-insensitive matching |
| `m` | Multiline mode |
| `s` | Dot-all mode |
| `u` | Unicode-aware mode |
| `v` | Unicode sets mode |
| `y` | Sticky matching |
| `d` | Include match indices |

Flags can be combined when their behavior is compatible.

```javascript
const pattern = /javascript/gi;
```

This searches globally and ignores letter casing.

# 🗁 Basic Regular Expression Characters

### 🖆 Literal Characters

Ordinary characters match themselves.

```javascript
const pattern = /cat/;

console.log(pattern.test("The cat is sleeping"));
// true

console.log(pattern.test("The dog is sleeping"));
// false
```

### 🖆 Dot (`.`)

The dot matches almost any single character except line terminators by default.

```javascript
const pattern = /c.t/;

console.log(pattern.test("cat"));
// true

console.log(pattern.test("cut"));
// true

console.log(pattern.test("coat"));
// false
```

The dot can match line terminators when the `s` flag is enabled.

### 🖆 Escaping Special Characters

Some characters have special meanings in regular expressions.

To match one of these characters literally, escape it with a backslash.

```javascript
const pattern = /\./;

console.log(pattern.test("file.txt"));
// true

console.log(pattern.test("fileXtxt"));
// false
```

Common special characters include:

```text
. * + ? ^ $ { } ( ) | [ ] \
```

For example, to match a literal question mark:

```javascript
const pattern = /\?/;

console.log(pattern.test("Are you ready?"));
// true
```

# 🗁 Character Classes

Character classes define a set of characters that can match at a particular position.

### 🖆 Character Set (`[]`)

Matches any one character contained in the set.

```javascript
const pattern = /[abc]/;

console.log(pattern.test("apple"));
// true

console.log(pattern.test("banana"));
// true

console.log(pattern.test("dog"));
// false
```

### 🖆 Character Range

A hyphen defines a range inside a character class.

```javascript
const lowercase = /[a-z]/;

console.log(lowercase.test("hello"));
// true

console.log(lowercase.test("HELLO"));
// false
```

Other examples:

```javascript
/[A-Z]/    // Uppercase English letters
/[a-z]/    // Lowercase English letters
/[0-9]/    // Digits
/[A-Za-z]/ // English letters
/[A-Za-z0-9]/ // English letters and digits
```

### 🖆 Negated Character Class (`[^]`)

A caret at the beginning of a character class negates the set.

```javascript
const pattern = /[^0-9]/;

console.log(pattern.test("123a"));
// true

console.log(pattern.test("12345"));
// false
```

The pattern matches any character that is not a digit.

### 🖆 Multiple Character Ranges

Different ranges can be combined.

```javascript
const pattern = /[A-Za-z0-9]/;

console.log(pattern.test("Hello123"));
// true

console.log(pattern.test("@#$"));
// false
```

# 🗁 Character Shorthand Classes

JavaScript provides shorthand character classes for common patterns.

### 🖆 Digit (`\d`)

Matches an ASCII digit from `0` to `9`.

```javascript
const pattern = /\d/;

console.log(pattern.test("abc123"));
// true
```

Equivalent character class:

```javascript
/[0-9]/
```

### 🖆 Non-Digit (`\D`)

Matches any character that is not an ASCII digit.

```javascript
const pattern = /\D/;

console.log(pattern.test("123a"));
// true
```

### 🖆 Word Character (`\w`)

Matches ASCII letters, digits, and underscores.

```javascript
const pattern = /\w/;

console.log(pattern.test("hello_123"));
// true
```

Equivalent character class:

```javascript
/[A-Za-z0-9_]/
```

### 🖆 Non-Word Character (`\W`)

Matches characters outside the ASCII word-character set.

```javascript
const pattern = /\W/;

console.log(pattern.test("hello@"));
// true
```

### 🖆 Whitespace (`\s`)

Matches whitespace characters, including spaces, tabs, and line terminators.

```javascript
const pattern = /\s/;

console.log(pattern.test("Hello World"));
// true
```

### 🖆 Non-Whitespace (`\S`)

Matches any character that is not whitespace.

```javascript
const pattern = /\S/;

console.log(pattern.test("   Hello"));
// true
```

### 🖆 Shorthand Reference

| Pattern | Meaning |
| --- | --- |
| `\d` | ASCII digit |
| `\D` | Non-digit |
| `\w` | ASCII word character |
| `\W` | Non-word character |
| `\s` | Whitespace |
| `\S` | Non-whitespace |

# 🗁 Quantifiers

Quantifiers specify how many times a character, group, or character class may occur.

### 🖆 Zero or More (`*`)

Matches zero or more occurrences.

```javascript
const pattern = /ab*c/;

console.log(pattern.test("ac"));
// true

console.log(pattern.test("abc"));
// true

console.log(pattern.test("abbbc"));
// true
```

### 🖆 One or More (`+`)

Matches one or more occurrences.

```javascript
const pattern = /ab+c/;

console.log(pattern.test("ac"));
// false

console.log(pattern.test("abc"));
// true

console.log(pattern.test("abbbc"));
// true
```

### 🖆 Zero or One (`?`)

Matches zero or one occurrence.

```javascript
const pattern = /colou?r/;

console.log(pattern.test("color"));
// true

console.log(pattern.test("colour"));
// true
```

### 🖆 Exact Quantity (`{n}`)

Matches exactly the specified number of occurrences.

```javascript
const pattern = /\d{4}/;

console.log(pattern.test("2026"));
// true

console.log(pattern.test("123"));
// false
```

### 🖆 Minimum Quantity (`{n,}`)

Matches at least the specified number of occurrences.

```javascript
const pattern = /\d{3,}/;

console.log(pattern.test("123"));
// true

console.log(pattern.test("12"));
// false
```

### 🖆 Quantity Range (`{n,m}`)

Matches between the specified minimum and maximum quantities.

```javascript
const pattern = /\d{2,4}/;

console.log(pattern.test("12"));
// true

console.log(pattern.test("1234"));
// true

console.log(pattern.test("12345"));
// true
```

The last example returns `true` because the pattern can match a four-digit substring. To require the entire string to contain two to four digits, use anchors:

```javascript
const pattern = /^\d{2,4}$/;

console.log(pattern.test("12345"));
// false
```

### 🖆 Quantifier Reference

| Quantifier | Meaning |
| --- | --- |
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `{n}` | Exactly n |
| `{n,}` | At least n |
| `{n,m}` | Between n and m |

# 🗁 Anchors

Anchors match positions rather than actual characters.

### 🖆 Beginning of String (`^`)

Matches the beginning of the input.

```javascript
const pattern = /^Hello/;

console.log(pattern.test("Hello World"));
// true

console.log(pattern.test("Say Hello"));
// false
```

### 🖆 End of String (`$`)

Matches the end of the input.

```javascript
const pattern = /World$/;

console.log(pattern.test("Hello World"));
// true

console.log(pattern.test("World Hello"));
// false
```

### 🖆 Word Boundary (`\b`)

Matches a position between a word character and a non-word character, or at the beginning or end of the input when adjacent to a word character.

```javascript
const pattern = /\bcat\b/;

console.log(pattern.test("The cat is sleeping"));
// true

console.log(pattern.test("category"));
// false
```

### 🖆 Non-Word Boundary (`\B`)

Matches a position that is not a word boundary.

```javascript
const pattern = /\Bcat/;

console.log(pattern.test("educate"));
// true
```

# 🗁 Alternation

The pipe (`|`) represents alternation, meaning one pattern or another.

```javascript
const pattern = /cat|dog/;

console.log(pattern.test("I have a cat"));
// true

console.log(pattern.test("I have a dog"));
// true

console.log(pattern.test("I have a bird"));
// false
```

### 🖆 Combining Alternation with Groups

```javascript
const pattern = /^(cat|dog)$/;

console.log(pattern.test("cat"));
// true

console.log(pattern.test("dog"));
// true

console.log(pattern.test("catalog"));
// false
```

The parentheses group the alternatives, while the anchors require the entire string to match.

# 🗁 Grouping

Parentheses group parts of a regular expression.

### 🖆 Capturing Groups

Capturing groups store the text matched by a group.

```javascript
const pattern = /(\d{4})-(\d{2})-(\d{2})/;

const result = "2026-11-14".match(pattern);

console.log(result[0]);
// 2026-11-14

console.log(result[1]);
// 2026

console.log(result[2]);
// 11

console.log(result[3]);
// 14
```

### 🖆 Non-Capturing Groups

A non-capturing group uses `(?:...)`.

It groups expressions without storing a separate captured result.

```javascript
const pattern = /(?:cat|dog)s/;

console.log(pattern.test("cats"));
// true

console.log(pattern.test("dogs"));
// true
```

### 🖆 Named Capturing Groups

Named groups use the `(?<name>...)` syntax.

```javascript
const pattern = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

const result = "2026-11-14".match(pattern);

console.log(result.groups.year);
// 2026

console.log(result.groups.month);
// 11

console.log(result.groups.day);
// 14
```

Named groups improve readability when extracting structured data.

# 🗁 Greedy and Lazy Matching

Quantifiers are greedy by default. They try to match as much text as possible while still allowing the overall pattern to succeed.

### 🖆 Greedy Matching

```javascript
const text = "<h1>Hello</h1><h1>World</h1>";

const result = text.match(/<h1>.*<\/h1>/);

console.log(result[0]);
// <h1>Hello</h1><h1>World</h1>
```

The `.*` expression consumes as much text as possible.

### 🖆 Lazy Matching

Adding `?` after a quantifier makes it lazy.

A lazy quantifier matches as little text as possible while still allowing the overall pattern to succeed.

```javascript
const text = "<h1>Hello</h1><h1>World</h1>";

const result = text.match(/<h1>.*?<\/h1>/);

console.log(result[0]);
// <h1>Hello</h1>
```

### 🖆 Greedy vs Lazy

| Pattern | Behavior |
| --- | --- |
| `.*` | Greedy: maximum possible match |
| `.*?` | Lazy: minimum possible match |
| `.+` | One or more, greedy |
| `.+?` | One or more, lazy |
| `\d+` | One or more digits, greedy |
| `\d+?` | One or more digits, lazy |

# 🗁 Lookahead and Lookbehind

Lookarounds check surrounding text without including that text in the actual match.

### 🖆 Positive Lookahead (`(?=...)`)

Requires a specified pattern to follow the current position.

```javascript
const pattern = /\d+(?= dollars)/;

const result = "Price: 100 dollars".match(pattern);

console.log(result[0]);
// 100
```

### 🖆 Negative Lookahead (`(?!...)`)

Requires a specified pattern not to follow the current position.

```javascript
const pattern = /\d+(?! dollars)/;

console.log("100 items".match(pattern)[0]);
// 100
```

### 🖆 Positive Lookbehind (`(?<=...)`)

Requires a specified pattern to appear before the current position.

```javascript
const pattern = /(?<=\$)\d+/;

const result = "$100".match(pattern);

console.log(result[0]);
// 100
```

### 🖆 Negative Lookbehind (`(?<!...)`)

Requires a specified pattern not to appear before the current position.

```javascript
const pattern = /(?<!\$)\d+/;

console.log("100 dollars".match(pattern)[0]);
// 100
```

Lookarounds are useful for validating context without including surrounding characters in the match.

# 🗁 Testing Regular Expressions

### 🖆 `test()`

The `test()` method checks whether a regular expression matches a string.

It returns a boolean.

```javascript
const pattern = /\d+/;

console.log(pattern.test("Order 123"));
// true

console.log(pattern.test("Order ABC"));
// false
```

### 🖆 `exec()`

The `exec()` method returns detailed information about a match.

```javascript
const pattern = /\d+/;

const result = pattern.exec("Order 123");

console.log(result[0]);
// 123

console.log(result.index);
// 6
```

When no match exists, `exec()` returns `null`.

### 🖆 `test()` vs `exec()`

| Method | Purpose | Return Value |
| --- | --- | --- |
| `test()` | Check whether a match exists | Boolean |
| `exec()` | Retrieve match details | Array or `null` |

For global and sticky regular expressions, both methods update the regular expression's `lastIndex` property.

# 🗁 Common Mistakes

### 🖆 Forgetting Anchors During Validation

Incorrect:

```javascript
const pattern = /\d{4}/;

console.log(pattern.test("abc1234xyz"));
// true
```

The pattern matches four digits anywhere in the string.

Correct:

```javascript
const pattern = /^\d{4}$/;

console.log(pattern.test("1234"));
// true

console.log(pattern.test("abc1234xyz"));
// false
```

### 🖆 Confusing `.` with a Literal Period

Incorrect:

```javascript
const pattern = /file.txt/;
```

The dot matches almost any single character.

Correct:

```javascript
const pattern = /file\.txt/;
```

### 🖆 Forgetting the Global Flag

```javascript
const text = "apple apple apple";

console.log(text.match(/apple/));
// ["apple"]

console.log(text.match(/apple/g));
// ["apple", "apple", "apple"]
```

### 🖆 Confusing Greedy and Lazy Quantifiers

```javascript
const text = "<p>One</p><p>Two</p>";

console.log(text.match(/<p>.*<\/p>/)[0]);
// <p>One</p><p>Two</p>

console.log(text.match(/<p>.*?<\/p>/)[0]);
// <p>One</p>
```

### 🖆 Forgetting That `\d` Matches ASCII Digits

The shorthand `\d` matches ASCII digits from `0` to `9`, not every digit character used in every writing system.

For Unicode-aware digit matching, use a Unicode property escape:

```javascript
const pattern = /\p{Decimal_Number}+/u;

console.log(pattern.test("123"));
// true
```

### 🖆 Reusing Global Regular Expressions Carelessly

Global regular expressions maintain state through `lastIndex`.

```javascript
const pattern = /hello/g;

console.log(pattern.test("hello"));
// true

console.log(pattern.test("hello"));
// false
```

The second call starts from the previous `lastIndex`.

For independent checks, avoid unnecessary global or sticky flags.

# 🗁 Best Practices

* Use regular expression literals for static patterns.
* Use the `RegExp` constructor when patterns must be generated dynamically.
* Escape special characters when they should be matched literally.
* Use anchors when validating an entire string.
* Use character classes for matching sets of characters.
* Use quantifiers to control repetition.
* Prefer non-capturing groups when captured values are unnecessary.
* Use named capturing groups for readable extraction.
* Understand greedy and lazy matching before constructing complex patterns.
* Use Unicode property escapes when matching international text.
* Be careful when reusing global and sticky regular expressions.
* Keep regular expressions readable and test them against both valid and invalid examples.
* Avoid excessively complex patterns that can cause catastrophic backtracking.

# 🗁 Summary

Regular expressions provide a powerful way to work with text patterns in JavaScript.

Key concepts:

* Regular expression literals and the `RegExp` constructor create patterns.
* Flags modify matching behavior.
* Character classes define which characters can match.
* Shorthand classes simplify common matching patterns.
* Quantifiers control repetition.
* Anchors match string positions.
* Alternation provides multiple matching choices.
* Groups organize patterns and capture values.
* Lookarounds inspect surrounding context.
* Greedy and lazy quantifiers control matching behavior.
* `test()` checks whether a match exists.
* `exec()` retrieves match details.

**Remember:** Regular expressions are most effective when the pattern is precise, readable, and tested against realistic input. Start with simple patterns and add complexity only when the requirement demands it.
