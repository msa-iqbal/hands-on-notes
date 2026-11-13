# Regex Groups

Groups allow you to combine multiple Regex elements into a single unit.

They are created using parentheses `()` and are useful for organizing patterns, applying quantifiers to multiple elements, capturing matched text, and defining alternatives.

Groups are essential when building complex Regex patterns.

Common use cases include:

* Grouping related characters and expressions.
* Extracting specific portions of text.
* Reusing previously matched text.
* Matching alternative patterns.
* Creating named captures.
* Applying quantifiers to complete expressions.
* Structuring complex validation patterns.

# 🗁 What Are Regex Groups?

Parentheses `()` create a capturing group by default.

Example:

```regex
(ab)+
```

Matches:

```text
ab
abab
ababab
```

Explanation:

* `(ab)` creates a group containing `ab`.
* `+` applies to the entire group.
* The group must appear at least once.

Without grouping:

```regex
ab+
```

Only the character `b` is repeated.

Matches:

```text
ab
abb
abbb
```

Grouping changes how Regex elements are combined and repeated.

# 🗁 Capturing Groups

A capturing group stores the portion of text matched by the expression inside its parentheses.

Example:

```regex
(\d{4})-(\d{2})-(\d{2})
```

Input:

```text
2026-11-12
```

Captured groups:

| Group | Captured Value | Meaning |
| --- | --- | --- |
| Group 1 | `2026` | Year |
| Group 2 | `11` | Month |
| Group 3 | `12` | Day |

The complete match is:

```text
2026-11-12
```

The individual groups allow you to access specific portions of the matched text.

**JavaScript example:**

```javascript
const text = "2026-11-12";

const pattern = /(\d{4})-(\d{2})-(\d{2})/;

const result = text.match(pattern);

console.log(result[0]); // 2026-11-12
console.log(result[1]); // 2026
console.log(result[2]); // 11
console.log(result[3]); // 12
```

**Python example:**

```python
import re

text = "2026-11-12"

pattern = r"(\d{4})-(\d{2})-(\d{2})"

result = re.search(pattern, text)

if result:
    print(result.group(0))  # 2026-11-12
    print(result.group(1))  # 2026
    print(result.group(2))  # 11
    print(result.group(3))  # 12
```

# 🗁 Multiple Capturing Groups

You can create multiple groups inside one Regex pattern.

Example:

```regex
(\w+)@(\w+)\.(\w+)
```

Input:

```text
admin@example.com
```

Captured groups:

| Group | Value |
| --- | --- |
| Group 0 | `admin@example.com` |
| Group 1 | `admin` |
| Group 2 | `example` |
| Group 3 | `com` |

Group 0 represents the complete match in common programming APIs. It is not an explicitly declared capturing group.

Another example:

```regex
(\d+)\s+([a-zA-Z]+)
```

Input:

```text
123 products
```

Captured groups:

* Group 1: `123`
* Group 2: `products`

This approach is useful when extracting structured information from text.

# 🗁 Non-Capturing Groups

A non-capturing group groups Regex elements without storing the matched text as a capture.

Syntax:

```regex
(?:pattern)
```

Example:

```regex
(?:ab)+
```

Matches:

```text
ab
abab
ababab
```

However, it does not create a numbered capturing group.

**Capturing group:**

```regex
(ab)+
```

**Non-capturing group:**

```regex
(?:ab)+
```

Both can match the same text, but only the first captures the repeated group.

Non-capturing groups are useful when you need grouping for structure or quantifiers but do not need to retrieve the matched portion separately.

# 🗁 Capturing vs Non-Capturing Groups

| Feature | Capturing `()` | Non-Capturing `(?:)` |
| --- | --- | --- |
| Groups expressions | Yes | Yes |
| Applies quantifiers | Yes | Yes |
| Stores captured text | Yes | No |
| Creates numbered group | Yes | No |
| Supports alternation | Yes | Yes |
| Supports nesting | Yes | Yes |

Example:

```regex
^(https?)://([^/]+)(?:/.*)?$
```

Input:

```text
https://example.com/products
```

Captured groups:

* Group 1: `https`
* Group 2: `example.com`

The optional path group is non-capturing because its value is not needed separately.

# 🗁 Named Capturing Groups

Named capturing groups allow you to assign meaningful names to captured portions of text.

They make complex Regex patterns easier to understand and maintain.

**Common syntax:**

```regex
(?<name>pattern)
```

This syntax is supported by JavaScript and several other engines.

Example:

```regex
(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})
```

Input:

```text
2026-11-12
```

Captured values:

| Group Name | Value |
| --- | --- |
| `year` | `2026` |
| `month` | `11` |
| `day` | `12` |

**JavaScript example:**

```javascript
const text = "2026-11-12";

const pattern = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

const result = text.match(pattern);

console.log(result.groups.year);
console.log(result.groups.month);
console.log(result.groups.day);
```

