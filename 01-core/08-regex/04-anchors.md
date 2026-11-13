# Regex Anchors

Anchors are special Regex constructs that match a position in text rather than matching an actual character.

They allow you to specify where a pattern must occur.

For example, you can use anchors to:

* Match text only at the beginning of a string.
* Match text only at the end of a string.
* Validate an entire input value.
* Find words at specific boundaries.
* Match individual lines in multiline text.
* Prevent a pattern from matching inside a larger word.

Anchors are particularly important when validating input such as usernames, email-like strings, phone numbers, and identifiers.

# 🗁 What Are Anchors?

A normal Regex pattern matches characters.

Example:

```regex
cat
```

It can find `cat` anywhere inside an input string.

Input:

```text
The cat is sleeping.
```

The pattern finds:

```text
cat
```

Anchors change the required matching position.

Example:

```regex
^cat
```

This pattern matches `cat` only at the beginning of the input or line, depending on the Regex mode.

# 🗁 Beginning Anchor: `^`

The caret (`^`) matches the beginning of the input.

Example:

```regex
^Hello
```

Matches:

```text
Hello World
```

Does not match:

```text
Say Hello
```

The pattern requires `Hello` to appear at the beginning.

**Practical example:**

```regex
^[A-Z]
```

Matches an input or line that begins with an uppercase English letter.

Examples:

```text
Hello
World
Admin
```

Does not match:

```text
hello
123Hello
```

# 🗁 Ending Anchor: `$`

The dollar sign (`$`) matches the end of the input.

Example:

```regex
world$
```

Matches:

```text
Hello world
```

Does not match:

```text
world is beautiful
```

The pattern requires `world` to appear at the end.

**Practical example:**

```regex
[0-9]$
```

Matches an input or line ending with a digit.

Examples:

```text
Order123
Product9
User7
```

Does not match:

```text
123Order
ProductA
```

# 🗁 Matching an Entire String

Combining `^` and `$` allows you to require a pattern to match the entire input in common single-line matching modes.

Example:

```regex
^[0-9]+$
```

This pattern matches an input containing only one or more digits.

Matches:

```text
123
456789
2026
```

Does not match:

```text
abc123
123abc
12 34
```

Explanation:

* `^` requires the match to begin at the start.
* `[0-9]+` matches one or more digits.
* `$` requires the match to reach the end.

**Important:** Some engines allow `$` to match before a final line terminator. For strict absolute-end validation, use an appropriate absolute-end anchor or a full-match API.

# 🗁 Absolute Beginning Anchor: `\A`

The `\A` anchor matches the absolute beginning of the input.

Example:

```regex
\AHello
```

Matches:

```text
Hello World
```

Does not match:

```text
Say Hello
```

Unlike `^`, `\A` generally remains tied to the absolute beginning even when multiline mode is enabled.

`\A` is supported by many engines, including Python and PCRE2, but it is not supported by every Regex flavor.

# 🗁 Absolute Ending Anchors: `\Z` and `\z`

Some Regex engines provide special anchors for the absolute end of the input.

**`\Z`**

In engines such as Python, `\Z` matches the absolute end of the string.

**`\z`**

In engines such as PCRE2, `\z` matches only the absolute end of the subject.

The distinction between `\Z` and `\z` is engine-specific. For example, Python's `\Z` is an absolute-end anchor, while PCRE2 distinguishes `\Z` from `\z` regarding a final newline.

| Anchor | Common Meaning |
| --- | --- |
| `^` | Beginning of input or line |
| `$` | End of input or line |
| `\A` | Absolute beginning |
| `\Z` | Absolute end in Python; behavior varies by engine |
| `\z` | Absolute end in engines that support it |

When writing portable patterns, check the target Regex engine's documentation.

# 🗁 Word Boundary: `\b`

The `\b` anchor matches a position between a word character and a non-word character, or at the beginning or end of text when adjacent to a word character.

It does not consume any character.

Example:

```regex
\bcat\b
```

Matches:

```text
The cat is sleeping.
A cat is an animal.
cat
```

Does not match:

```text
category
wildcat
cats
```

This is useful when you need to find a complete word rather than a substring.

**Find the word `admin`:**

