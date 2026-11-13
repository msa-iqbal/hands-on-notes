# JSON Common Errors

JSON is a lightweight data-interchange format with strict syntax rules. Even a small formatting mistake can make a JSON document invalid and cause parsing errors.

Understanding common JSON errors helps developers debug API responses, configuration files, application data, and communication between systems.

# 🗁 Trailing Commas

JSON does not allow trailing commas after the last element of an object or array.

**Invalid JSON**

```json
{
  "name": "John",
  "age": 25,
}
```

**Correct JSON**

```json
{
  "name": "John",
  "age": 25
}
```

The same rule applies to arrays.

**Invalid JSON**

```json
{
  "languages": [
    "JavaScript",
    "Python",
  ]
}
```

**Correct JSON**

```json
{
  "languages": [
    "JavaScript",
    "Python"
  ]
}
```

# 🗁 Using Single Quotes

JSON requires double quotes (`"`) around strings and object property names.

**Invalid JSON**

```json
{
  'name': 'John',
  'city': 'Dhaka'
}
```

**Correct JSON**

```json
{
  "name": "John",
  "city": "Dhaka"
}
```

JavaScript and Python allow single-quoted strings in their programming syntax, but JSON does not.

# 🗁 Unquoted Object Keys

Every JSON object property name must be a string enclosed in double quotes.

**Invalid JSON**

```json
{
  name: "John",
  age: 25
}
```

**Correct JSON**

```json
{
  "name": "John",
  "age": 25
}
```

# 🗁 Adding Comments

Standard JSON does not support comments.

**Invalid JSON**

```json
{
  // User information
  "name": "John",
  "age": 25
}
```

Block comments are also invalid.

```json
{
  /*
    User information
  */
  "name": "John"
}
```

**Correct JSON**

```json
{
  "name": "John",
  "age": 25
}
```

If you need comments in a configuration file, consider YAML or a format designed to support comments.

# 🗁 Invalid Escape Sequences

JSON strings support specific escape sequences.

Valid escape sequences include:

| Escape sequence | Meaning |
| --- | --- |
| `\"` | Double quotation mark |
| `\\` | Backslash |
| `\/` | Forward slash |
| `\b` | Backspace |
| `\f` | Form feed |
| `\n` | New line |
| `\r` | Carriage return |
| `\t` | Horizontal tab |
| `\uXXXX` | Unicode escape using four hexadecimal digits |

**Invalid JSON**

```json
{
  "message": "Hello \q World"
}
```

`\q` is not a valid JSON escape sequence.

**Correct JSON**

```json
{
  "message": "Hello World"
}
```

To represent a literal Windows path, escape each backslash.

**Invalid JSON**

```json
{
  "path": "C:\Users\John\Documents"
}
```

**Correct JSON**

```json
{
  "path": "C:\\Users\\John\\Documents"
}
```

Alternatively, use forward slashes:

```json
{
  "path": "C:/Users/John/Documents"
}
```

# 🗁 Invalid Number Formats

JSON has strict number syntax.

**Leading zeros**

Invalid:

```json
{
  "age": 025
}
```

Correct:

```json
{
  "age": 25
}
```

**Missing integer part**

Invalid:

```json
{
  "price": .99
}
```

Correct:

```json
{
  "price": 0.99
}
```

**Missing fractional digits**

Invalid:

```json
{
  "price": 10.
}
```

Correct:

```json
{
  "price": 10.0
}
```

**Invalid positive or negative notation**

Invalid:

```json
{
  "value": +10
}
```

Correct:

```json
{
  "value": 10
}
```

JSON also supports exponent notation:

```json
{
  "small": 1e-5,
  "large": 2.5e6
}
```

# 🗁 Invalid Boolean and Null Values

JSON uses lowercase literals for booleans and null.

Valid values:

```json
{
  "active": true,
  "verified": false,
  "address": null
}
```

Invalid values:

```json
{
  "active": True,
  "verified": FALSE,
  "address": Null
}
```

Remember:

* `true` is valid.
* `false` is valid.
* `null` is valid.
* `True`, `False`, and `Null` are invalid.

# 🗁 Missing Commas and Incorrect Separators

