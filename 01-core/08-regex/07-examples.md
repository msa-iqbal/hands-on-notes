# Regular Expressions: Practical Examples

Regular expressions become especially useful when applied to real-world development tasks such as validating input, extracting information, cleaning text, and processing logs.

This document provides practical regex patterns with JavaScript and Python examples.

# 🗁 Email Validation

A regular expression can perform basic email format validation.

Pragmatic pattern:

```regex
^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$
```

Explanation:

| Pattern | Meaning |
| --- | --- |
| `^` | Start of input |
| `[A-Za-z0-9._%+-]+` | One or more valid-looking local-part characters |
| `@` | Literal at sign |
| `[A-Za-z0-9.-]+` | Domain characters |
| `\.` | Literal dot |
| `[A-Za-z]{2,}` | Top-level domain with at least two letters |
| `$` | End of input |

Examples:

| Email | Result |
| --- | --- |
| `alice@example.com` | Match |
| `john.doe@gmail.com` | Match |
| `user+dev@example.org` | Match |
| `admin@example.co.uk` | Match |
| `user@example` | No match |
| `@example.com` | No match |
| `user@.com` | Match is possible with this simple pattern? No: the domain portion requires at least one character before the dot, but it does not fully validate domain labels |
| `user@example.c` | No match |

This is a pragmatic format check, not complete email-address validation. The email address standard permits forms that a simple regex may reject, and a regex cannot establish that an address exists or can receive mail.

For application registration, combine basic validation with a confirmation email.

# 🗁 IPv4 Address Validation

An IPv4 address contains four decimal octets separated by dots.

Each octet must be between `0` and `255`.

A simple pattern:

```regex
^\d{1,3}(\.\d{1,3}){3}$
```

This checks the general shape but accepts invalid values such as:

```text
999.999.999.999
256.10.20.30
```

A stricter pattern:

```regex
^(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}$
```

Explanation of the octet alternatives:

| Pattern | Range |
| --- | --- |
| `25[0-5]` | 250–255 |
| `2[0-4]\d` | 200–249 |
| `1\d\d` | 100–199 |
| `[1-9]?\d` | 0–99 |

Examples:

| Address | Result |
| --- | --- |
| `192.168.1.1` | Match |
| `10.0.0.1` | Match |
| `172.16.254.10` | Match |
| `255.255.255.255` | Match |
| `0.0.0.0` | Match |
| `256.1.1.1` | No match |
| `192.168.1.999` | No match |
| `192.168.1` | No match |

This pattern does not determine whether an address is assigned, reachable, or appropriate for a particular network. It validates the decimal IPv4 format.

For production code, use a tested IP-address parsing library when available.

# 🗁 URL Validation

A basic regex can recognize common HTTP and HTTPS URLs.

Pragmatic pattern:

```regex
^https?:\/\/[^\s/$.?#].[^\s]*$
```

Explanation:

| Pattern | Meaning |
| --- | --- |
| `^` | Start of input |
| `https?` | `http` or `https` |
| `:\/\/` | Literal `://` |
| `[^\s/$.?#]` | A character other than whitespace or selected delimiters |
| `.` | At least one additional character |
| `[^\s]*` | Remaining non-whitespace characters |
| `$` | End of input |

Examples:

| URL | Result |
| --- | --- |
| `https://example.com` | Match |
| `http://localhost:3000` | Match |
| `https://example.com/products?id=10` | Match |
| `ftp://example.com` | No match |
| `example.com` | No match |
| `https://` | No match |

This pattern is intentionally simple. It is not a complete URL validator and does not fully validate hosts, ports, paths, or percent-encoding.

In JavaScript, prefer the built-in `URL` class:

```javascript
function isValidHttpUrl(value) {
  try {
    const url = new URL(value);

    return (
      url.protocol === "http:" ||
      url.protocol === "https:"
    );
  } catch {
    return false;
  }
}

console.log(isValidHttpUrl("https://example.com"));
console.log(isValidHttpUrl("not-a-url"));
```

Output:

```text
true
false
```

