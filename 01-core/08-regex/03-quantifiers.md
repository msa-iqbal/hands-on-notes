# Regex Quantifiers

Quantifiers specify how many times a character, character class, group, or other pattern element must occur.

Without quantifiers, a Regex pattern generally matches a fixed sequence of characters.

With quantifiers, you can match patterns containing variable numbers of characters.

For example:

```regex
a+
```

Matches one or more consecutive occurrences of the character `a`.

It can match:

```text
a
aa
aaa
aaaa
```

Quantifiers are essential for:

* Matching numbers with different lengths.
* Validating usernames and passwords.
* Matching repeated characters.
* Extracting words and identifiers.
* Defining optional parts of a pattern.
* Controlling how much text a pattern consumes.

# 🗁 What Is a Quantifier?

A quantifier controls the repetition count of the preceding Regex element.

Example:

```regex
a+
```

Here:

* `a` is the pattern element.
* `+` is the quantifier.
* The pattern matches one or more consecutive `a` characters.

Quantifiers apply to the immediately preceding character, character class, or group.

Example:

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

# 🗁 Zero or More: `*`

The asterisk (`*`) matches zero or more occurrences of the preceding element.

Syntax:

```regex
pattern*
```

Example:

```regex
ab*
```

Matches:

```text
a
ab
abb
abbb
abbbb
```

Explanation:

* `a` is required.
* `b` is optional.
* `b` can occur any number of times, including zero.

Another example:

```regex
[0-9]*
```

Matches zero or more consecutive digits.

Input:

```text
123abc456
```

The pattern can match digit sequences, but because zero repetitions are allowed, it can also produce empty matches at positions where no digit is present.

This behavior matters when using `*` with search and replacement operations.

# 🗁 One or More: `+`

The plus sign (`+`) matches one or more occurrences of the preceding element.

Syntax:

```regex
pattern+
```

Example:

```regex
ab+
```

Matches:

```text
ab
abb
abbb
abbbb
```

Does not match:

```text
a
```

Explanation:

* `a` is required.
* At least one `b` is required.
* Additional `b` characters are allowed.

Another example:

```regex
\d+
```

Matches one or more consecutive digits.

Input:

```text
ID 123, Product 456, Quantity 10
```

Matches:

```text
123
456
10
```

# 🗁 Zero or One: `?`

The question mark (`?`) matches zero or one occurrence of the preceding element.

Syntax:

```regex
pattern?
```

Example:

```regex
colou?r
```

Matches:

```text
color
colour
```

Explanation:

* `colo` is required.
* `u` is optional.
* `r` is required.

Another example:

```regex
https?
```

Matches:

```text
http
https
```

This is useful when a character or part of a pattern is optional.

# 🗁 Exact Repetition: `{n}`

Curly braces allow you to specify an exact repetition count.

Syntax:

```regex
pattern{n}
```

Here, `n` represents the required number of repetitions.

Example:

```regex
\d{4}
```

Matches exactly four consecutive digits.

Input:

```text
Year 2026, Code 12345, ID 789
```

Finds:

```text
2026
1234
789
```

Notice that `12345` contains five digits, so an unanchored search can still find a four-digit substring within it.

To require the entire input to contain exactly four digits, use:

```regex
^\d{4}$
```

# 🗁 Minimum Repetition: `{n,}`

This quantifier matches at least `n` repetitions.

Syntax:

```regex
pattern{n,}
```

Example:

```regex
\d{3,}
```

Matches three or more consecutive digits.

Input:

```text
12 123 1234 12345
```

Matches:

```text
123
1234
12345
```

It does not match `12` because that sequence contains fewer than three digits.

# 🗁 Repetition Range: `{n,m}`

This quantifier matches between `n` and `m` repetitions, inclusive.

Syntax:

```regex
pattern{n,m}
```

Example:

```regex
\d{2,4}
```

Matches between two and four consecutive digits.

Input:

```text
1 12 123 1234 12345
```

Possible matches:

```text
12
123
1234
1234
```

The final `12345` contains five digits, but an unanchored search can match its first four digits.

To require the entire input to contain between two and four digits:

```regex
^\d{2,4}$
```

# 🗁 Quantifier Comparison

| Quantifier | Meaning | Example |
| --- | --- | --- |
| `*` | Zero or more | `a*` |
| `+` | One or more | `a+` |
| `?` | Zero or one | `a?` |
| `{3}` | Exactly three | `a{3}` |
| `{2,}` | At least two | `a{2,}` |
| `{2,5}` | Between two and five | `a{2,5}` |

# 🗁 Quantifiers with Character Classes

Quantifiers become especially useful when combined with character classes.

**Match one or more digits**

```regex
[0-9]+
```

**Match exactly six digits**

```regex
[0-9]{6}
```

**Match between three and eight letters**

```regex
[a-zA-Z]{3,8}
```

**Match one or more lowercase letters**

```regex
[a-z]+
```

