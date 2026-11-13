# YAML Objects

YAML objects are represented using **mappings**. A mapping is a collection of key-value pairs where each key is associated with a value.

Mappings are commonly used to organize related information, represent application settings, and define structured configuration data.

Unlike JSON, YAML does not require curly braces around objects when using block style. Instead, it uses indentation to represent relationships between properties.

# 🗁 What Is a YAML Mapping?

A mapping associates keys with values.

The basic syntax is:

```yaml
key: value
```

Example:

```yaml
name: John
age: 25
city: Dhaka
```

This mapping contains three properties:

* `name` has the value `John`.
* `age` has the value `25`.
* `city` has the value `Dhaka`.

# 🗁 YAML Object Syntax

A YAML mapping can contain multiple key-value pairs.

```yaml
user:
  id: 101
  name: John
  email: john@example.com
  role: developer
```

Here, `user` is a mapping containing four properties.

The indentation indicates that these properties belong to the `user` mapping.

# 🗁 Mapping Keys

YAML mapping keys are commonly strings.

```yaml
name: John
age: 25
active: true
```

Keys can also be quoted.

**Unquoted keys**

```yaml
name: John
email: john@example.com
```

**Single-quoted keys**

```yaml
'name': John
'email': john@example.com
```

**Double-quoted keys**

```yaml
"name": John
"email": john@example.com
```

Quoted keys are useful when keys contain special characters or need an explicit string representation.

# 🗁 Keys Containing Special Characters

Some characters have special meanings in YAML syntax.

When a key contains characters that could cause ambiguity, quote it.

```yaml
"first name": John
"last name": Smith
"email-address": john@example.com
"server:port": localhost:3000
```

This ensures that the parser interprets the complete text as the key.

# 🗁 Nested Mappings

A mapping can contain another mapping as its value.

This creates a nested object structure.

```yaml
user:
  id: 101
  name: John
  address:
    city: Dhaka
    country: Bangladesh
    postal_code: 1207
```

The structure contains:

* `user` as the root mapping.
* `id`, `name`, and `address` inside `user`.
* `city`, `country`, and `postal_code` inside `address`.

Nested mappings are useful for grouping related properties.

# 🗁 Multiple Nested Objects

YAML supports multiple nested mappings at different levels.

```yaml
company:
  name: Example Technologies
  location:
    city: Dhaka
    country: Bangladesh

  departments:
    development:
      manager: John
      employees: 15

    marketing:
      manager: Sarah
      employees: 8
```

This example represents a company with location and department information.

Each department is a nested mapping containing its own properties.

# 🗁 Empty Mappings

An empty mapping contains no key-value pairs.

YAML supports two common representations.

**Block style**

```yaml
user:
```

**Flow style**

```yaml
user: {}
```

Both represent an empty mapping.

An empty mapping is different from a null value.

```yaml
user: {}
address: null
```

* `user` is an empty mapping.
* `address` is a null value.

# 🗁 Flow-Style Mappings

YAML supports flow-style mappings using curly braces.

```yaml
user: {name: John, age: 25, city: Dhaka}
```

This represents the same structure as:

```yaml
user:
  name: John
  age: 25
  city: Dhaka
```

Flow style is compact and resembles JSON syntax.

Block style is generally easier to read when mappings contain many properties.

# 🗁 Mapping Order

YAML mappings represent associations between keys and values, not ordered sequences.

Although parsers often preserve the order in which mapping entries appear, applications should not depend on mapping order for meaning.

If order matters, use a sequence.

**Mapping**

```yaml
user:
  name: John
  age: 25
  city: Dhaka
```

**Sequence**

```yaml
steps:
  - Install dependencies
  - Build application
  - Run tests
```

The sequence explicitly represents an ordered collection.

# 🗁 Accessing YAML Objects in JavaScript

When YAML is parsed into JavaScript, mappings are commonly represented as JavaScript objects.

Install the `yaml` package:

```bash
npm install yaml
```

Example:

```javascript
import YAML from "yaml";

const yamlText = `
user:
  id: 101
  name: John
  address:
    city: Dhaka
    country: Bangladesh
`;

const data = YAML.parse(yamlText);

console.log(data.user.name);
console.log(data.user.address.city);
console.log(data.user.address.country);
```

You can also use bracket notation:

```javascript
console.log(data["user"]["name"]);
console.log(data["user"]["address"]["city"]);
```

# 🗁 Accessing YAML Objects in Python

When YAML is parsed into Python, mappings are commonly represented as dictionaries.

Install PyYAML:

```bash
pip install pyyaml
```

Example:

```python
import yaml

yaml_text = """
user:
  id: 101
  name: John
  address:
    city: Dhaka
    country: Bangladesh
"""

data = yaml.safe_load(yaml_text)

print(data["user"]["name"])
print(data["user"]["address"]["city"])
print(data["user"]["address"]["country"])
```

Python uses dictionary keys to access mapping values.

# 🗁 Updating YAML Objects in JavaScript

After parsing YAML, you can modify the resulting JavaScript object.

```javascript
import YAML from "yaml";

const yamlText = `
user:
  name: John
  age: 25
`;

const data = YAML.parse(yamlText);

// Update a property
data.user.name = "David";

// Add a property
data.user.email = "david@example.com";

// Remove a property
delete data.user.age;

// Convert the object back into YAML
const updatedYaml = YAML.stringify(data);

console.log(updatedYaml);
```

# 🗁 Updating YAML Objects in Python

After parsing YAML, you can modify the resulting Python dictionary.

```python
import yaml

yaml_text = """
user:
  name: John
  age: 25
"""

data = yaml.safe_load(yaml_text)

# Update a property
data["user"]["name"] = "David"

# Add a property
data["user"]["email"] = "david@example.com"

# Remove a property
del data["user"]["age"]

# Convert the dictionary back into YAML
updated_yaml = yaml.safe_dump(data, sort_keys=False)

print(updated_yaml)
```

# 🗁 Iterating Over YAML Objects in JavaScript

JavaScript provides several methods for iterating over object properties.

**Using `Object.keys()`**

```javascript
const user = {
  name: "John",
  age: 25,
  city: "Dhaka"
};

for (const key of Object.keys(user)) {
  console.log(key);
}
```

**Using `Object.values()`**

```javascript
for (const value of Object.values(user)) {
  console.log(value);
}
```

**Using `Object.entries()`**

```javascript
for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}
```

# 🗁 Iterating Over YAML Objects in Python

Python provides dictionary methods for iterating over mappings.

**Using `keys()`**

```python
user = {
    "name": "John",
    "age": 25,
    "city": "Dhaka"
}

for key in user.keys():
    print(key)
```

**Using `values()`**

```python
for value in user.values():
    print(value)
```

**Using `items()`**

```python
for key, value in user.items():
    print(f"{key}: {value}")
```

# 🗁 YAML Objects vs JSON Objects

YAML mappings and JSON objects represent similar key-value structures.

| Feature | YAML Mapping | JSON Object |
| --- | --- | --- |
| Syntax | `key: value` | `"key": "value"` |
| Block style | Indentation | Braces |
| Flow style | `{}` | `{}` |
| Nested objects | Supported | Supported |
| Comments | Supported | Not supported |
| Key-value separator | Colon | Colon |
| Key quotation | Often optional | Required |
| Empty object | `{}` | `{}` |

**YAML**

```yaml
user:
  name: John
  age: 25
```

**JSON**

```json
{
  "user": {
    "name": "John",
    "age": 25
  }
}
```

# 🗁 Practical Example: Application Configuration

Application settings are frequently organized using YAML mappings.

```yaml
app:
  name: my-application
  version: "1.0"
  environment: development

server:
  host: localhost
  port: 3000

database:
  host: localhost
  port: 5432
  name: application_db
  username: admin

logging:
  level: debug
  enabled: true
```

This structure separates application, server, database, and logging settings into independent mappings.

# 🗁 Practical Example: Database Configuration

```yaml
database:
  primary:
    driver: postgresql
    host: localhost
    port: 5432
    database: production_db

  replica:
    driver: postgresql
    host: replica.example.com
    port: 5432
    database: production_db

  connection:
    pool_size: 20
    timeout: 30
    ssl: true
```

This example demonstrates multiple nested mappings for database configuration.

# 🗁 Best Practices

* Use meaningful and descriptive mapping keys.
* Keep indentation consistent.
* Prefer block style for complex mappings.
* Quote keys containing special characters.
* Group related properties together.
* Avoid unnecessarily deep nesting.
* Use unique keys within each mapping.
* Use sequences when element order is important.
* Distinguish empty mappings from null values.
* Validate YAML after modifying configuration files.
* Preserve important configuration structure when updating values.

# 🗁 Summary

* YAML objects are represented using mappings.
* Mappings contain key-value pairs.
* Keys are commonly strings.
* Nested mappings represent hierarchical objects.
* Empty mappings can be represented using `{}`.
* Flow-style mappings use curly braces.
* Mapping order should not be relied upon for application logic.
* JavaScript commonly converts mappings into objects.
* Python commonly converts mappings into dictionaries.
* YAML mappings are widely used for application and infrastructure configuration.
