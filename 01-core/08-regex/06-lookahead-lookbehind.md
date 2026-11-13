# Regular Expressions: Lookahead and Lookbehind

Lookahead and lookbehind are **zero-width assertions** in regular expressions. They allow you to check whether a pattern exists before or after the current matching position without including the checked text in the final match.

They are useful when matching text based on surrounding conditions.

# 🗁 What Are Zero-Width Assertions?

A normal regex pattern consumes characters when it matches them.

For example:

```regex
cat
```

Matches the three characters `cat`.

A lookaround checks a condition at a particular position but does not consume characters.

For example:

```regex
cat(?=s)
```

Matches `cat` only when it is immediately followed by `s`.

Given:

```text
cats
cat
category
```

The pattern matches `cat` in `cats` and `category`? No. It matches only `cat` in `cats`, because the next character must be `s`.

The matched result does not include the `s`.

# 🗁 Types of Lookaround

There are four main types:

| Type | Syntax | Meaning |
| --- | --- | --- |
| Positive lookahead | `(?=pattern)` | The following text must match the pattern |
| Negative lookahead | `(?!pattern)` | The following text must not match the pattern |
| Positive lookbehind | `(?<=pattern)` | The preceding text must match the pattern |
| Negative lookbehind | `(?<!pattern)` | The preceding text must not match the pattern |

# 🗁 Positive Lookahead

Positive lookahead checks whether a pattern exists immediately after the current position.

Syntax:

```regex
(?=pattern)
```

Example:

```regex
\d+(?=px)
```

Matches digits only when they are followed by `px`.

Input:

```text
10px
25px
30em
40px
```

Matches:

```text
10
25
40
```

The `px` text is checked but not included in the match.

Another example:

```regex
\w+(?=@example\.com)
```

Input:

```text
alice@example.com
bob@gmail.com
john@example.com
```

Matches:

```text
alice
john
```

The email domain is checked but not consumed.

# 🗁 Negative Lookahead

Negative lookahead checks that a pattern does not exist immediately after the current position.

Syntax:

```regex
(?!pattern)
```

Example:

```regex
\w+(?!\.txt)
```

This pattern can be misleading because `\w+` may backtrack and match a shorter portion of a word to satisfy the assertion.

For a clearer example, consider matching `foo` only when it is not followed by `bar`:

```regex
foo(?!bar)
```

Input:

```text
foobar
foobaz
foo
```

Matches:

```text
foo
foo
```

It does not match the `foo` in `foobar`.

Another example:

```regex
^(?!admin$)\w+$
```

This matches a username containing word characters but excludes the exact username `admin`.

Matches:

```text
john
developer
user123
```

Does not match:

```text
admin
```

# 🗁 Positive Lookbehind

Positive lookbehind checks whether a pattern exists immediately before the current position.

Syntax:

```regex
(?<=pattern)
```

Example:

```regex
(?<=\$)\d+
```

Input:

```text
$100
$250
€300
$500
```

Matches:

```text
100
250
500
```

The dollar sign is required before the digits but is not included in the match.

Another example:

```regex
(?<=ID-)\d+
```

Input:

```text
ID-101
ID-205
USER-301
ID-450
```

Matches:

```text
101
205
450
```

# 🗁 Negative Lookbehind

Negative lookbehind checks that a pattern does not exist immediately before the current position.

Syntax:

```regex
(?<!pattern)
```

Example:

```regex
(?<!\$)\d+
```

Matches digits that are not immediately preceded by `$`.

Input:

```text
$100
200
$300
400
```

Matches:

```text
200
400
```

Another example:

```regex
(?<!ID-)\d+
```

This matches digits that are not immediately preceded by `ID-`.

Input:

```text
ID-101
USER-202
ID-303
CODE-404
```

Matches:

```text
202
404
```

# 🗁 Combining Lookarounds with Consuming Patterns

Lookarounds can be combined with ordinary regex tokens.

Consider:

```regex
(?<=\$)\d+(?=USD)
```

This pattern requires:

* A dollar sign before the digits.
* One or more digits.
* `USD` immediately after the digits.

Input:

```text
$100USD
$250USD
€300USD
$400EUR
```

Matches:

```text
100
250
```

Both surrounding conditions are checked, but neither the dollar sign nor `USD` is included in the result.

Another example:

```regex
(?<!-)\d+(?!-)
```

This matches digits that are not immediately preceded or followed by a hyphen.

Input:

```text
123
-456
789-
101
```

Matches:

```text
123
101
```

# 🗁 Password Validation with Multiple Lookaheads

Lookaheads are especially useful for validating multiple password requirements.

Suppose a password must contain:

* At least eight characters.
* At least one lowercase letter.
* At least one uppercase letter.
* At least one digit.
* At least one special character.

Pattern:

```regex
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]).{8,}$
```

Explanation:

| Pattern | Requirement |
| --- | --- |
| `^` | Start of input |
| `(?=.*[a-z])` | At least one lowercase letter |
| `(?=.*[A-Z])` | At least one uppercase letter |
| `(?=.*\d)` | At least one digit |
| `(?=.*[^A-Za-z0-9])` | At least one special character |
| `.{8,}` | At least eight characters |
| `$` | End of input |

Examples:

| Password | Result |
| --- | --- |
| `Hello123!` | Valid |
| `Developer9@` | Valid |
| `hello123!` | Invalid: no uppercase letter |
| `HELLO123!` | Invalid: no lowercase letter |
| `HelloWorld!` | Invalid: no digit |
| `Hello1234` | Invalid: no special character |
| `Ab1!` | Invalid: too short |

This is a useful starting point for password policy validation. Production applications should also define their allowed characters, maximum length, and password-handling policy explicitly.

# 🗁 Excluding Specific Prefixes and Suffixes

Lookarounds can exclude text based on what appears around a match.

Exclude words beginning with `un`:

```regex
\b(?!un)\w+\b
```

Input:

```text
happy
unhappy
developer
unknown
```

Matches:

```text
happy
developer
```

Exclude words ending with `ing`:

```regex
\b\w+(?<!ing)\b
```

Input:

```text
coding
developer
testing
design
```

Matches:

```text
developer
design
```

The negative lookbehind checks the characters immediately before the word boundary. In this example, it prevents a word ending in `ing` from matching.

# 🗁 JavaScript Examples

JavaScript supports lookahead and lookbehind in modern regex engines.

```javascript
const text = "$100 $250 €300 $400";

const regex = /(?<=\$)\d+/g;

console.log(text.match(regex));
```

Output:

```javascript
["100", "250", "400"]
```

Positive lookahead:

```javascript
const text = "10px 20em 30px";

const regex = /\d+(?=px)/g;

console.log(text.match(regex));
```

Output:

```javascript
["10", "30"]
```

Negative lookahead:

```javascript
const text = "foobar foobaz foo";

const regex = /foo(?!bar)/g;

console.log(text.match(regex));
```

Output:

```javascript
["foo", "foo"]
```

Password validation:

```javascript
const passwordRegex =
  /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]).{8,}$/;

console.log(passwordRegex.test("Hello123!"));
console.log(passwordRegex.test("hello123!"));
```

Output:

```text
true
false
```

# 🗁 Python Examples

Python's built-in `re` module supports lookahead and fixed-width lookbehind.

Positive lookbehind:

```python
import re

text = "$100 $250 €300 $400"

matches = re.findall(r"(?<=\$)\d+", text)

print(matches)
```

Output:

```text
['100', '250', '400']
```

Positive lookahead:

```python
import re

text = "10px 20em 30px"

matches = re.findall(r"\d+(?=px)", text)

print(matches)
```

Output:

```text
['10', '30']
```

Negative lookahead:

```python
import re

text = "foobar foobaz foo"

matches = re.findall(r"foo(?!bar)", text)

print(matches)
```

Output:

```text
['foo', 'foo']
```

Password validation:

```python
import re

password_regex = (
    r"^(?=.*[a-z])"
    r"(?=.*[A-Z])"
    r"(?=.*\d)"
    r"(?=.*[^A-Za-z0-9])"
    r".{8,}$"
)

password = "Hello123!"

print(bool(re.match(password_regex, password)))
```

Output:

```text
True
```

# 🗁 Variable-Length Lookbehind Support

Lookbehind support differs between regex engines.