Output:

```text
2026
11
12
```

**Python example:**

Python uses a different named-group syntax:

```regex
(?P<name>pattern)
```

Example:

```python
import re

text = "2026-11-12"

pattern = r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})"

result = re.search(pattern, text)

if result:
    print(result.group("year"))
    print(result.group("month"))
    print(result.group("day"))
```

Output:

```text
2026
11
12
```

# 🗁 Group Numbering

Capturing groups are generally numbered according to the order of their opening parentheses.

Example:

```regex
(\d+)-([a-z]+)-(\w+)
```

Input:

```text
123-admin-user
```

Group numbering:

| Group | Captured Value |
| --- | --- |
| Group 0 | `123-admin-user` |
| Group 1 | `123` |
| Group 2 | `admin` |
| Group 3 | `user` |

Non-capturing groups do not receive capturing group numbers.

Example:

```regex
(\d+)-(?:[a-z]+)-(\w+)
```

Input:

```text
123-admin-user
```

Captured groups:

| Group | Captured Value |
| --- | --- |
| Group 0 | `123-admin-user` |
| Group 1 | `123` |
| Group 2 | `user` |

The non-capturing group does not affect the numbering of subsequent capturing groups.

# 🗁 Nested Groups

Groups can be placed inside other groups.

Example:

```regex
((\d{4})-(\d{2}))-(\d{2})
```

Input:

```text
2026-11-12
```

Captured groups:

| Group | Value |
| --- | --- |
| Group 1 | `2026-11` |
| Group 2 | `2026` |
| Group 3 | `11` |
| Group 4 | `12` |

Nested groups are useful for organizing complex expressions.

However, excessive nesting can make patterns difficult to read.

Prefer named groups when the captured values have specific meanings.

# 🗁 Alternation Inside Groups

The pipe character (`|`) represents alternation, meaning OR.

Example:

```regex
(cat|dog)
```

Matches either:

```text
cat
dog
```

You can combine alternation with other pattern elements.

Example:

```regex
I like (tea|coffee)
```

Matches:

```text
I like tea
I like coffee
```

Another example:

```regex
^(https?|ftp)://
```

Matches strings beginning with:

```text
http://
https://
ftp://
```

Parentheses make the alternatives part of one expression.

Without grouping:

```regex
^https?|ftp://
```

The pattern has a different meaning because alternation has lower precedence than concatenation.

# 🗁 Alternation with Non-Capturing Groups

If you do not need to capture the selected alternative, use a non-capturing group.

Example:

```regex
^(?:GET|POST|PUT|DELETE)$
```

Matches:

```text
GET
POST
PUT
DELETE
```

Does not match:

```text
PATCH
OPTIONS
```

This is useful when validating a value against a predefined set of alternatives.

# 🗁 Backreferences

A backreference allows you to match the same text that was previously captured by a group.

A numbered backreference uses a backslash followed by the group number.

Example:

```regex
(\w+)\s+\1
```

Input:

```text
hello hello
hello world
test test
```

Matches:

```text
hello hello
test test
```

Explanation:

* `(\w+)` captures a word.
* `\s+` matches whitespace.
* `\1` matches exactly the text captured by Group 1.

The second word must be identical to the first captured word.

Backreferences are useful for finding repeated words, duplicated values, and matching paired text patterns.

# 🗁 Multiple Backreferences

You can reference multiple capturing groups.

Example:

```regex
(\w+)-(\d+)-\1
```

Matches:

```text
admin-123-admin
user-456-user
```

Does not match:

```text
admin-123-user
```

Explanation:

* Group 1 captures a word.
* Group 2 captures digits.
* `\1` requires the original word to appear again.

# 🗁 Named Backreferences

Named capturing groups can also be referenced by name.

Common syntax:

```regex
(?<name>pattern)
\k<name>
```

Example:

```regex
(?<word>\w+)\s+\k<word>
```

Matches repeated words such as:

```text
hello hello
test test
```

Python uses a different syntax:

```regex
(?P<word>\w+)\s+(?P=word)
```

Named backreferences improve readability when a pattern contains several groups.

# 🗁 Backreferences in JavaScript

Example:

```javascript
const text = "hello hello";

const pattern = /(\w+)\s+\1/;

console.log(pattern.test(text));
```

Output:

```text
true
```

Using a named backreference:

```javascript
const text = "hello hello";

const pattern = /(?<word>\w+)\s+\k<word>/;

console.log(pattern.test(text));
```

Output:

```text
true
```

# 🗁 Backreferences in Python

Example:

```python
import re

text = "hello hello"

pattern = r"(\w+)\s+\1"

print(bool(re.search(pattern, text)))
```

Output:

```text
True
```

Using a named backreference:

```python
import re

text = "hello hello"

pattern = r"(?P<word>\w+)\s+(?P=word)"

print(bool(re.search(pattern, text)))
```

Output:

```text
True
```

