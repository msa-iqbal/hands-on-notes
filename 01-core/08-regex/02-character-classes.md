# Regex Character Classes

Character classes allow you to define a set of characters that a Regex pattern can match at a particular position.

Instead of matching one specific character, you can match any character from a specified group.

For example, the pattern `[abc]` matches exactly one character: either `a`, `b`, or `c`.

Character classes are useful for:

* Matching letters and numbers.
* Validating specific character formats.
* Finding hexadecimal values.
* Matching uppercase or lowercase letters.
* Excluding unwanted characters.
* Building more complex validation patterns.

# 🗁 Basic Character Classes

Square brackets `[]` define a character class.

Example:

```regex
[abc]
```

Matches any one of these characters:

```text
a
b
c
```

Given this input:

```text
cat bat dog
```

The pattern `[abc]` finds individual occurrences of `c`, `a`, and `b`.

A character class matches only one character at a time.

# 🗁 Matching Multiple Characters

You can combine character classes with quantifiers to match sequences.

Example:

```regex
[abc]+
```

Matches one or more consecutive occurrences of `a`, `b`, or `c`.

Matches:

```text
abc
aab
bca
ccc
```

Does not match:

```text
dog
xyz
```

Another example:

```regex
[0-9]+
```

Matches one or more consecutive digits.

Input:

```text
Order 123, Product 456, Quantity 10
```

Matches:

```text
123
456
10
```

# 🗁 Character Ranges

A character range defines a sequence of characters using a hyphen (`-`).

Examples:

| Character Class | Meaning |
| --- | --- |
| `[a-z]` | Any lowercase English letter |
| `[A-Z]` | Any uppercase English letter |
| `[0-9]` | Any decimal digit |
| `[a-f]` | Any lowercase letter from `a` to `f` |
| `[A-F]` | Any uppercase letter from `A` to `F` |
| `[a-zA-Z]` | Any uppercase or lowercase English letter |
| `[a-zA-Z0-9]` | Any English letter or digit |

Example:

```regex
[A-Z]+
```

Matches consecutive uppercase English letters.

Input:

```text
HELLO World REGEX
```

Matches:

```text
HELLO
W
REGEX
```

Example:

```regex
[a-zA-Z]+
```

Matches consecutive English alphabetic characters.

Input:

```text
Hello123 World456
```

Matches:

```text
Hello
World
```

# 🗁 Negated Character Classes

A caret (`^`) immediately after the opening square bracket negates a character class.

It means match any character except the characters listed.

Example:

```regex
[^0-9]
```

Matches any character that is not a digit.

Input:

```text
abc123
```

Matches:

```text
a
b
c
```

Another example:

```regex
[^aeiou]
```

Matches any character except lowercase vowels.

Input:

```text
hello
```

Matches:

```text
h
l
l
```

The caret has a different meaning outside a character class: it is commonly used as a beginning-of-line or beginning-of-input anchor.

# 🗁 Combining Ranges and Negation

You can combine multiple ranges inside one character class.

Example:

```regex
[^a-zA-Z0-9]
```

Matches characters that are not English letters or digits.

Input:

```text
user_123@example.com
```

Matches:

```text
_
@
.
```

Another example:

```regex
[a-zA-Z0-9_]
```

Matches English letters, digits, and underscores.

This is useful when defining permitted characters in usernames or identifiers.

# 🗁 Shorthand Character Classes

Most common Regex engines provide shorthand classes for frequently used character groups.

| Shorthand | Common Meaning |
| --- | --- |
| `\d` | A digit |
| `\D` | A non-digit |
| `\w` | A word character |
| `\W` | A non-word character |
| `\s` | A whitespace character |
| `\S` | A non-whitespace character |

The exact character set represented by these shortcuts can vary by Regex engine and Unicode mode.

# 🗁 Digit Class: `\d` and `\D`

The `\d` shorthand matches a digit.

Example:

```regex
\d+
```

Matches one or more consecutive digits.

Input:

```text
Order 123, Price 500, Quantity 25
```

Matches:

```text
123
500
25
```

The `\D` shorthand matches a non-digit.

Example:

```regex
\D+
```

Matches sequences containing characters other than digits.

Input:

```text
abc123xyz
```

Matches:

```text
abc
xyz
```

For ASCII decimal digits specifically, use:

```regex
[0-9]
```

This is useful when the expected input format explicitly requires digits `0` through `9`.

# 🗁 Word Character Class: `\w` and `\W`

The `\w` shorthand commonly matches:

* English uppercase letters (`A-Z`).
* English lowercase letters (`a-z`).
* Digits (`0-9`).
* Underscore (`_`).

Example:

```regex
\w+
```

Input:

```text
user_name123@example.com
```

Matches:

```text
user_name123
example
com
```

The `\W` shorthand matches characters that are not word characters.

Example:

```regex
\W+
```

Input:

```text
user_name123@example.com
```

Matches:

```text
@
.
```

**Important:** The exact Unicode behavior of `\w` varies between Regex engines. In JavaScript without Unicode property-based matching, and in many traditional Regex modes, it is primarily ASCII-oriented.

# 🗁 Whitespace Class: `\s` and `\S`

The `\s` shorthand matches whitespace characters.

Common examples include:

* Space.
* Tab.
* Newline.
* Carriage return.
* Other Unicode whitespace characters in engines that support them.

Example:

```regex
\s+
```

Matches one or more consecutive whitespace characters.

Input:

```text
Hello     World
```

Matches the whitespace between the words.

The `\S` shorthand matches any non-whitespace character.

Example:

```regex
\S+
```

Input:

```text
Hello World 123
```

Matches:

```text
Hello
World
123
```

# 🗁 The Dot Character

The dot (`.`) matches almost any single character except line terminators in many default Regex modes.

Example:

```regex
a.c
```

Matches:

```text
abc
a1c
a-c
```

Does not match:

```text
ac
abbc
```

The dot is not technically a character class, but it is another commonly used character-matching construct.

With a DotAll or single-line mode, the dot can also match line terminators. The flag and behavior depend on the Regex engine.

# 🗁 Escaping Characters Inside Character Classes

Some characters have special meanings inside square brackets.

For example:

* `]` closes the character class.
* `-` can define a range.
* `^` negates the class when it appears immediately after `[`.

To match these characters literally, escape them when necessary or position them where their special meaning does not apply.

Examples:

```regex
[\[\]]
```

Matches either a literal opening or closing square bracket.

```regex
[a\-z]
```

Matches `a`, `-`, or `z`.

```regex
[-a-z]
```

Matches a hyphen or any lowercase English letter.

```regex
[a-z-]
```

Also matches a hyphen or any lowercase English letter.

To match a literal caret:

```regex
[\^]
```

To match a literal closing bracket:

```regex
[\]]
```

Escaping rules can vary slightly between Regex flavors, so test patterns in the target engine.

# 🗁 Unicode Character Classes

Traditional character ranges such as `[a-z]` only describe the English lowercase alphabet.

They do not automatically include all letters from other writing systems.

For Unicode-aware matching, some Regex engines support Unicode property escapes.

Example in JavaScript:

```javascript
const pattern = /\p{L}+/gu;

console.log("Hello".match(pattern));
console.log("বাংলা".match(pattern));
console.log("日本語".match(pattern));
```

The `\p{L}` property matches Unicode letters.

Other useful Unicode properties include:

| Property | Meaning |
| --- | --- |
| `\p{L}` | Letters |
| `\p{N}` | Numbers |
| `\p{M}` | Combining marks |
| `\p{P}` | Punctuation |
| `\p{S}` | Symbols |
| `\p{Z}` | Separators |

JavaScript requires Unicode-aware mode (`u` or `v`) for Unicode property escapes.

Python supports Unicode character matching by default for many shorthand classes when using string patterns. It also provides Unicode-aware character properties through its third-party `regex` package.

# 🗁 Practical Examples

**Find English letters**

```regex
[a-zA-Z]+
```

**Find digits**

```regex
[0-9]+
```

**Find uppercase letters**

```regex
[A-Z]+
```

**Find lowercase letters**

```regex
[a-z]+
```

**Find letters and digits**

```regex
[a-zA-Z0-9]+
```

**Find characters other than letters and digits**

```regex
[^a-zA-Z0-9]+
```

**Find whitespace**

```regex
\s+
```

**Find non-whitespace sequences**

```regex
\S+
```

**Find hexadecimal characters**

```regex
[0-9a-fA-F]+
```

**Find a simple username character sequence**

```regex
[a-zA-Z0-9_]+
```