Use a platform URL parser for actual URL processing rather than relying on a complex regex.

# 🗁 Date Format Validation

Suppose an application expects dates in this format:

```text
YYYY-MM-DD
```

Regex:

```regex
^\d{4}-\d{2}-\d{2}$
```

Examples:

| Date | Result |
| --- | --- |
| `2026-11-12` | Match |
| `2025-01-01` | Match |
| `2026-1-12` | No match |
| `12-11-2026` | No match |
| `2026/11/12` | No match |

This checks only the format. It does not verify whether the date exists.

For example:

```text
2026-02-31
```

matches the regex but is not a valid calendar date.

JavaScript example:

```javascript
function isValidDate(value) {
  if (!/^\d{4}-\d{2}-\d{2}$/.test(value)) {
    return false;
  }

  const [year, month, day] = value.split("-").map(Number);

  const date = new Date(Date.UTC(year, month - 1, day));

  return (
    date.getUTCFullYear() === year &&
    date.getUTCMonth() === month - 1 &&
    date.getUTCDate() === day
  );
}

console.log(isValidDate("2026-11-12"));
console.log(isValidDate("2026-02-31"));
```

Output:

```text
true
false
```

# 🗁 Username Validation

Suppose usernames must:

* Contain between 3 and 20 characters.
* Start with a letter.
* Contain only letters, digits, and underscores.

Regex:

```regex
^[A-Za-z][A-Za-z0-9_]{2,19}$
```

Explanation:

| Pattern | Meaning |
| --- | --- |
| `^` | Start of input |
| `[A-Za-z]` | First character must be a letter |
| `[A-Za-z0-9_]{2,19}` | Two to nineteen additional allowed characters |
| `$` | End of input |

Examples:

| Username | Result |
| --- | --- |
| `john` | Match |
| `developer_01` | Match |
| `Admin123` | Match |
| `ab` | No match |
| `123john` | No match |
| `_developer` | No match |
| `john-doe` | No match |

# 🗁 URL Slug Validation

A slug is a URL-friendly text identifier commonly used for articles, products, and categories.

Examples:

```text
getting-started-with-javascript
network-security-basics
regular-expressions-guide
```

Regex:

```regex
^[a-z0-9]+(?:-[a-z0-9]+)*$
```

Explanation:

| Pattern | Meaning |
| --- | --- |
| `^` | Start of input |
| `[a-z0-9]+` | One or more lowercase letters or digits |
| `(?:-[a-z0-9]+)*` | Optional groups of a hyphen followed by letters or digits |
| `$` | End of input |

Examples:

| Slug | Result |
| --- | --- |
| `hello-world` | Match |
| `javascript-guide` | Match |
| `version2-update` | Match |
| `Hello-World` | No match |
| `hello--world` | No match |
| `-hello` | No match |
| `hello-` | No match |

JavaScript example:

```javascript
function isValidSlug(value) {
  return /^[a-z0-9]+(?:-[a-z0-9]+)*$/.test(value);
}

console.log(isValidSlug("hello-world"));
console.log(isValidSlug("Hello-World"));
```

Output:

```text
true
false
```

# 🗁 Whitespace Cleanup

Regex is useful for finding and replacing unnecessary whitespace.

**Remove leading and trailing whitespace:**

```regex
^\s+|\s+$
```

JavaScript:

```javascript
const text = "   Hello Developer   ";

console.log(text.replace(/^\s+|\s+$/g, ""));
```

Output:

```text
Hello Developer
```

A simpler JavaScript solution is:

```javascript
console.log(text.trim());
```

**Replace repeated whitespace with a single space:**

```regex
\s+
```

JavaScript:

```javascript
const text = "Hello     World\n\nDeveloper";

console.log(text.replace(/\s+/g, " "));
```

Output:

```text
Hello World Developer
```

**Remove all whitespace:**

```regex
\s+
```

JavaScript:

```javascript
const text = "Hello World Developer";

console.log(text.replace(/\s+/g, ""));
```

Output:

```text
HelloWorldDeveloper
```

Python example:

```python
import re

text = "Hello     World\n\nDeveloper"

cleaned = re.sub(r"\s+", " ", text).strip()

print(cleaned)
```

Output:

```text
Hello World Developer
```

# 🗁 Log File Extraction

Logs often contain timestamps, log levels, IP addresses, and messages.

Example log:

```text
2026-11-12 08:30:15 INFO Server started
2026-11-12 08:31:20 ERROR Database connection failed
2026-11-12 08:32:10 WARNING High memory usage
```

Extract log levels:

```regex
\b(INFO|ERROR|WARNING|DEBUG)\b
```

Extract timestamps:

```regex
^\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}
```

Extract messages after the log level:

```regex
\b(?:INFO|ERROR|WARNING|DEBUG)\s+(.*)$
```

Python example:

```python
import re

logs = """
2026-11-12 08:30:15 INFO Server started
2026-11-12 08:31:20 ERROR Database connection failed
2026-11-12 08:32:10 WARNING High memory usage
"""

pattern = (
    r"^(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) "
    r"(INFO|ERROR|WARNING|DEBUG) (.*)$"
)

for line in logs.strip().splitlines():
    match = re.match(pattern, line)

    if match:
        timestamp, level, message = match.groups()

        print({
            "timestamp": timestamp,
            "level": level,
            "message": message
        })
```

Output:

```text
{'timestamp': '2026-11-12 08:30:15', 'level': 'INFO', 'message': 'Server started'}
{'timestamp': '2026-11-12 08:31:20', 'level': 'ERROR', 'message': 'Database connection failed'}
{'timestamp': '2026-11-12 08:32:10', 'level': 'WARNING', 'message': 'High memory usage'}
```

# 🗁 Password Policy Validation

A common password policy requires:

* At least eight characters.
* At least one lowercase letter.
* At least one uppercase letter.
* At least one digit.
* At least one special character.

Regex:

```regex
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]).{8,}$
```

JavaScript:

```javascript
const passwordRegex =
  /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]).{8,}$/;

const passwords = [
  "Hello123!",
  "developer123!",
  "HELLO123!",
  "HelloWorld!",
  "Ab1!",
];

for (const password of passwords) {
  console.log(password, passwordRegex.test(password));
}
```

Output:

```text
Hello123! true
developer123! false
HELLO123! false
HelloWorld! false
Ab1! false
```

This pattern treats any non-ASCII letter or digit as a special character. If the application needs a specific allowed-character policy, define that character set explicitly.

# 🗁 Extracting HTML Attributes

Regex can extract simple text fragments or predictable attributes from controlled HTML snippets.

Example:

```html
<a href="https://example.com">Example</a>
```

A simple pattern for this exact attribute style:

```regex
href="([^"]+)"
```

The captured group contains:

```text
https://example.com
```

However, regex is not suitable for parsing arbitrary HTML.

HTML supports nested elements, quoted and unquoted attributes, comments, entities, and many other syntax features. Use an HTML parser such as `DOMParser` in browsers or an appropriate HTML parsing library in other environments.

# 🗁 JSON and Structured Data

Regex can identify simple text patterns inside a JSON string, but it should not be used to parse arbitrary JSON.

JSON supports nested objects and arrays, escaped strings, and different value types.

For example:

```json
{
  "user": {
    "name": "John",
    "roles": ["admin", "developer"]
  }
}
```

Use a JSON parser:

JavaScript:

```javascript
const json = `{
  "user": {
    "name": "John",
    "roles": ["admin", "developer"]
  }
}`;

const data = JSON.parse(json);

console.log(data.user.name);
console.log(data.user.roles);
```

Python:

```python
import json

json_text = """
{
    "user": {
        "name": "John",
        "roles": ["admin", "developer"]
    }
}
"""

data = json.loads(json_text)

print(data["user"]["name"])
print(data["user"]["roles"])
```

# 🗁 Using Regex with grep

The `grep` command searches text files using regular expressions.

Example file:

```text
server.log
```

Search for errors:

```bash
grep -E "ERROR|FATAL" server.log
```

Search for IPv4-shaped strings:

```bash
grep -Eo '[0-9]{1,3}(\.[0-9]{1,3}){3}' server.log
```

This finds strings with the general IPv4 shape, but does not enforce the `0–255` range.

Search for lines beginning with a date:

```bash
grep -E '^[0-9]{4}-[0-9]{2}-[0-9]{2}' server.log
```

Search case-insensitively:

```bash
grep -i "error" server.log
```

Show line numbers:

```bash
grep -n "ERROR" server.log
```

Search recursively:

```bash
grep -r -n "TODO" ./src
```

# 🗁 Testing Regular Expressions

Testing is an essential part of regex development.

A reliable workflow:

* Define exactly what should match.
* Define what must not match.
* Write a small pattern.
* Test valid and invalid examples.
* Check boundary conditions.
* Test the target regex engine.
* Review performance on realistic input.
* Add regression tests when the pattern is used in application code.

Useful online regex testing tools include:

* [regex101](https://regex101.com/) — interactive regex testing with explanations and multiple regex engines.
* [RegExr](https://regexr.com/) — visual regex editor and reference.
* [MDN Regular Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions) — JavaScript regex documentation.

# 🗁 Common Regex Mistakes in Real Projects

**Using `.` without considering newlines**

In many regex engines, `.` does not match newline characters unless dot-all mode is enabled.

**Forgetting anchors**

Without `^` and `$`, a validation pattern may match only part of an input.

**Using `*` when `+` is required**

* `*` allows zero or more repetitions.
* `+` requires at least one repetition.

**Assuming all regex engines behave identically**

Features, flags, Unicode handling, and lookbehind support differ between engines.

**Using overly complicated patterns**

A large regex may become difficult to maintain. Consider a parser or dedicated validation library when the input has a structured format.

**Confusing format validation with semantic validation**

A regex can verify that a string looks like a date or IP address. It does not necessarily establish that the value is meaningful or valid in its application context.

# 🗁 Practice Exercises

Write regex patterns for the following requirements:

* Match a six-digit numeric postal code.
* Match a username containing 5–15 letters, digits, or underscores.
* Match filenames ending in `.jpg`, `.jpeg`, or `.png`.
* Extract HTTP status codes from log lines.
* Match words beginning with an uppercase letter.
* Replace multiple spaces with one space.
* Match hexadecimal color codes such as `#FFFFFF` and `#abc`.
* Match a phone number in the format `+8801XXXXXXXXX`.
* Extract text inside parentheses.
* Match a string that contains only lowercase letters and digits.

Suggested answers:

**Six-digit numeric postal code:**

```regex
^\d{6}$
```

**Username with 5–15 allowed characters:**

```regex
^[A-Za-z0-9_]{5,15}$
```

**Image filenames:**

```regex
^.+\.(?:jpg|jpeg|png)$
```

Use case-insensitive mode if uppercase extensions should also be accepted.

**HTTP status code in a log line:**

```regex
\b[1-5]\d{2}\b
```

**Words beginning with an uppercase letter:**

```regex
\b[A-Z][a-zA-Z]*\b
```

**Repeated spaces:**

```regex
 {2,}
```

**Three- or six-digit hexadecimal color codes:**

```regex
^#(?:[0-9A-Fa-f]{3}|[0-9A-Fa-f]{6})$
```

**Bangladeshi mobile number in the specified format:**

```regex
^\+8801\d{9}$
```

**Text inside parentheses:**

```regex
\(([^()]*)\)
```

This handles simple non-nested parentheses, not arbitrary nested structures.

**Lowercase letters and digits only:**

```regex
^[a-z0-9]+$
```

# 🗁 Key Takeaways

* Regex is useful for validation, extraction, searching, and text transformation.
* A practical regex should match the application's actual input requirements.
* Format validation is not always equivalent to semantic validation.
* Use URL, date, IP, HTML, and JSON parsers when they provide more reliable structured-data handling.
* Always test both matching and non-matching cases.
* Regex syntax and behavior can vary between programming languages and engines.
* Prefer readable, maintainable patterns over unnecessarily complicated expressions.
