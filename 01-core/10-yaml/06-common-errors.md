# YAML Common Errors

YAML is a human-readable data serialization language that relies heavily on indentation, whitespace, and special characters.

Although YAML syntax is relatively simple, small formatting mistakes can cause parsing errors or unexpected data types.

Understanding common YAML errors helps you write reliable configuration files and troubleshoot problems in tools such as Docker Compose, Kubernetes, GitHub Actions, and application frameworks.

# 🗁 Indentation Errors

Indentation defines the structure of YAML documents.

Incorrect indentation can cause parsing failures or change the meaning of the document.

Incorrect:

```yaml
application:
  name: hands-on-notes
   environment: development
```

Correct:

```yaml
application:
  name: hands-on-notes
  environment: development
```

Both properties must have the same indentation because they belong to the same mapping.

# 🗁 Using Tabs Instead of Spaces

YAML indentation must use spaces. Tabs are not permitted for indentation.

Incorrect:

```yaml
application:
 name: hands-on-notes
 environment: development
```

Correct:

```yaml
application:
  name: hands-on-notes
  environment: development
```

Configure your code editor to insert spaces when pressing the Tab key.

# 🗁 Missing Space After a Colon

A mapping uses a colon followed by a space to separate a key from its value.

Incorrect:

```yaml
name:Hands-on-Notes
port:3000
```

Correct:

```yaml
name: Hands-on-Notes
port: 3000
```

A colon without a following space may be interpreted as part of a plain scalar rather than a mapping separator.

# 🗁 Missing Space After a Sequence Hyphen

Sequence items must begin with a hyphen followed by a space.

Incorrect:

```yaml
languages:
  -JavaScript
  -Python
  -PHP
```

Correct:

```yaml
languages:
  - JavaScript
  - Python
  - PHP
```

# 🗁 Incorrect Nesting

Nested mappings and sequences must use consistent indentation.

Incorrect:

```yaml
server:
  name: web-server

  database:
    host: localhost
   port: 5432
```

Correct:

```yaml
server:
  name: web-server

  database:
    host: localhost
    port: 5432
```

Incorrect nesting can prevent parsing or place a property at an unintended level.

# 🗁 Incorrect Sequence Alignment

Sequence items and their nested properties must be aligned correctly.

Incorrect:

```yaml
users:
  - name: Alice
    role: admin
   - name: Bob
    role: developer
```

Correct:

```yaml
users:
  - name: Alice
    role: admin

  - name: Bob
    role: developer
```

Each sequence item begins at the same indentation level.

# 🗁 Quoting and Special Characters

Some characters have special meanings in YAML.

Examples include:

* `:` mapping separator.
* `#` comment indicator.
* `[` and `]` flow sequence delimiters.
* `{` and `}` flow mapping delimiters.
* `&` anchor indicator.
* `*` alias indicator.
* `!` tag indicator.
* `|` literal block scalar indicator.
* `>` folded block scalar indicator.

When a string contains characters that could be interpreted as YAML syntax, quoting can make the intended value explicit.

Potentially problematic:

```yaml
message: Hello: World
```

Correct:

```yaml
message: "Hello: World"
```

A string beginning with `#` should also be quoted.

```yaml
color: "#ffffff"
```

# 🗁 Single and Double Quotation Marks

YAML supports single-quoted and double-quoted strings.

Single-quoted strings:

```yaml
message: 'Hello, World!'
```

Double-quoted strings:

```yaml
message: "Hello, World!"
```

The main difference is escape processing.

Double-quoted strings support escape sequences:

```yaml
message: "First line\nSecond line"
```

Single-quoted strings preserve backslashes literally. To represent a single quote inside a single-quoted string, double it.

```yaml
message: 'It''s a YAML example'
```

Use the quoting style that best represents the intended value.

# 🗁 Boolean Differences Between YAML Versions

Boolean interpretation can differ between YAML 1.1 and YAML 1.2.

YAML 1.1 recognizes additional Boolean-like values, including:

```yaml
enabled: yes
disabled: no
active: on
inactive: off
```

YAML 1.2 uses `true` and `false` as the standard Boolean values.

```yaml
enabled: true
disabled: false
```

For predictable behavior across parsers and applications, prefer explicit `true` and `false`.

If `yes`, `no`, `on`, or `off` must remain strings, quote them.

```yaml
answer: "yes"
mode: "on"
```

Parser behavior also depends on the YAML version and library configuration.

# 🗁 Implicit Type Conversion

YAML parsers can automatically interpret unquoted values as numbers, Booleans, null values, or other scalar types.

Example:

```yaml
settings:
  port: 8080
  enabled: true
  timeout: 30.5
  username: admin
  description: null
```

These values are interpreted as different scalar types.

However, some values may be interpreted differently than expected.

For example:

```yaml
version: 1.0
```

This represents a numeric value, not necessarily the string `"1.0"`.

If an application expects a string, quote it:

```yaml
version: "1.0"
```