**Match zero or more whitespace characters**

```regex
\s*
```

**Match one or more whitespace characters**

```regex
\s+
```

**Match a sequence of letters followed by digits**

```regex
[a-zA-Z]+\d+
```

Example matches:

```text
user123
admin45
product2026
```

# 🗁 Quantifiers with Groups

Parentheses allow you to apply a quantifier to an entire group.

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

It does not match:

```text
a
abb
```

Explanation:

* `(ab)` defines a group.
* `+` repeats the complete group one or more times.

Another example:

```regex
(ha){2,4}
```

Matches:

```text
haha
hahaha
hahahaha
```

This is different from:

```regex
ha{2,4}
```

The second pattern repeats only `a`, not the complete `ha` sequence.

# 🗁 Greedy Quantifiers

Most common Regex engines use greedy quantifiers by default.

A greedy quantifier tries to match as much text as possible while still allowing the overall pattern to succeed.

Example:

```regex
<.*>
```

Input:

```text
<div>Hello</div>
```

Possible match:

```text
<div>Hello</div>
```

Explanation:

* `<` matches the opening angle bracket.
* `.*` matches as much text as possible.
* `>` matches the final closing angle bracket.

The `.*` portion can consume the text between both tags and the tags themselves.

Another example:

```regex
\d+
```

Input:

```text
123456
```

The greedy quantifier matches all six digits.

Common greedy quantifiers:

| Greedy | Meaning |
| --- | --- |
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `{n,m}` | Between n and m |

# 🗁 Lazy Quantifiers

Lazy quantifiers try to match as little text as possible while still allowing the complete pattern to succeed.

In many common Regex engines, adding `?` after a quantifier makes it lazy.

Example:

```regex
<.*?>
```

Input:

```text
<div>Hello</div>
```

Matches:

```text
<div>
</div>
```

The lazy quantifier stops at the first position where the remaining pattern can succeed.

Another example:

```regex
\d+?
```

Input:

```text
123456
```

When used in a global search, it can match individual digits:

```text
1
2
3
4
5
6
```

Compare:

```regex
\d+
```

With:

```regex
\d+?
```

The first is greedy; the second is lazy.

# 🗁 Greedy vs Lazy Comparison

| Pattern | Behavior |
| --- | --- |
| `.*` | Greedy: matches as much as possible |
| `.*?` | Lazy: matches as little as possible |
| `\d+` | Greedy digit sequence |
| `\d+?` | Lazy digit sequence |
| `a{2,5}` | Greedy repetition within the range |
| `a{2,5}?` | Lazy repetition within the range |

Example:

Input:

```text
<one><two><three>
```

Greedy pattern:

```regex
<.*>
```

Match:

```text
<one><two><three>
```

Lazy pattern:

```regex
<.*?>
```

Matches:

```text
<one>
<two>
<three>
```

# 🗁 Possessive Quantifiers

Possessive quantifiers consume as much text as possible and do not give back matched characters during backtracking.

They are supported by some Regex engines, including PCRE2 and Java, but not by every engine.

Example:

```regex
\d++
```

The `++` form is a possessive version of `+`.

Compare:

```regex
\d+\d
```

With:

```regex
\d++\d
```

Input:

```text
123
```

The first pattern can match the digits by allowing the first quantifier to give back one digit.

The possessive version consumes all digits and refuses to give one back, so the final `\d` cannot match.

Possessive quantifiers can improve performance in some patterns by preventing unnecessary backtracking.

They should be used only when their matching behavior is understood and supported by the target engine.

# 🗁 Quantifier Backtracking

Backtracking occurs when a Regex engine changes an earlier matching decision to try another possible match.

Example:

```regex
\d+\d
```

Input:

```text
123
```

The engine may initially let `\d+` consume all three digits.

However, the final `\d` still needs one digit.

The engine can backtrack, give back the last digit, and allow the final `\d` to match it.

This pattern can therefore match `123`.

Possessive quantifiers and atomic groups can prevent certain backtracking operations.

Poorly designed patterns with nested repetition can also cause excessive backtracking.

For example:

```regex
(a+)+$
```

On certain long inputs that almost match, this pattern can cause significant performance problems in backtracking engines.

Prefer simpler patterns whenever possible.

# 🗁 Optional Characters and Groups

The `?` quantifier is commonly used for optional characters and groups.

**Optional country code**

```regex
\+?880
```

Matches:

```text
880
+880
```

**Optional file extension separator**

```regex
report\.?txt
```

Matches:

```text
report.txt
reporttxt
```

**Optional group**

```regex
https?://
```

Matches:

```text
http://
https://
```

Here, only the `s` is optional.

# 🗁 Practical Examples

**Match a four-digit PIN**

```regex
^\d{4}$
```

**Match a six-digit verification code**

```regex
^\d{6}$
```

**Match a username between three and sixteen characters**

```regex
^[a-zA-Z0-9_]{3,16}$
```

**Match a sequence of one or more letters**

