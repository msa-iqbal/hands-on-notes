# YAML Arrays

YAML arrays are called **sequences**. A sequence represents an ordered collection of values.

Sequences are commonly used to store:

* Lists of strings, numbers, or Boolean values.
* Collections of objects.
* Application configuration options.
* Server, database, and service definitions.
* Deployment steps and environment settings.

YAML supports two sequence styles:

* **Block style:** Uses hyphens (`-`) and indentation.
* **Flow style:** Uses square brackets (`[]`).

# 🗁 Block Style Sequences

Block style is the most common and readable way to define YAML sequences.

Each item starts with a hyphen followed by a space.

```yaml
languages:
  - JavaScript
  - Python
  - PHP
  - Java
```

The equivalent JSON representation is:

```json
{
  "languages": [
    "JavaScript",
    "Python",
    "PHP",
    "Java"
  ]
}
```

# 🗁 Flow Style Sequences

Flow style defines a sequence using square brackets.

```yaml
languages: [JavaScript, Python, PHP, Java]
```

Flow style is useful for short collections, while block style is generally easier to read when a sequence contains many items.

# 🗁 Empty Sequences

An empty sequence contains no elements.

```yaml
languages: []
```

This represents an empty array.

# 🗁 Sequences of Scalar Values

A sequence can contain different scalar data types.

```yaml
settings:
  - development
  - 8080
  - true
  - null
  - 3.14
```

The values represent:

* `development`: String
* `8080`: Integer
* `true`: Boolean
* `null`: Null
* `3.14`: Floating-point number

You can also explicitly quote values to ensure they are interpreted as strings.

```yaml
ports:
  - "3000"
  - "8080"
  - "9000"
```

# 🗁 Sequences of Objects

A sequence can contain multiple mappings (objects).

Each object begins with a hyphen, and its properties are indented beneath it.

```yaml
users:
  - id: 1
    name: Alice
    role: admin

  - id: 2
    name: Bob
    role: developer

  - id: 3
    name: Charlie
    role: editor
```

Equivalent JSON:

```json
{
  "users": [
    {
      "id": 1,
      "name": "Alice",
      "role": "admin"
    },
    {
      "id": 2,
      "name": "Bob",
      "role": "developer"
    },
    {
      "id": 3,
      "name": "Charlie",
      "role": "editor"
    }
  ]
}
```

# 🗁 Nested Sequences

A sequence can contain other sequences.

```yaml
matrix:
  - - 1
    - 2
    - 3

  - - 4
    - 5
    - 6

  - - 7
    - 8
    - 9
```

This represents a two-dimensional array.

A more compact flow-style equivalent is:

```yaml
matrix:
  - [1, 2, 3]
  - [4, 5, 6]
  - [7, 8, 9]
```

# 🗁 Mixing Sequences and Mappings

YAML allows sequences and mappings to be combined.

```yaml
projects:
  - name: frontend
    technologies:
      - React
      - TypeScript
      - Tailwind CSS

  - name: backend
    technologies:
      - Node.js
      - Express
      - PostgreSQL
```

Here:

* `projects` is a sequence.
* Each project is a mapping.
* `technologies` is another sequence inside each mapping.

This structure is useful for complex application configurations.

# 🗁 Indentation Rules

YAML uses indentation to represent structure.

Correct example:

```yaml
servers:
  - name: web-01
    ip: 192.168.1.10

  - name: web-02
    ip: 192.168.1.11
```

Incorrect example:

```yaml
servers:
  - name: web-01
   ip: 192.168.1.10
```

The second example has inconsistent indentation.

Important rules:

* Use spaces instead of tabs.
* Add a space after every sequence hyphen.
* Keep properties of the same object aligned.
* Indent nested collections consistently.
* Use a consistent indentation width, commonly two spaces.

# 🗁 Accessing YAML Sequences in JavaScript

After parsing YAML, sequences become native JavaScript arrays.

Install the `yaml` package:

```bash
npm install yaml
```

Example YAML file (`config.yaml`):

```yaml
languages:
  - JavaScript
  - Python
  - PHP

users:
  - name: Alice
    role: admin
  - name: Bob
    role: developer
```

Read and parse the file:

```javascript
import fs from "node:fs";
import YAML from "yaml";

const content = fs.readFileSync("config.yaml", "utf8");

const config = YAML.parse(content);

console.log(config.languages);
console.log(config.users);
```

Access individual elements:

```javascript
console.log(config.languages[0]);
// JavaScript

console.log(config.users[0].name);
// Alice

console.log(config.users[1].role);
// developer
```

# 🗁 Iterating Over Sequences in JavaScript

Use `for...of`, `forEach()`, or other array methods.

```javascript
for (const language of config.languages) {
  console.log(language);
}
```

Using `forEach()`:

```javascript
config.languages.forEach((language, index) => {
  console.log(index, language);
});
```

Iterating over objects:

```javascript
for (const user of config.users) {
  console.log(user.name, user.role);
}
```

# 🗁 Modifying Sequences in JavaScript

Parsed YAML sequences are regular JavaScript arrays.

```javascript
config.languages.push("Go");

config.languages.pop();

config.languages[0] = "TypeScript";
```

Add an object:

