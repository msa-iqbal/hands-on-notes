# YAML Data Types

YAML supports several data types for representing structured information. These types determine how YAML parsers interpret values when converting YAML documents into native programming-language data structures.

The main YAML data types include:

* Strings
* Integers
* Floating-point numbers
* Booleans
* Null values
* Timestamps
* Sequences
* Mappings

Understanding YAML data types is important when creating configuration files, API-related data, infrastructure definitions, and automation workflows.

# 🗁 YAML Scalars

A scalar is a single value rather than a collection of values.

YAML scalar types include strings, numbers, booleans, null values, and timestamps.

Example:

```yaml
name: John
age: 25
price: 99.99
active: true
address: null
```

Each property contains an individual scalar value.

# 🗁 String Data Type

A string represents textual information.

YAML supports plain, single-quoted, and double-quoted strings.

**Plain strings**

Strings can often be written without quotation marks.

```yaml
name: John
city: Dhaka
role: Software Engineer
```

**Single-quoted strings**

Single quotes explicitly represent strings.

```yaml
name: 'John'
message: 'Welcome to YAML'
```

To include a literal single quote inside a single-quoted string, use two consecutive single quotes.

```yaml
message: 'John''s application'
```

**Double-quoted strings**

Double quotes support escape sequences.

```yaml
name: "John"
message: "Welcome to YAML"
```

For example:

```yaml
message: "Hello\nWorld"
path: "C:\\Users\\John"
```

Here:

* `\n` represents a newline.
* `\\` represents a literal backslash.

# 🗁 Multiline Strings

YAML supports block scalar styles for representing multiline text.

**Literal block style (`|`)**

The literal style preserves line breaks.

```yaml
description: |
  YAML is a data serialization language.
  It is commonly used for configuration files.
  It is easy for humans to read.
```

The resulting string contains newline characters.

**Folded block style (`>`)**

The folded style generally converts ordinary line breaks into spaces.

```yaml
description: >
  YAML is a data serialization language.
  It is commonly used for configuration files.
  It is easy for humans to read.
```

The resulting text is generally interpreted as one paragraph.

**Chomping indicators**

YAML provides indicators to control trailing newlines.

| Indicator | Behavior |
| --- | --- |
| ` | ` | Keeps one final newline |
| ` | -` | Removes trailing newlines |
| ` | +` | Preserves trailing newlines |
| `>` | Folded style with one final newline |
| `>-` | Folded style without trailing newlines |
| `>+` | Folded style preserving trailing newlines |

Example:

```yaml
normal: |
  Hello
  World

strip: |-
  Hello
  World

keep: |+
  Hello
  World
```

# 🗁 Integer Data Type

Integers represent whole numbers without fractional parts.

```yaml
age: 25
quantity: 100
port: 8080
temperature: -10
```

YAML supports positive and negative integers.

It also supports additional integer representations.

**Decimal notation**

```yaml
value: 100
```

**Hexadecimal notation**

```yaml
value: 0x64
```

**Octal notation**

```yaml
value: 0o144
```

Under the YAML 1.2 core schema, these values represent the integer `100`.

# 🗁 Floating-Point Data Type

Floating-point numbers represent numeric values with fractional parts.

```yaml
price: 99.99
temperature: 36.5
percentage: 85.75
```

YAML also supports scientific notation.

```yaml
small: 1.5e-5
large: 2.5e6
```

Special floating-point values are also available under the YAML 1.2 core schema.

```yaml
positive_infinity: .inf
negative_infinity: -.inf
not_a_number: .nan
```

These values represent positive infinity, negative infinity, and NaN respectively.

# 🗁 Boolean Data Type

Booleans represent logical states.

YAML 1.2 core schema recognizes these lowercase values:

```yaml
active: true
verified: false
enabled: true
```

Boolean values are commonly used in configuration files to enable or disable features.

**Important YAML version difference**

YAML 1.1 parsers may interpret values such as `yes`, `no`, `on`, and `off` as booleans.

Under the YAML 1.2 core schema, these values are strings.

```yaml
enabled: true
legacy_yes: yes
legacy_on: on
```

For predictable behavior, use lowercase `true` and `false`.

# 🗁 Null Data Type

Null represents the absence of a value.

YAML supports several null representations under the YAML 1.2 core schema.

```yaml
username: null
email: ~
address:
```

All three represent null.

Null is different from an empty string.

```yaml
name: ""
email: null
```

Here:

* `name` contains an empty string.
* `email` contains a null value.

# 🗁 Timestamp Data Type

YAML 1.2 core schema supports timestamp values.

Timestamps commonly represent dates and date-time values.

**Date**

```yaml
created_date: 2026-11-12
```

**Date and time**

```yaml
created_at: 2026-11-12T10:30:00Z
```

**Date and time with timezone offset**

```yaml
updated_at: 2026-11-12T16:30:00+06:00
```

The `Z` suffix represents UTC.

**Important:** Timestamp interpretation depends on the YAML schema and parser implementation. Some parsers may convert timestamps into native date objects, while others may retain them as strings.

If a value must always be interpreted as a string, quote it.

```yaml
created_date: "2026-11-12"
```

# 🗁 Explicit Type Tags

YAML allows explicit tags to specify how a value should be interpreted.

The standard tag prefix is:

```yaml
!!
```

Examples:

**Explicit string**

