# Regex Basics

Regular Expressions (Regex) are sequences of characters that define search patterns. These patterns allow you to find, match, validate, extract, and replace text.

Instead of searching for an exact string, Regex lets you describe a pattern that can match multiple strings.

For example, you can use Regex to find:

* All email-like strings in a document.
* Phone numbers in a text file.
* Words that begin with a specific character.
* Numbers containing exactly five digits.
* URLs inside application logs.
* Repeated or unwanted whitespace.

# 🗁 What Is a Regular Expression?

A regular expression is a pattern that describes a set of strings.

Consider this text:

```text
Hello, my name is Muhammad.
Hello, my name is Ahmed.
Hello, my name is John.
```

The following Regex pattern:

```regex
Hello
```

Matches the word `Hello` in all three lines.

Now consider:

```regex
H.llo
```

The dot (`.`) is a special Regex character that matches one character in many common Regex engines.

It can match:

```text
Hello
Hallo
H3llo
H-llo
```

The pattern describes a matching rule rather than one exact string.

# 🗁 Literal Characters

Most ordinary characters match themselves.

For example:

| Regex | Matches |
| --- | --- |
| `cat` | `cat` |
| `hello` | `hello` |
| `admin` | `admin` |
| `192` | `192` |
| `example.com` | `example.com` except that `.` is a metacharacter |

Some characters have special meanings in Regex. These are called metacharacters.

# 🗁 Regex Metacharacters

Common metacharacters include:

| Character | Meaning |
| --- | --- |
| `.` | Matches any character except line terminators in many default modes |
| `^` | Matches the beginning of a line or input |
| `$` | Matches the end of a line or input |
| `*` | Matches zero or more repetitions |
| `+` | Matches one or more repetitions |
| `?` | Matches zero or one repetition |
| `{}` | Specifies repetition counts |
| `[]` | Defines a character class |
| `()` | Creates a group |
| `\` | Escapes a metacharacter or introduces a special sequence |
| ` | ` | Represents alternation (OR) |

These characters are explained in detail in the following files.

# 🗁 The Dot Metacharacter

The dot (`.`) matches a single character, except line terminators in many default Regex modes.

Example:

```regex
c.t
```

Matches:

```text
cat
cot
cut
c9t
c-t
```

It does not match `ct` because the pattern requires one character between `c` and `t`.

To match a literal dot, escape it:

```regex
\.
```

Example:

```regex
example\.com
```

Matches:

```text
example.com
```

It does not treat the dot as a wildcard.

# 🗁 Escaping Special Characters

A backslash (`\`) changes the meaning of a special character.

For example:

| Regex | Meaning |
| --- | --- |
| `\.` | Literal dot |
| `\+` | Literal plus sign |
| `\*` | Literal asterisk |
| `\?` | Literal question mark |
| `\(` | Literal opening parenthesis |
| `\)` | Literal closing parenthesis |
| `\\` | Literal backslash |

Example:

```regex
price: \$50
```

Matches:

```text
price: $50
```

The dollar sign is escaped because it normally has a special meaning in Regex.

# 🗁 Case Sensitivity

Regular expressions are generally case-sensitive by default.

Example:

```regex
hello
```

Matches:

```text
hello
```

Does not match:

```text
Hello
HELLO
```

You can enable case-insensitive matching with an appropriate flag.

Common case-insensitive flags:

| Regex Engine | Flag |
| --- | --- |
| JavaScript | `i` |
| Python | `re.IGNORECASE` or `re.I` |
| PCRE2 | `i` |

JavaScript example:

```javascript
const pattern = /hello/i;

console.log(pattern.test("hello")); // true
console.log(pattern.test("Hello")); // true
console.log(pattern.test("HELLO")); // true
```

Python example:

```python
import re

pattern = re.compile(r"hello", re.IGNORECASE)

print(bool(pattern.search("hello")))  # True
print(bool(pattern.search("Hello")))  # True
print(bool(pattern.search("HELLO")))  # True
```

# 🗁 Matching and Searching

Matching and searching are related but different operations.

**Searching**

Searching looks for a pattern anywhere within the input.

Example:

```regex
cat
```

Input:

```text
The cat is sleeping.
```

The pattern is found inside the sentence.

**Matching the Entire Input**

Some programming APIs provide methods that require the entire input to match the pattern.

For example, JavaScript's `test()` checks whether a match exists anywhere unless the pattern uses anchors or other constraints.

To match an entire string, use an appropriate API or anchor the pattern.

```regex
^cat$
```

This matches only the complete string `cat` in a single-line context.

# 🗁 Regex Flags

Flags modify how a regular expression behaves.

Common flags include:

| Flag | Name | Meaning |
| --- | --- | --- |
| `i` | Case-insensitive | Ignores letter case |
| `g` | Global | Finds multiple matches in JavaScript |
| `m` | Multiline | Makes `^` and `$` work at line boundaries |
| `s` | DotAll | Allows `.` to match line terminators |
| `u` | Unicode | Enables Unicode-aware matching in JavaScript |
| `y` | Sticky | Matches at the current position in JavaScript |

Example using JavaScript:

```javascript
const text = "cat CAT Cat";

const pattern = /cat/gi;