# 🗁 Atomic Groups

Atomic groups prevent the Regex engine from backtracking into a group after it has successfully matched.

Common syntax:

```regex
(?>pattern)
```

Atomic groups are supported by engines such as PCRE2 and Python's `re` module in modern versions, but not by every Regex engine.

Example:

```regex
(?>a+)a
```

Input:

```text
aaa
```

The atomic group consumes all three `a` characters.

It cannot give one character back to satisfy the final `a`, so the pattern fails.

Compare it with:

```regex
(a+)a
```

The normal capturing group can backtrack, allowing the final `a` to match.

Atomic groups can help control backtracking and improve performance in specific patterns.

They should not be used without understanding how they change matching behavior.

# 🗁 Grouping Best Practices

* Use capturing groups when you need to extract matched values.
* Use non-capturing groups when you only need logical grouping.
* Use named groups when captured values have meaningful names.
* Keep nested groups as simple as possible.
* Use backreferences only when matching previously captured text is necessary.
* Avoid unnecessary capturing groups in large patterns.
* Test atomic groups carefully because they can change whether a pattern matches.

# 🗁 Practical Examples

**Extract a date into separate components:**

```regex
(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})
```

**Match HTTP methods:**

```regex
^(?:GET|POST|PUT|PATCH|DELETE)$
```

**Find repeated words:**

```regex
\b(\w+)\s+\1\b
```

**Match a simple key-value pair:**

```regex
(\w+)\s*=\s*(\w+)
```

Input:

```text
username=admin
```

Captured values:

* Group 1: `username`
* Group 2: `admin`

**Match an optional HTTP scheme and capture the hostname:**

```regex
^(?:https?://)?([^/]+)$
```

**Match a version number and capture its components:**

```regex
(\d+)\.(\d+)\.(\d+)
```

Input:

```text
2.5.10
```

Captured values:

* Group 1: `2`
* Group 2: `5`
* Group 3: `10`

**Match duplicated text:**

```regex
^(.+)\s+\1$
```

Matches:

```text
hello hello
admin admin
```

# 🗁 Common Mistakes

**Using capturing groups unnecessarily**

```regex
(\d+)-([a-z]+)
```

If you do not need the captured values, consider:

```regex
(?:\d+)-(?:[a-z]+)
```

**Confusing group repetition with backreferences**

```regex
(ab)+
```

Repeats the group.

```regex
(ab)\1
```

Matches the captured text again.

**Using the wrong named-group syntax**

JavaScript:

```regex
(?<name>\w+)
```

Python:

```regex
(?P<name>\w+)
```

**Forgetting that group numbering follows opening parentheses**

Nested capturing groups can change the numbering of subsequent groups.

**Assuming all engines support the same group features**

Named groups, atomic groups, and named backreferences can have different syntax or support across Regex engines.

# 🗁 Practice Exercises

**Exercise: Extract Date Components**

Given:

```text
2026-11-12
```

Create three capturing groups for the year, month, and day.

**Exercise: Non-Capturing Group**

Write a pattern that matches:

```text
http://
https://
```

Use a non-capturing group for the optional `s`.

**Exercise: Match HTTP Methods**

Create a pattern that accepts:

```text
GET
POST
PUT
DELETE
```

but rejects other methods.

**Exercise: Find Repeated Words**

Given:

```text
hello hello world test test
```

Write a pattern that finds repeated consecutive words.

**Exercise: Named Groups**

Write a Python Regex pattern that extracts a username and domain from:

```text
admin@example.com
```

Use named capturing groups.

**Exercise: Backreferences**

Write a pattern that matches:

```text
admin-123-admin
user-456-user
```

The first and last words must be identical.

**Exercise: Nested Groups**

Create a pattern that captures the major, minor, and patch components of:

```text
1.2.3
10.20.30
```

# 🗁 Quick Reference

| Syntax | Description |
| --- | --- |
| `(pattern)` | Capturing group |
| `(?:pattern)` | Non-capturing group |
| `(?<name>pattern)` | Named group in JavaScript and several engines |
| `(?P<name>pattern)` | Named group in Python |
| `\1` | Backreference to Group 1 |
| `\2` | Backreference to Group 2 |
| `\k<name>` | Named backreference in JavaScript and several engines |
| `(?P=name)` | Named backreference in Python |
| `(?>pattern)` | Atomic group in supported engines |
| `(a | b)` | Captures either alternative `a` or `b` |

# 🗁 Final Notes

Groups make Regex patterns more organized, reusable, and powerful.

Understand these key concepts:

* Capturing groups store matched text.
* Non-capturing groups organize expressions without storing matches.
* Named groups provide meaningful identifiers.
* Nested groups organize complex expressions.
* Alternation allows multiple matching alternatives.
* Backreferences match previously captured text.
* Atomic groups restrict backtracking in supported engines.

Groups are especially useful when extracting structured information from unstructured text.