**Find text containing only digits**

```regex
^[0-9]+$
```

# 🗁 Character Classes in JavaScript

Example:

```javascript
const text = "Order 123, Product 456";

const pattern = /\d+/g;

console.log(text.match(pattern));
```

Output:

```text
["123", "456"]
```

Using a character range:

```javascript
const text = "Hello World 123";

const pattern = /[A-Z]+/g;

console.log(text.match(pattern));
```

Output:

```text
["H", "W"]
```

Using a negated character class:

```javascript
const text = "user_123@example.com";

const pattern = /[^a-zA-Z0-9]+/g;

console.log(text.match(pattern));
```

Output:

```text
["_", "@", "."]
```

# 🗁 Character Classes in Python

Example:

```python
import re

text = "Order 123, Product 456"

pattern = r"\d+"

print(re.findall(pattern, text))
```

Output:

```text
['123', '456']
```

Using a character range:

```python
import re

text = "Hello World 123"

pattern = r"[A-Z]+"

print(re.findall(pattern, text))
```

Output:

```text
['H', 'W']
```

Using a negated character class:

```python
import re

text = "user_123@example.com"

pattern = r"[^a-zA-Z0-9]+"

print(re.findall(pattern, text))
```

Output:

```text
['_', '@', '.']
```

# 🗁 Common Mistakes

**Using `^` in the wrong position**

```regex
[^0-9]
```

This matches a non-digit.

```regex
^0-9
```

This is not a digit-range negation. Outside a character class, `^` is an anchor.

**Assuming a character class matches a complete sequence**

```regex
[abc]
```

Matches one character, not the complete word `abc`.

Use:

```regex
[abc]+
```

to match one or more consecutive characters from that set.

**Confusing `\w` with all Unicode letters**

The meaning of `\w` depends on the engine and mode. Use Unicode properties when you need a clearly defined Unicode character category.

**Forgetting that ranges are inclusive**

```regex
[a-f]
```

Matches `a`, `b`, `c`, `d`, `e`, and `f`.

**Using an unescaped hyphen unintentionally**

```regex
[a-z]
```

Defines a range, not three literal characters.

Use `\-` or place the hyphen at the beginning or end when you want a literal hyphen.

# 🗁 Practice Exercises

**Exercise: Find All Digits**

Input:

```text
Order 123, Product 456, Quantity 10
```

Write a Regex pattern that extracts all numeric sequences.

**Exercise: Find Uppercase Letters**

Input:

```text
Hello WORLD Regex
```

Write a pattern that matches uppercase letters.

**Exercise: Exclude Digits**

Input:

```text
user123admin456
```

Write a pattern that matches sequences containing no digits.

**Exercise: Match Hexadecimal Characters**

Write a character class that matches hexadecimal characters from `0-9`, `a-f`, and `A-F`.

**Exercise: Match Username Characters**

Write a character class that allows English letters, digits, and underscores.

**Exercise: Match Unicode Letters**

Write a JavaScript Regex pattern that matches Unicode letters using `\p{L}`.

# 🗁 Quick Reference

| Pattern | Description |
| --- | --- |
| `[abc]` | Matches `a`, `b`, or `c` |
| `[^abc]` | Matches anything except `a`, `b`, or `c` |
| `[a-z]` | Matches lowercase English letters |
| `[A-Z]` | Matches uppercase English letters |
| `[0-9]` | Matches ASCII digits |
| `[a-zA-Z]` | Matches English letters |
| `[a-zA-Z0-9]` | Matches English letters and digits |
| `\d` | Matches a digit |
| `\D` | Matches a non-digit |
| `\w` | Matches a word character |
| `\W` | Matches a non-word character |
| `\s` | Matches whitespace |
| `\S` | Matches non-whitespace |
| `.` | Matches a character except line terminators in common default modes |
| `\p{L}` | Matches Unicode letters in supported Unicode-aware engines |

# 🗁 Final Notes

Character classes are essential for defining which characters a Regex pattern can accept.

Understand the difference between:

* A character class: `[abc]`
* A negated character class: `[^abc]`
* A range: `[a-z]`
* A shorthand class: `\d`, `\w`, and `\s`
* A Unicode property: `\p{L}`

Once you understand these concepts, you can combine character classes with quantifiers and anchors to build more powerful patterns.