console.log(text.match(pattern));
```

Output:

```text
["cat", "CAT", "Cat"]
```

The `g` flag finds multiple matches, while the `i` flag ignores case.

Flag support and behavior vary across Regex engines.

# 🗁 Regex in JavaScript

JavaScript supports regular expressions through literals and the `RegExp` constructor.

**Regex Literal**

```javascript
const pattern = /hello/i;
```

**RegExp Constructor**

```javascript
const pattern = new RegExp("hello", "i");
```

The literal form is generally convenient when the pattern is known in advance.

The constructor is useful when a pattern must be created dynamically.

**Common JavaScript Regex Methods**

| Method | Purpose |
| --- | --- |
| `test()` | Checks whether a match exists |
| `exec()` | Returns detailed match information |
| `match()` | Retrieves matching text |
| `matchAll()` | Returns an iterator of all matches |
| `search()` | Returns the index of the first match |
| `replace()` | Replaces matching text |
| `replaceAll()` | Replaces all matching occurrences |
| `split()` | Splits a string using a Regex pattern |

Example:

```javascript
const text = "My username is admin123";

const pattern = /admin/;

console.log(pattern.test(text));
console.log(text.search(pattern));
console.log(text.match(pattern));
console.log(text.replace(pattern, "user"));
```

# 🗁 Regex in Python

Python provides the built-in `re` module for regular expressions.

Import the module:

```python
import re
```

**Common Python Regex Functions**

| Function | Purpose |
| --- | --- |
| `re.search()` | Finds the first match anywhere |
| `re.match()` | Checks for a match at the beginning |
| `re.fullmatch()` | Requires the entire string to match |
| `re.findall()` | Returns all non-overlapping matches |
| `re.finditer()` | Returns match objects for all matches |
| `re.sub()` | Replaces matching text |
| `re.split()` | Splits text using a Regex pattern |
| `re.compile()` | Compiles a reusable Regex pattern |

Example:

```python
import re

text = "My username is admin123"

pattern = re.compile(r"admin")

print(pattern.search(text))
print(pattern.findall(text))
print(pattern.sub("user", text))
```

Use raw strings such as `r"\d+"` for Python Regex patterns. They help avoid confusion between Python string escapes and Regex escapes.

# 🗁 Regex in Linux

Linux command-line tools use regular expressions for searching and processing text.

**grep**

Search for a word:

```bash
grep 'error' application.log
```

Use extended regular expressions:

```bash
grep -E 'error|warning' application.log
```

Ignore letter case:

```bash
grep -i 'error' application.log
```

Search recursively:

```bash
grep -r 'database' ./logs/
```

**sed**

Replace text:

```bash
sed 's/error/warning/' application.log
```

Replace all occurrences on each line:

```bash
sed 's/error/warning/g' application.log
```

**awk**

Match a pattern:

```bash
awk '/error/ { print $0 }' application.log
```

These tools support different Regex dialects. For example, traditional `grep` uses Basic Regular Expressions, while `grep -E` uses Extended Regular Expressions.

# 🗁 Common Beginner Mistakes

**Forgetting to escape special characters**

Incorrect:

```regex
example.com
```

This treats the dot as a wildcard.

Correct:

```regex
example\.com
```

**Confusing searching with full-string validation**

Pattern:

```regex
[0-9]+
```

This can find digits inside a larger string.

For a complete numeric string, use:

```regex
^[0-9]+$
```

**Assuming all Regex engines behave identically**

Different engines support different syntax, flags, and features.

Always check the target engine.

**Using overly complicated patterns**

A long Regex pattern can become difficult to maintain.

Break complex patterns into understandable components and document their purpose.

# 🗁 Practice Exercises

**Exercise: Match a Word**

Write a Regex pattern that finds the word `network` inside a sentence.

Example input:

```text
I am learning network engineering.
```

Expected match:

```text
network
```

**Exercise: Match a Literal Dot**

Write a Regex pattern that matches:

```text
example.com
```

The dot must be treated as a literal character.

**Exercise: Case-Insensitive Matching**

Write a Regex pattern that matches all of these:

```text
admin
Admin
ADMIN
```

**Exercise: Find Numbers**

Given:

```text
Order 123, Product 456, Quantity 10
```

Write a Regex pattern that finds all numeric sequences.

**Exercise: Find Multiple Words**

Given:

```text
The server is running. The network is available.
```

Write a Regex pattern that finds either `server` or `network`.

# 🗁 Quick Reference

| Pattern | Description |
| --- | --- |
| `cat` | Matches literal text `cat` |
| `.` | Matches one character except line terminators in common default modes |
| `\.` | Matches a literal dot |
| `\+` | Matches a literal plus sign |
| `^` | Beginning of input or line, depending on mode |
| `$` | End of input or line, depending on mode |
| `\|` | Alternation (OR) |
| `()` | Grouping |
| `[]` | Character class |
| `*` | Zero or more repetitions |
| `+` | One or more repetitions |
| `?` | Zero or one repetition |

# 🗁 Final Notes

Regex is a powerful tool for working with text. Start with literal matching, understand metacharacters, and practice using simple patterns before combining advanced features.

Remember that Regex syntax can vary between programming languages and command-line tools. Always test your patterns in the target environment.