```yaml
age: !!str 25
```

Although `25` looks like a number, the explicit string tag instructs the parser to interpret it as text.

**Explicit integer**

```yaml
age: !!int "25"
```

**Explicit boolean**

```yaml
active: !!bool true
```

**Explicit null**

```yaml
value: !!null null
```

Explicit tags can help when a value might otherwise be interpreted ambiguously.

# 🗁 Quoted vs Unquoted Values

YAML allows many strings to be written without quotation marks.

However, some values can be interpreted as other data types.

Compare:

```yaml
version: 1.0
version_text: "1.0"

enabled: true
enabled_text: "true"

date: 2026-11-12
date_text: "2026-11-12"
```

Depending on the schema and parser:

* `version` may be interpreted as a floating-point number.
* `version_text` is a string.
* `enabled` is a boolean.
* `enabled_text` is a string.
* `date` may be interpreted as a timestamp.
* `date_text` is a string.

Use quotation marks when the value must remain textual.

# 🗁 YAML Collections

YAML also supports collection data types.

The two main collection types are:

* Mappings: Key-value collections.
* Sequences: Ordered collections of values.

**Mapping**

```yaml
user:
  name: John
  age: 25
  active: true
```

**Sequence**

```yaml
languages:
  - JavaScript
  - Python
  - PHP
```

Mappings and sequences are covered in detail in `03-objects.md` and `04-arrays.md`.

# 🗁 YAML Data Types vs JSON Data Types

YAML and JSON share several fundamental data types.

| Data Type | YAML | JSON |
| --- | --- | --- |
| String | Supported | Supported |
| Integer | Supported | Supported as a number |
| Floating-point | Supported | Supported as a number |
| Boolean | Supported | Supported |
| Null | Supported | Supported |
| Array / Sequence | Supported | Supported |
| Object / Mapping | Supported | Supported |
| Timestamp | Schema-dependent | No dedicated type |
| Comments | Supported | Not supported |

JSON has a smaller set of native data types, while YAML provides additional scalar representations and features.

# 🗁 Data Types in JavaScript

When YAML is parsed into JavaScript values, its supported types are commonly represented using JavaScript types.

| YAML Type | JavaScript Type |
| --- | --- |
| String | `string` |
| Integer | `number` |
| Floating-point | `number` |
| Boolean | `boolean` |
| Null | `null` |
| Sequence | `Array` |
| Mapping | `Object` |
| Timestamp | Parser-dependent, often `Date` or `string` |

Example using the `yaml` package:

```javascript
import YAML from "yaml";

const yamlText = `
name: John
age: 25
price: 99.99
active: true
address: null
skills:
  - JavaScript
  - Python
`;

const data = YAML.parse(yamlText);

console.log(data.name);
console.log(typeof data.age);
console.log(typeof data.price);
console.log(typeof data.active);
console.log(data.address);
console.log(data.skills);
```

# 🗁 Data Types in Python

When YAML is parsed into Python values, common mappings include:

| YAML Type | Python Type |
| --- | --- |
| String | `str` |
| Integer | `int` |
| Floating-point | `float` |
| Boolean | `bool` |
| Null | `NoneType` |
| Sequence | `list` |
| Mapping | `dict` |
| Timestamp | Parser-dependent, often `datetime.date` or `datetime.datetime` |

Example using PyYAML:

```python
import yaml

yaml_text = """
name: John
age: 25
price: 99.99
active: true
address: null
skills:
  - JavaScript
  - Python
"""

data = yaml.safe_load(yaml_text)

print(data["name"])
print(type(data["age"]))
print(type(data["price"]))
print(type(data["active"]))
print(data["address"])
print(data["skills"])
```

`safe_load()` is the recommended PyYAML loading method for ordinary YAML data from external sources.

# 🗁 Practical Example: Application Configuration

The following example combines several YAML data types.

```yaml
app:
  name: my-application
  version: "1.0"
  debug: true

server:
  host: localhost
  port: 3000

database:
  host: localhost
  port: 5432
  username: admin
  password: null

features:
  - authentication
  - logging
  - notifications

metadata:
  created_at: "2026-11-12"
  description: |
    Application configuration
    for the development environment.
```

This example contains:

* Strings.
* Integers.
* Booleans.
* Null values.
* Sequences.
* Mappings.
* Multiline strings.

# 🗁 Best Practices

* Use quotation marks for values that must remain strings.
* Prefer lowercase `true` and `false` for boolean values.
* Use `null` when explicitly representing an absent value.
* Distinguish empty strings from null values.
* Use literal block style (`|`) for multiline text where line breaks matter.
* Use folded block style (`>`) for multiline text intended as a paragraph.
* Be consistent with numeric representations.
* Understand the YAML version and schema used by your parser.
* Avoid ambiguous values in configuration files.
* Use explicit tags only when necessary.
* Test how your target YAML parser interprets important values.

# 🗁 Summary

* YAML supports strings, integers, floating-point numbers, booleans, null values, and timestamps.
* Scalars represent individual values.
* Strings can use plain, single-quoted, double-quoted, or block styles.
* Literal block style preserves line breaks.
* Folded block style generally converts line breaks into spaces.
* YAML supports explicit type tags.
* Mappings and sequences represent collections.
* YAML data types are converted into native types when parsed by programming languages.
* Parser behavior can vary depending on YAML version and schema.