JSON object properties and array elements must be separated by commas.

**Invalid JSON**

```json
{
  "name": "John"
  "age": 25
}
```

**Correct JSON**

```json
{
  "name": "John",
  "age": 25
}
```

Another example:

**Invalid JSON**

```json
[
  "JavaScript"
  "Python"
]
```

**Correct JSON**

```json
[
  "JavaScript",
  "Python"
]
```

# 🗁 Mismatched Brackets and Braces

JSON uses curly braces for objects and square brackets for arrays.

| Symbol | Purpose |
| --- | --- |
| `{ }` | Object |
| `[ ]` | Array |

Every opening delimiter must have a matching closing delimiter.

**Invalid JSON**

```json
{
  "users": [
    {
      "name": "John"
    }
}
```

**Correct JSON**

```json
{
  "users": [
    {
      "name": "John"
    }
  ]
}
```

# 🗁 Duplicate Object Keys

JSON object names should be unique.

Consider:

```json
{
  "name": "John",
  "age": 25,
  "name": "David"
}
```

The `name` property appears twice.

The JSON grammar permits duplicate names, but their handling is unpredictable across implementations. Many parsers retain only the last value, while others may behave differently.

**Recommended approach:**

```json
{
  "name": "John",
  "age": 25
}
```

Always use unique property names within an object.

# 🗁 Top-Level JSON Values

A JSON document does not have to start with an object.

A valid JSON text can contain any JSON value.

**Object**

```json
{
  "name": "John"
}
```

**Array**

```json
[
  "JavaScript",
  "Python"
]
```

**String**

```json
"Hello World"
```

**Number**

```json
100
```

**Boolean**

```json
true
```

**Null**

```json
null
```

All these are valid JSON texts.

However, an application or API may impose additional requirements, such as expecting an object or array at the top level.

# 🗁 Encoding and UTF-8 Issues

JSON exchanged between systems should use UTF-8 encoding.

Incorrect character encoding can cause text corruption, commonly called *mojibake*.

For example, a character intended to display as:

```text
বাংলাদেশ
```

may appear incorrectly if the bytes are decoded using the wrong character encoding.

When working with JSON files:

* Prefer UTF-8 encoding.
* Ensure the producer and consumer agree on the encoding.
* Avoid converting text between incompatible encodings.
* Check the actual file encoding when unexpected characters appear.

A UTF-8 byte order mark (BOM) is not part of the JSON data itself. Some parsers tolerate it, while others reject it.

# 🗁 Parsing Errors in JavaScript

JavaScript provides `JSON.parse()` to convert JSON text into native JavaScript values.

Invalid JSON causes a `SyntaxError`.

**Example**

```javascript
const jsonString = `{
  "name": "John",
  "age": 25,
}`;

try {
  const data = JSON.parse(jsonString);

  console.log(data);
} catch (error) {
  console.error("Invalid JSON:", error.message);
}
```

The trailing comma makes the JSON invalid.

**Corrected example**

```javascript
const jsonString = `{
  "name": "John",
  "age": 25
}`;

try {
  const data = JSON.parse(jsonString);

  console.log(data);
} catch (error) {
  console.error("Invalid JSON:", error.message);
}
```

# 🗁 Parsing Errors in Python

Python provides `json.loads()` for parsing JSON strings and `json.load()` for reading JSON files.

Invalid JSON raises `json.JSONDecodeError`.

**Example**

```python
import json

json_string = """
{
    "name": "John",
    "age": 25,
}
"""

try:
    data = json.loads(json_string)

    print(data)

except json.JSONDecodeError as error:
    print("Invalid JSON:", error)
```

**Corrected example**

```python
import json

json_string = """
{
    "name": "John",
    "age": 25
}
"""

try:
    data = json.loads(json_string)

    print(data)

except json.JSONDecodeError as error:
    print("Invalid JSON:", error)
```

Python error information can include:

* `msg`: Description of the parsing problem.
* `lineno`: Line number where the error occurred.
* `colno`: Column number where the error occurred.
* `pos`: Character position where the error was detected.

You can access these properties directly:

```python
import json

try:
    json.loads('{"name": "John",}')

except json.JSONDecodeError as error:
    print("Message:", error.msg)
    print("Line:", error.lineno)
    print("Column:", error.colno)
    print("Position:", error.pos)
```

# 🗁 Syntax Validation vs Schema Validation

Syntax validation and schema validation are different processes.

**JSON syntax validation** checks whether a document follows JSON grammar.

For example:

```json
{
  "name": "John",
  "age": 25
}
```

This is valid JSON syntax.

However, syntax validation does not determine whether the data is appropriate for a particular application.

**Schema validation** checks whether the data follows a defined structure and set of rules.

For example, an application may require:

* `name` to be a string.
* `age` to be a number.
* `email` to be a valid email address.
* `active` to be a boolean.

A JSON document can be syntactically valid but fail schema validation.

**Example: Valid JSON, invalid application data**

```json
{
  "name": 123,
  "age": "twenty-five"
}
```

The JSON syntax is valid, but the values do not match the expected application types.

# 🗁 JSON Schema Example

JSON Schema is a standard for describing and validating JSON data structures.

Example schema:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "age": {
      "type": "integer",
      "minimum": 0
    },
    "active": {
      "type": "boolean"
    }
  },
  "required": ["name", "age", "active"],
  "additionalProperties": false
}
```

This schema describes an object that must contain:

* `name`: String.
* `age`: Non-negative integer.
* `active`: Boolean.

It also prevents additional properties.

A document can pass JSON syntax validation but fail this schema.

# 🗁 Common JSON Validation Tools

Several tools can help identify JSON syntax errors.

| Tool | Purpose |
| --- | --- |
| `JSON.parse()` | Parse JSON in JavaScript |
| `json.loads()` | Parse JSON in Python |
| `python -m json.tool` | Validate and format JSON from the command line |
| `jq` | Process and inspect JSON |
| JSONLint | Validate JSON through a web interface |
| JSON Schema validators | Validate JSON structure and constraints |

**Validate JSON using Python CLI**

```bash
python -m json.tool data.json
```

If the file is valid, Python prints its formatted JSON representation.

If invalid, it reports a parsing error.

**Inspect JSON using jq**

```bash
jq . data.json
```

This parses and pretty-prints the JSON document.

# 🗁 Debugging JSON Errors

Follow a systematic debugging process when JSON parsing fails.

**Check the parser error**

Read the error message and identify the reported line and column.

**Inspect nearby syntax**

Look for:

* Missing commas.
* Extra commas.
* Missing quotation marks.
* Incorrect brackets or braces.
* Invalid escape sequences.

**Validate the document**

Use a JSON parser or validator to identify syntax errors.

**Check the data source**

If JSON comes from an API, inspect the actual response body rather than assuming the server returned valid JSON.

**Check the encoding**

Confirm that the document is encoded correctly, preferably using UTF-8.

**Validate the structure**

After successful parsing, verify that required properties and expected data types are present.

**Handle errors properly**

Use exception handling so malformed JSON does not unexpectedly terminate application execution.

# 🗁 Best Practices

* Always use double quotes for JSON strings and property names.
* Never include comments in standard JSON.
* Avoid trailing commas.
* Use valid JSON escape sequences.
* Use lowercase `true`, `false`, and `null`.
* Use unique object property names.
* Prefer UTF-8 encoding.
* Validate JSON before sending it to another system.
* Separate syntax validation from schema validation.
* Handle parsing exceptions in application code.
* Inspect raw API responses when debugging parsing problems.
* Use consistent formatting to make JSON easier to read and maintain.

# 🗁 Summary

* JSON follows strict syntax rules.
* Trailing commas, single quotes, comments, and unquoted keys are invalid.
* Strings require valid escape sequences.
* Numbers and literals must follow JSON syntax.
* JSON text can contain any valid JSON value at the top level.
* Duplicate object keys should be avoided.
* UTF-8 is the recommended encoding for JSON exchange.
* JavaScript uses `JSON.parse()` to parse JSON.
* Python uses `json.loads()` and `json.load()`.
* Syntax validation checks JSON grammar.
* Schema validation checks data structure and application-specific constraints.
* Systematic debugging makes JSON errors easier to identify and resolve.