| Regex engine | Lookbehind support |
| --- | --- |
| JavaScript modern engines | Supports lookbehind, subject to engine-specific constraints |
| Python `re` | Generally requires fixed-width lookbehind |
| PCRE2 | Supports certain variable-length lookbehinds, subject to restrictions and configuration |
| .NET | Supports variable-length lookbehind |
| Java | Supports variable-length lookbehind with engine-specific restrictions |

For example:

```regex
(?<=\$|€)\d+
```

The lookbehind alternatives `$` and `€` are each one character, so this is fixed-width.

However:

```regex
(?<=USD|EUR)\d+
```

The alternatives have different lengths. Python's built-in `re` rejects this because its lookbehind must have a fixed width.

A practical Python alternative is to capture the prefix:

```python
import re

text = "USD100 EUR200"

matches = re.findall(r"(?:USD|EUR)(\d+)", text)

print(matches)
```

Output:

```text
['100', '200']
```

Always check the regex engine's documentation when using advanced lookbehind expressions.

# 🗁 Lookarounds vs Capturing Groups

Lookarounds and capturing groups serve different purposes.

**Capturing group:**

```regex
(\$)\d+
```

The dollar sign is consumed and captured as a group.

**Lookbehind:**

```regex
(?<=\$)\d+
```

The dollar sign is checked but is not part of the match.

Comparison:

| Feature | Capturing group | Lookaround |
| --- | --- | --- |
| Consumes characters | Yes | No |
| Includes text in overall match | Yes | No |
| Can capture text | Yes | No |
| Checks surrounding conditions | Not by itself | Yes |

Backreferences reuse previously captured text. They are different from lookarounds.

Example:

```regex
\b(\w+)\s+\1\b
```

This matches repeated words such as:

```text
hello hello
```

Here, `\1` refers to the text captured by the first group.

# 🗁 Performance and Readability

Lookarounds are powerful, but complicated expressions can become difficult to maintain.

Consider these practices:

* Use lookarounds when surrounding conditions should not be included in the match.
* Prefer simple patterns over deeply nested assertions.
* Avoid unnecessary lookaheads that repeat expensive matching work.
* Test positive and negative cases.
* Consider capturing groups when extracting prefixes or suffixes is acceptable.
* Check compatibility with the target regex engine.
* Use a parser instead of regex when processing structured languages.

A readable regex is generally easier to debug and maintain than a clever but unnecessarily complicated one.

# 🗁 Common Mistakes

**Including checked text in the match**

Incorrect assumption:

```regex
\d+(?=px)
```

This does not match `10px` as a whole. It matches only `10`.

**Using negative lookahead without considering backtracking**

Pattern:

```regex
\w+(?!\.txt)
```

This may match a shorter portion of a filename because `\w+` can backtrack.

If you need to exclude an entire filename ending in `.txt`, use a more carefully anchored pattern:

```regex
^(?!.*\.txt$).+$
```

**Using unsupported variable-length lookbehind**

Python's `re` module rejects many variable-length lookbehind patterns. Use a capture group or restructure the expression.

**Confusing lookahead with lookbehind**

* Lookahead checks text after the current position.
* Lookbehind checks text before the current position.

**Forgetting zero-width behavior**

Lookarounds do not consume characters. They only verify conditions at a matching position.

# 🗁 Practice Exercises

Try writing patterns for these requirements:

* Match numbers followed by `kg` without including `kg`.
* Match `admin` only when it is not followed by `123`.
* Match digits preceded by `ID-` without including `ID-`.
* Match numbers that are not preceded by `$`.
* Match words that do not end with `ing`.
* Validate passwords containing lowercase, uppercase, digits, and special characters.
* Match `http` only when it is followed by `://`.

Suggested answers:

```regex
\d+(?=kg)
```

```regex
admin(?!123)
```

```regex
(?<=ID-)\d+
```

```regex
(?<!\$)\d+
```

```regex
\b\w+(?<!ing)\b
```

```regex
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]).{8,}$
```

```regex
http(?=://)
```

# 🗁 Key Takeaways

* Lookahead checks what comes after a matching position.
* Lookbehind checks what comes before a matching position.
* Positive assertions require a condition to be true.
* Negative assertions require a condition to be false.
* Lookarounds are zero-width and do not consume characters.
* Multiple lookaheads can enforce several conditions simultaneously.
* Regex engine compatibility matters, especially for variable-length lookbehind.
* Use lookarounds when they improve matching precision without making the pattern unnecessarily complex.