```regex
\badmin\b
```

Input:

```text
admin administrator superadmin
```

Only the standalone `admin` is matched.

The exact definition of a word character depends on the Regex engine and its Unicode settings.

# 🗁 Non-Word Boundary: `\B`

The `\B` anchor matches a position that is not a word boundary.

Example:

```regex
\Bcat
```

Can match `cat` when it is preceded by another word character.

Input:

```text
wildcat
cat
```

The pattern can match the `cat` portion of `wildcat`, but not the standalone `cat` at the beginning of the second string.

Another example:

```regex
cat\B
```

Can match `cat` when it is followed by another word character.

Input:

```text
cats
cat!
```

It matches the `cat` portion of `cats`, but not the `cat` followed by punctuation.

# 🗁 Word Boundary vs Non-Word Boundary

| Pattern | Purpose |
| --- | --- |
| `\bword\b` | Matches a complete word |
| `\bword` | Matches a word beginning with `word` |
| `word\b` | Matches a word ending with `word` |
| `\Bword` | Matches `word` without a preceding word boundary |
| `word\B` | Matches `word` without a following word boundary |

Example:

```regex
\bcat\b
```

Input:

```text
cat category wildcat cats
```

Matches only:

```text
cat
```

# 🗁 Multiline Mode: `m`

The multiline flag changes how `^` and `$` behave.

Without multiline mode:

* `^` matches the beginning of the entire input.
* `$` matches the end of the entire input, subject to engine-specific final-newline behavior.

With multiline mode:

* `^` can match the beginning of each line.
* `$` can match the end of each line.

JavaScript example:

```javascript
const text = `admin
user
guest`;

const pattern = /^user/m;

console.log(pattern.test(text));
```

Output:

```text
true
```

The `m` flag allows `^` to match the beginning of the second line.

Without `m`:

```javascript
const text = `admin
user
guest`;

const pattern = /^user/;

console.log(pattern.test(text));
```

Output:

```text
false
```

Here, `^` only matches the beginning of the entire input.

# 🗁 Single-Line and Multiline Behavior

The term *single-line* can be confusing because Regex engines use it differently.

In many engines, the `m` flag controls the behavior of `^` and `$` across lines.

The `s` or DotAll flag controls whether `.` matches line terminators.

These are separate behaviors.

| Flag | Common Effect |
| --- | --- |
| `m` | Changes the behavior of `^` and `$` |
| `s` | Allows `.` to match line terminators |

Example:

```javascript
const text = `Hello
World`;

console.log(/^World/.test(text));   // false
console.log(/^World/m.test(text));  // true
console.log(/Hello.World/s.test(text)); // true
```

# 🗁 Anchors with Character Classes and Quantifiers

Anchors become more powerful when combined with other Regex elements.

**Validate a numeric string:**

```regex
^[0-9]+$
```

**Validate a four-digit PIN:**

```regex
^[0-9]{4}$
```

**Match a username between three and sixteen characters:**

```regex
^[a-zA-Z0-9_]{3,16}$
```

**Match a string beginning with `https`:**

```regex
^https
```

**Match a string ending with `.pdf`:**

```regex
\.pdf$
```

**Match a complete word containing only lowercase English letters:**

```regex
^[a-z]+$
```

**Find standalone occurrences of `error`:**

```regex
\berror\b
```

# 🗁 Anchors in JavaScript

JavaScript supports `^`, `$`, `\b`, and `\B`, along with multiline behavior through the `m` flag.

Example:

```javascript
const username = "admin123";

const pattern = /^[a-zA-Z0-9_]{3,16}$/;

console.log(pattern.test(username));
```

Output:

```text
true
```

Example using word boundaries:

```javascript
const text = "error occurred, no error found";

const pattern = /\berror\b/g;

console.log(text.match(pattern));
```

Output:

```text
["error", "error"]
```

JavaScript does not provide the `\A`, `\Z`, or `\z` absolute anchors as Regex syntax. Use `^` and `$` with the appropriate mode or use a suitable validation approach.

# 🗁 Anchors in Python

Python supports `^`, `$`, `\A`, `\Z`, `\b`, and `\B`.

Example:

```python
import re

username = "admin123"

pattern = r"\A[a-zA-Z0-9_]{3,16}\Z"

print(bool(re.fullmatch(pattern, username)))
```

Output:

```text
True
```

Example using multiline mode:

```python
import re

text = """admin
user
guest"""

pattern = r"^user"

print(bool(re.search(pattern, text)))
print(bool(re.search(pattern, text, re.MULTILINE)))
```

Output:

```text
False
True
```

Example using word boundaries:

```python
import re

text = "error occurred, no error found"

pattern = r"\berror\b"

print(re.findall(pattern, text))
```

Output:

```text
['error', 'error']
```

# 🗁 Practical Examples

**Validate a four-digit PIN:**

```regex
^[0-9]{4}$
```

**Validate a six-digit verification code:**

```regex
^[0-9]{6}$
```

**Validate a username:**

```regex
^[a-zA-Z0-9_]{3,16}$
```

**Find a complete word:**

```regex
\bnetwork\b
```

**Find lines beginning with `ERROR`:**

```regex
^ERROR
```

Use multiline mode when searching multiple lines as one input.

**Find lines ending with `.log`:**

```regex
\.log$
```

**Match an input containing only lowercase letters:**

```regex
^[a-z]+$
```

**Match an input containing only digits and optional surrounding whitespace:**

```regex
^\s*[0-9]+\s*$
```

**Match a string beginning with an HTTP or HTTPS scheme:**

```regex
^https?://
```

# 🗁 Common Mistakes

**Forgetting the end anchor**

```regex
^[0-9]+
```

This requires the input to begin with digits but allows additional characters afterward.

Use:

```regex
^[0-9]+$
```

when the entire input must contain only digits.

**Confusing `^` with `\b`**

```regex
^admin
```

Matches `admin` at the beginning of the input or line.

```regex
\badmin\b
```

Matches `admin` as a complete word anywhere in the input.

**Assuming multiline mode changes the dot**

The `m` flag changes `^` and `$` behavior. It does not make the dot match newline characters.

Use the `s` flag when DotAll behavior is required.

**Assuming `\Z` and `\z` are identical everywhere**

Their behavior depends on the Regex engine. Check the documentation before relying on either anchor.

**Assuming `$` always means the absolute end**

Some engines allow `$` to match before a final newline. Use an absolute-end anchor or a full-match API when strict whole-input validation is required.

# 🗁 Practice Exercises

**Exercise: Match the Beginning**

Write a pattern that matches strings beginning with `admin`.

**Exercise: Match the End**

Write a pattern that matches strings ending with `.txt`.

**Exercise: Validate an Entire String**

Write a pattern that accepts only digits and requires at least five digits.

**Exercise: Match a Complete Word**

Given:

```text
network networking networker
```

Write a pattern that matches only the standalone word `network`.

**Exercise: Multiline Matching**

Given:

```text
INFO Server started
ERROR Database failed
INFO Connection restored
```

Write a pattern that matches lines beginning with `ERROR`.

**Exercise: Username Validation**

Write a pattern that accepts usernames containing three to twelve English letters, digits, or underscores.

**Exercise: Absolute Boundaries**

In Python, write a pattern using absolute beginning and ending anchors to validate a string containing exactly six digits.

# 🗁 Quick Reference

| Anchor | Meaning |
| --- | --- |
| `^` | Beginning of input or line |
| `$` | End of input or line |
| `\A` | Absolute beginning of input |
| `\Z` | Absolute end in Python; engine-specific elsewhere |
| `\z` | Absolute end in engines that support it |
| `\b` | Word boundary |
| `\B` | Non-word boundary |

# 🗁 Final Notes

Anchors control where a Regex pattern can match.

Remember the key differences:

* `^` and `$` define beginning and ending positions.
* `\A` and `\Z` provide absolute input boundaries in supported engines.
* `\z` is an absolute-end anchor in engines that support it.
* `\b` identifies word boundaries.
* `\B` identifies positions that are not word boundaries.
* Multiline mode changes how `^` and `$` behave across lines.

Anchors are especially important for input validation because they prevent a pattern from accepting only a matching substring when the entire input must satisfy the rule.
