# YAML Basics

YAML (YAML Ain't Markup Language) is a human-readable data serialization language designed to represent structured data.

It is commonly used for configuration files, application settings, infrastructure automation, and DevOps tools.

Unlike JSON, YAML uses indentation rather than braces and brackets to represent relationships between data elements.

# 🗁 What Is YAML?

YAML stores data using a simple, readable syntax.

Example:

```yaml
name: John
age: 25
city: Dhaka
```

This document contains three key-value pairs:

* `name` has the value `John`.
* `age` has the value `25`.
* `city` has the value `Dhaka`.

# 🗁 YAML File Extension

YAML files commonly use these extensions:

| Extension | Description |
| --- | --- |
| `.yaml` | Standard YAML file extension |
| `.yml` | Alternative YAML file extension |

Both extensions represent the same YAML format.

Examples:

```text
config.yaml
docker-compose.yml
application.yaml
settings.yml
```

# 🗁 Basic YAML Syntax

YAML uses key-value pairs to represent structured information.

The basic syntax is:

```yaml
key: value
```

Example:

```yaml
username: john
email: john@example.com
role: developer
```

A colon separates the key from its value.

A space after the colon is required when writing a block-style mapping.

# 🗁 YAML Keys and Values

A YAML mapping associates keys with values.

```yaml
name: John
age: 25
active: true
```

In this example:

| Key | Value |
| --- | --- |
| `name` | `John` |
| `age` | `25` |
| `active` | `true` |

Keys are commonly strings, while values can represent different YAML data types.

# 🗁 YAML Indentation

Indentation is one of the most important YAML syntax rules.

YAML uses indentation to represent hierarchy and nesting.

Example:

```yaml
user:
  name: John
  age: 25
  address:
    city: Dhaka
    country: Bangladesh
```

The structure contains:

* `user` as the root-level key.
* `name`, `age`, and `address` inside `user`.
* `city` and `country` inside `address`.

**Important indentation rules:**

* Use spaces for indentation.
* Do not use tabs for indentation.
* Keep indentation consistent.
* Child elements must be indented relative to their parent.
* Elements at the same hierarchy level should have the same indentation.

Two spaces per indentation level are a common convention.

# 🗁 YAML Comments

YAML supports comments using the hash symbol (`#`).

Comments are ignored by YAML parsers.

**Single-line comment**

```yaml
# Application configuration
name: my-application
version: 1.0
```

**Inline comment**

```yaml
name: John  # User's name
age: 25     # User's age
```

**Multiple comments**

```yaml
# Database configuration
database:
  # Database hostname
  host: localhost

  # Database connection port
  port: 5432
```

YAML does not have a dedicated multiline comment syntax. Use `#` at the beginning of each comment line.

# 🗁 YAML Strings

Strings represent textual data.

```yaml
name: John
city: Dhaka
message: Welcome to YAML
```

Strings can also be written using quotation marks.

**Single-quoted string**

```yaml
name: 'John'
message: 'Welcome to YAML'
```

**Double-quoted string**

```yaml
name: "John"
message: "Welcome to YAML"
```

Both quoted styles are valid.

Double-quoted strings support escape sequences, while single-quoted strings generally treat backslashes literally.

# 🗁 YAML Numbers

YAML supports integer and floating-point numbers.

**Integer**

```yaml
age: 25
quantity: 100
port: 8080
```

**Floating-point**

```yaml
price: 99.99
temperature: 36.5
percentage: 85.75
```

Numbers do not normally require quotation marks.

# 🗁 YAML Booleans

Boolean values represent logical states.

```yaml
active: true
verified: false
enabled: true
```

For YAML 1.2, the standard boolean values are:

* `true`
* `false`

Using lowercase `true` and `false` is a good convention for predictable parsing.

# 🗁 YAML Null Values

Null represents the absence of a value.

YAML supports several representations:

```yaml
username: null
email: ~
address:
```

All three represent null values under YAML 1.2 core schema rules.

# 🗁 YAML Arrays

YAML uses sequences to represent collections of values.

Each element begins with a hyphen followed by a space.

```yaml
languages:
  - JavaScript
  - Python
  - PHP
```

The `languages` key contains three elements.

A sequence can also contain numbers:

```yaml
ports:
  - 3000
  - 5000
  - 8080
```

Arrays and sequences are covered in detail in `04-arrays.md`.

# 🗁 YAML Objects

YAML uses mappings to represent objects and key-value relationships.

```yaml
user:
  name: John
  age: 25
  email: john@example.com
```

The `user` key contains a nested mapping.

Mappings and their structures are covered in detail in `03-objects.md`.

# 🗁 YAML Document Structure

A YAML file can contain one or more documents.

The document start marker is:

```yaml
---
```

Example:

```yaml
---
name: John
age: 25
```

The document end marker is:

```yaml
...
```

Example:

```yaml
---
name: John
age: 25
...
```

The markers are optional in many ordinary YAML files.

Multiple documents can be separated using `---`:

```yaml
---
name: John
role: developer

---
name: David
role: designer
```

Each document is a separate YAML data structure.

# 🗁 YAML Block Style and Flow Style

YAML supports two general presentation styles.

**Block style**

Block style uses indentation and line breaks.

```yaml
user:
  name: John
  age: 25
  skills:
    - JavaScript
    - Python
```

**Flow style**

Flow style uses braces and brackets, similar to JSON.

```yaml
user: {name: John, age: 25, skills: [JavaScript, Python]}
```

Both represent structured YAML data.

Block style is generally preferred for readability in configuration files.

# 🗁 YAML vs JSON

YAML and JSON are both data serialization formats.

However, they use different syntax conventions.

| Feature | YAML | JSON |
| --- | --- | --- |
| File extensions | `.yaml`, `.yml` | `.json` |
| Objects | Mappings | Objects |
| Arrays | Sequences | Arrays |
| Indentation | Defines structure | Formatting only |
| Comments | Supported | Not supported |
| Strings | Can be unquoted when valid | Must be quoted |
| Syntax | Less punctuation | Braces, brackets, commas |
| Readability | Human-friendly configuration | Structured data exchange |

**YAML example**

```yaml
user:
  name: John
  age: 25
  skills:
    - JavaScript
    - Python
```

**JSON equivalent**

```json
{
  "user": {
    "name": "John",
    "age": 25,
    "skills": [
      "JavaScript",
      "Python"
    ]
  }
}
```

# 🗁 YAML 1.2

YAML has multiple specification versions.

YAML 1.2 introduced important compatibility improvements, particularly around JSON compatibility and boolean interpretation.

YAML 1.2.2 is the published specification version to use as a modern reference.

Some YAML parsers still implement older YAML 1.1 behavior or provide their own schema rules.

For example:

```yaml
enabled: true
disabled: false
```

These are unambiguous boolean values under YAML 1.2.

Values such as `yes`, `no`, `on`, and `off` are strings under the YAML 1.2 core schema, although some older parsers may interpret them as booleans.

# 🗁 Practical Example: Application Configuration

YAML is frequently used to store application settings.

```yaml
app:
  name: my-application
  version: 1.0
  environment: development
  debug: true

server:
  host: localhost
  port: 3000

database:
  host: localhost
  port: 5432
  name: application_db
```

This example organizes application, server, and database settings into separate sections.

# 🗁 Practical Example: Docker Compose

Docker Compose uses YAML files to define application services.

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"

  database:
    image: postgres:17
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
```

This configuration defines two services:

* `web`: Runs an Nginx container.
* `database`: Runs a PostgreSQL container.

The example demonstrates how YAML represents nested configuration structures.

# 🗁 Practical Example: GitHub Actions

GitHub Actions uses YAML workflow files to define automation tasks.

```yaml
name: Node.js CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

This workflow runs when changes are pushed to the `main` branch.

It demonstrates YAML mappings, sequences, and nested structures.

# 🗁 Common YAML Mistakes

**Using tabs for indentation**

Invalid:

```yaml
user:
 name: John
```

Correct:

```yaml
user:
  name: John
```

**Incorrect indentation**

Invalid:

```yaml
user:
  name: John
    age: 25
```

Correct:

```yaml
user:
  name: John
  age: 25
```

**Missing space after colon**

Potentially problematic:

```yaml
name:John
```

Correct:

```yaml
name: John
```

**Incorrect sequence formatting**

Invalid:

```yaml
languages:
-JavaScript
-Python
```

Correct:

```yaml
languages:
  - JavaScript
  - Python
```

# 🗁 Best Practices

* Use spaces instead of tabs.
* Prefer two spaces per indentation level.
* Keep indentation consistent throughout the file.
* Use meaningful and descriptive keys.
* Add comments where configuration requires explanation.
* Prefer lowercase `true` and `false` for booleans.
* Quote values when ambiguity could affect their interpretation.
* Use block style for complex configuration structures.
* Follow the YAML version and schema expected by your parser.
* Validate YAML before using it in production workflows.

# 🗁 Summary

* YAML is a human-readable data serialization language.
* YAML commonly uses `.yaml` and `.yml` file extensions.
* Mappings represent key-value relationships.
* Sequences represent ordered collections.
* Indentation defines nested structures.
* Comments begin with `#`.
* YAML supports strings, numbers, booleans, and null values.
* `---` separates YAML documents.
* Block and flow styles provide different ways to represent data.
* YAML is widely used in Docker, Kubernetes, GitHub Actions, and DevOps automation.