```regex
^[a-zA-Z]+$
```

**Match one or more words separated by spaces**

```regex
^[a-zA-Z]+(?:\s+[a-zA-Z]+)*$
```

**Match an optional plus sign followed by digits**

```regex
^\+?\d+$
```

**Match a simple version number**

```regex
^\d+\.\d+\.\d+$
```

Examples:

```text
1.0.0
2.5.1
10.20.300
```

**Match a number with an optional decimal part**

```regex
^\d+(\.\d+)?$
```

Examples:

```text
10
10.5
250.75
```

# 🗁 Quantifiers in JavaScript

JavaScript supports greedy and lazy quantifiers.

Example:

```javascript
const text = "Order 123, Product 456, Quantity 10";

const pattern = /\d+/g;

console.log(text.match(pattern));
```

Output:

```text
["123", "456", "10"]
```

Example using an exact repetition count:

```javascript
const pin = "123456";

const pattern = /^\d{6}$/;

console.log(pattern.test(pin)); // true
```

Example using a lazy quantifier:

```javascript
const html = "<div>Hello</div><p>World</p>";

const pattern = /<.*?>/g;

console.log(html.match(pattern));
```

Output:

```text
["<div>", "</div>", "<p>", "</p>"]
```

# 🗁 Quantifiers in Python

Python supports greedy and lazy quantifiers through the built-in `re` module.

Example:

```python
import re

text = "Order 123, Product 456, Quantity 10"

pattern = r"\d+"

print(re.findall(pattern, text))
```

Output:

```text
['123', '456', '10']
```

Example using exact repetition:

```python
import re

pin = "123456"

pattern = r"^\d{6}$"

print(bool(re.fullmatch(pattern, pin)))
```

Output:

```text
True
```

Example using a lazy quantifier:

```python
import re

html = "<div>Hello</div><p>World</p>"

pattern = r"<.*?>"

print(re.findall(pattern, html))
```

Output:

```text
['<div>', '</div>', '<p>', '</p>']
```

# 🗁 Common Mistakes

**Confusing `*` and `+`**

```regex
a*
```

Allows zero occurrences.

```regex
a+
```

Requires at least one occurrence.

**Applying a quantifier to the wrong element**

```regex
ab+
```

Repeats only `b`.

```regex
(ab)+
```

Repeats the complete `ab` group.

**Assuming `{n,m}` validates an entire string**

```regex
\d{2,4}
```

Can match a substring inside a longer sequence.

Use:

```regex
^\d{2,4}$
```

when the entire input must contain between two and four digits.

**Using greedy matching when lazy matching is needed**

```regex
<.*>
```

Can consume multiple tag-like sections.

```regex
<.*?>
```

Stops at the first position where the complete pattern can succeed.

**Using nested repetition without considering performance**

Patterns with overlapping alternatives and nested quantifiers can cause excessive backtracking.

Prefer patterns that clearly define what each repetition should consume.

# 🗁 Practice Exercises

**Exercise: Match One or More Digits**

Write a pattern that matches:

```text
123
4567
89
```

But does not match an empty string.

**Exercise: Match an Exact Length**

Write a pattern that validates exactly eight digits.

**Exercise: Match an Optional Character**

Write a pattern that matches both:

```text
color
colour
```

**Exercise: Match a Repetition Range**

Write a pattern that matches between five and ten lowercase letters.

**Exercise: Match Repeated Groups**

Write a pattern that matches:

```text
ha
haha
hahaha
```

**Exercise: Greedy vs Lazy**

Given:

```text
<first><second><third>
```

Write two patterns:

* One that matches the complete sequence.
* Another that extracts each individual tag-like section.

**Exercise: Optional Decimal Part**

Write a pattern that matches:

```text
100
100.50
25.75
```

But does not match:

```text
100.
.50
```

# 🗁 Quick Reference

| Pattern | Meaning |
| --- | --- |
| `a*` | Zero or more `a` characters |
| `a+` | One or more `a` characters |
| `a?` | Zero or one `a` character |
| `a{3}` | Exactly three `a` characters |
| `a{2,}` | At least two `a` characters |
| `a{2,5}` | Between two and five `a` characters |
| `(ab)+` | One or more repetitions of `ab` |
| `a+?` | Lazy one-or-more repetition |
| `.*?` | Lazy matching of any number of characters |
| `a++` | Possessive one-or-more repetition in supported engines |

# 🗁 Final Notes

Quantifiers control repetition and make Regex patterns flexible.

Understand these concepts:

* `*` means zero or more.
* `+` means one or more.
* `?` means zero or one.
* `{n}` means an exact number.
* `{n,}` means at least a specified number.
* `{n,m}` means a repetition range.
* Greedy quantifiers prefer longer matches.
* Lazy quantifiers prefer shorter matches.
* Possessive quantifiers prevent giving back matched characters in supported engines.

The next important concept is anchors, which allow you to control where a pattern must match.