# 🗁 Date and Timestamp Interpretation

Some YAML parsers recognize date-like values as timestamps.

Example:

```yaml
created_at: 2026-11-12
```

Depending on the parser, this may become a date object or another timestamp representation rather than a string.

To preserve it as a string:

```yaml
created_at: "2026-11-12"
```

This is particularly important when dates are passed to APIs or application code that expects strings.

# 🗁 Duplicate Mapping Keys

A mapping should not contain duplicate keys.

Incorrect:

```yaml
server:
  port: 3000
  host: localhost
  port: 8080
```

The duplicate `port` key creates ambiguity.

Some parsers reject duplicate keys, while others may silently keep the first or last value.

Correct:

```yaml
server:
  port: 8080
  host: localhost
```

Use unique keys within each mapping and enable duplicate-key detection when supported by your parser.

# 🗁 Invalid Flow Sequences

Flow sequences use square brackets.

Incorrect:

```yaml
languages: [JavaScript, Python, PHP
```

Correct:

```yaml
languages: [JavaScript, Python, PHP]
```

The closing bracket is required.

# 🗁 Invalid Flow Mappings

Flow mappings use curly braces.

Incorrect:

```yaml
database: {host: localhost, port: 5432
```

Correct:

```yaml
database: {host: localhost, port: 5432}
```

Both opening and closing braces must be present.

# 🗁 Block Scalar Indentation Errors

YAML supports multiline strings using literal (`|`) and folded (`>`) block scalars.

Literal style preserves line breaks:

```yaml
description: |
  This is the first line.
  This is the second line.
  This is the third line.
```

Folded style generally joins adjacent non-empty lines with spaces:

```yaml
description: >
  This is the first line.
  This is the second line.
  This is the third line.
```

Incorrect indentation can cause parsing problems or unexpected content.

Correct indentation:

```yaml
script: |
  npm install
  npm run build
  npm test
```

All lines belonging to the block scalar must be indented beneath the key.

# 🗁 Block Scalar Chomping Indicators

YAML provides chomping indicators to control trailing newline behavior.

| Indicator | Behavior |
| --- | --- |
| ` | ` | Clip trailing newlines to the default single final newline |
| ` | -` | Strip trailing newlines |
| ` | +` | Keep trailing newlines |

Examples:

```yaml
default_message: |
  Hello
  World

trimmed_message: |-
  Hello
  World

preserved_message: |+
  Hello
  World
```

The same chomping indicators can be used with folded style.

```yaml
description: >-
  This is a folded
  multiline string.
```

Choose the indicator based on how the resulting string should end.

# 🗁 Incorrect Document Markers

YAML supports document start and end markers.

* `---` indicates the beginning of a document.
* `...` indicates the end of a document.

Example:

```yaml
---
name: application
environment: production
...
```

Multiple documents can exist in one YAML stream.

```yaml
---
name: development
port: 3000

---
name: production
port: 8080
```

A common mistake is treating document markers as ordinary separators inside a mapping.

Use them only when the document structure requires them.

# 🗁 Anchor and Alias Errors

YAML supports anchors (`&`) and aliases (`*`) for reusing previously defined nodes.

Example:

```yaml
defaults: &defaults
  environment: production
  logging: true

application:
  <<: *defaults
  name: hands-on-notes
```

Here:

* `&defaults` defines an anchor.
* `*defaults` references that anchor.
* `<<` uses the merge-key convention to merge mapping entries.

Merge-key support is not equally standardized across all YAML versions and parsers, so verify compatibility before relying on it.

Common mistakes include:

* Referencing an undefined anchor.
* Using an invalid alias name.
* Creating unsupported or problematic recursive references.
* Assuming every parser supports mapping merges.

# 🗁 Special Characters in Plain Scalars

Plain scalars are unquoted strings.

Some character combinations can change how a value is interpreted.

Potentially problematic:

```yaml
message: #Welcome
```

The `#` begins a comment when it appears in a position where a comment is permitted.

Correct:

```yaml
message: "#Welcome"
```

Another example:

```yaml
url: https://example.com/path?a=1&b=2
```

This is generally valid as a plain scalar, but quoting URLs can make configuration values easier to maintain.

```yaml
url: "https://example.com/path?a=1&b=2"
```

# 🗁 Parser-Specific Behavior

Different YAML libraries may behave differently depending on their supported YAML versions, schema settings, and implementation choices.

For example:

* Boolean interpretation can vary.
* Timestamp conversion can vary.
* Duplicate-key handling can vary.
* Custom tags may not be supported.
* Anchor and merge behavior can vary.
* Serialization formatting can differ.

A document that parses successfully in one tool may behave differently in another.

Use the parser and configuration supported by your actual application environment.

# 🗁 JavaScript YAML Parsing Errors

The `yaml` npm package can parse and serialize YAML.

Install it:

```bash
npm install yaml
```

Example:

```javascript
import fs from "node:fs";
import YAML from "yaml";

try {
  const content = fs.readFileSync("config.yaml", "utf8");

  const config = YAML.parse(content);

  console.log(config);
} catch (error) {
  console.error("Unable to parse YAML:", error.message);
}
```

For more detailed diagnostics, use the document API:

```javascript
import fs from "node:fs";
import YAML from "yaml";

const content = fs.readFileSync("config.yaml", "utf8");

const document = YAML.parseDocument(content);

if (document.errors.length > 0) {
  for (const error of document.errors) {
    console.error(error.message);
  }
} else {
  console.log(document.toJS());
}
```

The document API exposes parsing errors and warnings and can help when building YAML validation tools.

# 🗁 Python YAML Parsing Errors

PyYAML provides parsing and serialization functionality.

Install it:

```bash
pip install pyyaml
```

Example:

```python
import yaml

try:
    with open("config.yaml", "r", encoding="utf-8") as file:
        config = yaml.safe_load(file)

    print(config)

except yaml.YAMLError as error:
    print("Unable to parse YAML:", error)
```

Use `safe_load()` for ordinary configuration files rather than unrestricted object construction with `load()`.

# 🗁 YAML Syntax Validation vs Application Validation

A YAML file can be syntactically valid but still contain incorrect configuration.

Consider:

```yaml
server:
  port: "not-a-number"
  host: localhost
```

This may be valid YAML, but an application expecting an integer port can reject it.

There are two separate validation stages:

**Syntax validation**

Checks whether the document follows YAML grammar.

Examples:

* Correct indentation.
* Valid mappings and sequences.
* Proper quotation marks.
* Correct flow-style delimiters.

**Application validation**

Checks whether the parsed data satisfies the application's requirements.

Examples:

* Port must be an integer.
* Host must be a valid hostname or IP address.
* Required properties must exist.
* Environment values must be supported.
* Configuration values must meet business rules.

Use schema validation when configuration correctness matters.

# 🗁 Debugging Workflow

Follow a systematic process when a YAML file fails to load.

* Identify the exact file and parser reporting the error.
* Read the error message and locate the reported line and column.
* Inspect indentation around the reported location.
* Check for tabs and inconsistent spaces.
* Verify colons, hyphens, brackets, and braces.
* Check quotation marks and special characters.
* Look for duplicate keys and unexpected scalar types.
* Validate the file using the same parser used by the application.
* Check application-specific schema requirements.
* Re-run the application or deployment process after correcting the configuration.

# 🗁 Useful YAML Validation Tools

Some useful tools for validating and troubleshooting YAML include:

| Tool | Purpose |
| --- | --- |
| `yaml` npm package | Parse, serialize, and inspect YAML in JavaScript |
| PyYAML | Parse and serialize YAML in Python |
| yamllint | Check YAML formatting and common issues |
| Red Hat YAML extension | YAML support and diagnostics in Visual Studio Code |
| Kubernetes validation tools | Validate Kubernetes manifests |
| Docker Compose | Validate and inspect Compose configurations |

# 🗁 Practical Example: Fixing a Broken Configuration

Incorrect configuration:

```yaml
application:
  name: hands-on-notes
  enabled: yes
  port:3000

  services:
    -name: api
      ports:
       - 3000
      - 4000

  database:
    host: localhost
    port: 5432
    port: 3306
```

Problems include:

* `yes` may be interpreted differently depending on the YAML version and parser.
* Missing space after `port:`.
* Missing space after sequence hyphens.
* Incorrect sequence indentation.
* Duplicate `port` mapping key.

Corrected configuration:

```yaml
application:
  name: hands-on-notes
  enabled: true
  port: 3000

  services:
    - name: api
      ports:
        - 3000
        - 4000

  database:
    host: localhost
    port: 5432
```

The corrected version uses consistent indentation, valid sequence syntax, explicit Boolean values, and unique mapping keys.

# 🗁 Best Practices

* Use spaces instead of tabs.
* Keep indentation consistent throughout the file.
* Prefer two-space indentation.
* Always add a space after mapping colons and sequence hyphens.
* Quote ambiguous strings.
* Use `true` and `false` for Boolean values.
* Avoid duplicate mapping keys.
* Use literal or folded block scalars intentionally.
* Understand how the selected parser handles dates and implicit types.
* Use safe parsing functions.
* Validate both YAML syntax and application-specific requirements.
* Use a YAML linter in development workflows.
* Keep configuration files simple and maintainable.

# 🗁 Summary

Most YAML problems come from indentation, whitespace, special characters, implicit type conversion, and incorrect nesting.

Key concepts:

* YAML indentation must use spaces.
* Mapping colons and sequence hyphens require appropriate spacing.
* Quoting prevents ambiguous scalar interpretation.
* YAML versions and parsers can differ in type resolution.
* Duplicate mapping keys should be avoided.
* Block scalar indentation and chomping affect multiline strings.
* Syntax validation and application validation are separate processes.
* JavaScript and Python provide libraries for parsing and diagnosing YAML errors.