```javascript
config.users.push({
  name: "Charlie",
  role: "editor"
});
```

Serialize the updated configuration:

```javascript
const updatedYaml = YAML.stringify(config);

fs.writeFileSync("config.yaml", updatedYaml);
```

# 🗁 Accessing YAML Sequences in Python

When YAML is parsed using PyYAML, sequences become native Python lists.

Install PyYAML:

```bash
pip install pyyaml
```

Example:

```python
import yaml

with open("config.yaml", "r", encoding="utf-8") as file:
    config = yaml.safe_load(file)

print(config["languages"])
print(config["users"])
```

Access individual elements:

```python
print(config["languages"][0])
# JavaScript

print(config["users"][0]["name"])
# Alice

print(config["users"][1]["role"])
# developer
```

# 🗁 Iterating Over Sequences in Python

Use a `for` loop to iterate over a sequence.

```python
for language in config["languages"]:
    print(language)
```

Iterating over objects:

```python
for user in config["users"]:
    print(user["name"], user["role"])
```

Using `enumerate()`:

```python
for index, language in enumerate(config["languages"]):
    print(index, language)
```

# 🗁 Modifying Sequences in Python

Parsed YAML sequences are regular Python lists.

```python
config["languages"].append("Go")

config["languages"].remove("PHP")

config["languages"][0] = "TypeScript"
```

Add an object:

```python
config["users"].append({
    "name": "Charlie",
    "role": "editor"
})
```

Save the updated configuration:

```python
with open("config.yaml", "w", encoding="utf-8") as file:
    yaml.safe_dump(
        config,
        file,
        sort_keys=False,
        allow_unicode=True
    )
```

# 🗁 Practical Example: Application Configuration

```yaml
application:
  name: hands-on-notes
  environment: development

  allowed_origins:
    - http://localhost:3000
    - http://localhost:5173

  features:
    - authentication
    - logging
    - caching

  database:
    host: localhost
    port: 5432
    databases:
      - development
      - testing
```

This example combines sequences, mappings, and nested values.

# 🗁 Practical Example: Docker Compose

A Docker Compose configuration commonly uses sequences for services, ports, volumes, and environment entries.

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro

  api:
    image: node:22
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
```

Notice that port mappings are quoted to preserve their intended string representation.

# 🗁 Practical Example: GitHub Actions

GitHub Actions uses sequences for workflow triggers, jobs, steps, and commands.

```yaml
name: Node.js CI

on:
  push:
    branches:
      - main
      - develop

  pull_request:
    branches:
      - main

jobs:
  test:
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

# 🗁 Practical Example: Kubernetes Manifest

Kubernetes manifests use sequences for containers, ports, environment variables, and other resource collections.

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: web-pod

spec:
  containers:
    - name: nginx
      image: nginx:latest

      ports:
        - containerPort: 80

    - name: sidecar
      image: busybox:latest
      command:
        - sh
        - -c
        - "while true; do sleep 3600; done"
```

# 🗁 YAML Sequences vs Programming Language Arrays

YAML sequences are converted into the corresponding native collection type when parsed.

| YAML concept | JavaScript | Python |
| --- | --- | --- |
| Sequence | Array | List |
| Empty sequence | `[]` | `[]` |
| First element | `[0]` | `[0]` |
| Add element | `push()` | `append()` |
| Iterate | `for...of` | `for` |

YAML sequences preserve item order. This makes them suitable for ordered configuration entries, deployment steps, and command lists.

# 🗁 Common Mistakes

**Missing space after the hyphen**

Incorrect:

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

**Incorrect indentation**

Incorrect:

```yaml
users:
  - name: Alice
   role: admin
```

Correct:

```yaml
users:
  - name: Alice
    role: admin
```

**Incorrect sequence alignment**

Incorrect:

```yaml
matrix:
  - - 1
   - 2
```

Correct:

```yaml
matrix:
  - - 1
    - 2
```

**Invalid flow sequence**

Incorrect:

```yaml
languages: [JavaScript, Python
```

Correct:

```yaml
languages: [JavaScript, Python]
```

**Unexpected scalar type**

Potentially ambiguous:

```yaml
ports:
  - 8080
```

Explicit string:

```yaml
ports:
  - "8080"
```

Choose the type expected by the application rather than assuming every numeric-looking value should be a number.

# 🗁 Best Practices

* Prefer block style for long or complex sequences.
* Use flow style for short, simple collections.
* Keep sequence indentation consistent.
* Use descriptive property names in objects.
* Quote values when their intended type could be ambiguous.
* Avoid mixing unrelated data types in one sequence unless necessary.
* Use a YAML parser to validate configuration files.
* Remember that parsed sequences become native arrays or lists.
* Preserve sequence order when order matters to the application.
* Validate application-specific configuration separately from YAML syntax.

# 🗁 Summary

YAML sequences represent ordered collections of values and are equivalent to arrays in JavaScript and lists in Python after parsing.

Key concepts:

* Block sequences use hyphens.
* Flow sequences use square brackets.
* Sequences can contain scalar values, mappings, or other sequences.
* Indentation determines nested structure.
* JavaScript parses sequences as arrays.
* Python parses sequences as lists.
* YAML libraries can parse and serialize sequence data.
