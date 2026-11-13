# YAML Nesting

YAML nesting is the practice of placing mappings (objects) and sequences (arrays or lists) inside other mappings and sequences.

Nesting allows you to represent hierarchical data in a readable structure.

It is commonly used in:

* Application configuration files.
* Docker Compose configurations.
* Kubernetes manifests.
* GitHub Actions workflows.
* API configuration and structured data.
* Infrastructure automation.

YAML uses indentation to define relationships between nested elements.

# 🗁 Understanding Nested Mappings

A mapping contains key-value pairs. A nested mapping is a mapping placed inside another mapping.

```yaml
application:
  name: hands-on-notes
  environment: development

  database:
    host: localhost
    port: 5432
    username: admin
```

The structure can be represented as:

```text
application
├── name
├── environment
└── database
    ├── host
    ├── port
    └── username
```

Here, `database` is a nested mapping inside `application`.

# 🗁 Understanding Nested Sequences

A sequence can contain another sequence.

```yaml
departments:
  - Development
  - Networking
  - Security

teams:
  - - Frontend
    - Backend

  - - DevOps
    - Infrastructure
```

The `teams` sequence contains two nested sequences.

# 🗁 Mapping Inside a Mapping

Mappings can be nested at multiple levels.

```yaml
company:
  name: Example Technologies

  address:
    country: Bangladesh
    city: Dhaka

    office:
      building: Tech Tower
      floor: 5
      room: 502
```

Each indentation level represents a deeper relationship.

# 🗁 Sequence Inside a Mapping

A mapping can contain a sequence as one of its values.

```yaml
server:
  hostname: web-server
  ip: 192.168.1.10

  services:
    - nginx
    - ssh
    - docker
```

Here, `services` is a sequence inside the `server` mapping.

# 🗁 Mapping Inside a Sequence

A sequence can contain multiple mappings.

```yaml
employees:
  - id: 101
    name: Alice
    department: Engineering

  - id: 102
    name: Bob
    department: Operations

  - id: 103
    name: Charlie
    department: Security
```

Each sequence item represents an employee object.

# 🗁 Combining Mappings and Sequences

Complex YAML structures commonly combine mappings and sequences.

```yaml
company:
  name: Example Technologies

  departments:
    - name: Engineering
      manager: Alice

      teams:
        - Frontend
        - Backend

    - name: Operations
      manager: Bob

      teams:
        - DevOps
        - Infrastructure
```

This structure contains:

* A root mapping named `company`.
* A nested mapping containing company information.
* A sequence named `departments`.
* Multiple department mappings.
* Nested sequences named `teams`.

# 🗁 Deeply Nested Structures

YAML can represent several levels of nested data.

```yaml
application:
  name: ecommerce-platform

  infrastructure:
    environment: production

    region:
      country: Bangladesh
      city: Dhaka

    servers:
      - name: web-server
        role: frontend

        network:
          ip: 192.168.1.10
          ports:
            - 80
            - 443

      - name: api-server
        role: backend

        network:
          ip: 192.168.1.20
          ports:
            - 3000
            - 5000
```

A deeply nested structure should remain logically organized and consistently indented.

# 🗁 Accessing Nested Properties in JavaScript

After parsing YAML using the `yaml` package, nested mappings become JavaScript objects and nested sequences become arrays.

Install the package:

```bash
npm install yaml
```

Example YAML:

```yaml
application:
  name: ecommerce-platform

  database:
    host: localhost
    port: 5432

  servers:
    - name: web-server
      ip: 192.168.1.10

    - name: api-server
      ip: 192.168.1.20
```

Parse the YAML:

```javascript
import fs from "node:fs";
import YAML from "yaml";

const content = fs.readFileSync("config.yaml", "utf8");

const config = YAML.parse(content);
```

Access nested values:

```javascript
console.log(config.application.name);
// ecommerce-platform

console.log(config.application.database.host);
// localhost

console.log(config.application.servers[0].name);
// web-server

console.log(config.application.servers[1].ip);
// 192.168.1.20
```

# 🗁 Accessing Nested Properties Safely in JavaScript

Use optional chaining (`?.`) when a nested property might not exist.

```javascript
const databaseHost = config.application?.database?.host;

console.log(databaseHost);
```

You can also provide a default value using nullish coalescing (`??`).

```javascript
const databasePort = config.application?.database?.port ?? 3306;

console.log(databasePort);
```

This prevents errors when an optional property is missing or `null`.

# 🗁 Accessing Nested Sequences in JavaScript

Use array indexes to access elements inside nested sequences.

```yaml
projects:
  - name: frontend
    technologies:
      - React
      - TypeScript

  - name: backend
    technologies:
      - Node.js
      - PostgreSQL
```

JavaScript:

```javascript
console.log(config.projects[0].name);
// frontend

console.log(config.projects[0].technologies[0]);
// React

console.log(config.projects[1].technologies[1]);
// PostgreSQL
```

# 🗁 Iterating Over Nested Structures in JavaScript

Nested structures can be traversed using loops.

```javascript
for (const project of config.projects) {
  console.log(`Project: ${project.name}`);

  for (const technology of project.technologies) {
    console.log(`- ${technology}`);
  }
}
```

Using `forEach()`:

```javascript
config.projects.forEach((project) => {
  console.log(project.name);

  project.technologies.forEach((technology) => {
    console.log(technology);
  });
});
```

# 🗁 Modifying Nested Structures in JavaScript

Nested properties can be modified like regular JavaScript objects.

```javascript
config.projects[0].name = "frontend-platform";

config.projects[0].technologies.push("Vite");

config.projects.push({
  name: "mobile",
  technologies: ["React Native", "TypeScript"]
});
```

Serialize the modified object:

```javascript
const updatedYaml = YAML.stringify(config);

fs.writeFileSync("config.yaml", updatedYaml);
```

# 🗁 Accessing Nested Properties in Python

PyYAML converts nested mappings into dictionaries and nested sequences into lists.

Install PyYAML:

```bash
pip install pyyaml
```

Example YAML:

```yaml
application:
  name: ecommerce-platform

  database:
    host: localhost
    port: 5432

  servers:
    - name: web-server
      ip: 192.168.1.10

    - name: api-server
      ip: 192.168.1.20
```

Parse the YAML:

```python
import yaml

with open("config.yaml", "r", encoding="utf-8") as file:
    config = yaml.safe_load(file)
```

Access nested values:

```python
print(config["application"]["name"])
# ecommerce-platform

print(config["application"]["database"]["host"])
# localhost

print(config["application"]["servers"][0]["name"])
# web-server

print(config["application"]["servers"][1]["ip"])
# 192.168.1.20
```

# 🗁 Accessing Nested Properties Safely in Python

Use the dictionary `get()` method to access optional properties.

```python
database = config.get("application", {}).get("database", {})

database_host = database.get("host", "localhost")

print(database_host)
```

This avoids a `KeyError` when a key is missing.

For deeply nested structures, you can use intermediate variables to improve readability.

```python
application = config.get("application", {})
database = application.get("database", {})

host = database.get("host")
port = database.get("port", 5432)
```

# 🗁 Accessing Nested Sequences in Python

Use dictionary keys and list indexes together.

```yaml
projects:
  - name: frontend
    technologies:
      - React
      - TypeScript

  - name: backend
    technologies:
      - Node.js
      - PostgreSQL
```

Python:

```python
print(config["projects"][0]["name"])
# frontend

print(config["projects"][0]["technologies"][0])
# React

print(config["projects"][1]["technologies"][1])
# PostgreSQL
```

# 🗁 Iterating Over Nested Structures in Python

Use nested `for` loops to traverse nested mappings and sequences.

```python
for project in config["projects"]:
    print(f"Project: {project['name']}")

    for technology in project["technologies"]:
        print(f"- {technology}")
```

Using `enumerate()`:

```python
for index, project in enumerate(config["projects"]):
    print(index, project["name"])
```

# 🗁 Modifying Nested Structures in Python

You can update nested dictionaries and lists directly.

```python
config["projects"][0]["name"] = "frontend-platform"

config["projects"][0]["technologies"].append("Vite")

config["projects"].append({
    "name": "mobile",
    "technologies": ["React Native", "TypeScript"]
})
```

Save the updated YAML:

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

A real application may require several levels of configuration.

```yaml
application:
  name: hands-on-notes
  version: 1.0.0

  server:
    host: 0.0.0.0
    port: 3000

  database:
    driver: postgresql

    connection:
      host: localhost
      port: 5432
      database: hands_on_notes

    pool:
      min: 2
      max: 10

  authentication:
    enabled: true

    providers:
      - name: local
        enabled: true

      - name: google
        enabled: false
```

# 🗁 Practical Example: Docker Compose

Docker Compose uses nested mappings and sequences to define services and their configuration.

```yaml
services:
  frontend:
    image: nginx:latest

    ports:
      - "8080:80"

    depends_on:
      - backend

  backend:
    image: node:22

    working_dir: /app

    volumes:
      - ./backend:/app

    environment:
      NODE_ENV: production
      DATABASE_HOST: database

    depends_on:
      - database

  database:
    image: postgres:17

    environment:
      POSTGRES_DB: application
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: change-me

    volumes:
      - database-data:/var/lib/postgresql/data

volumes:
  database-data:
```

This example demonstrates how services, environment variables, dependencies, and volumes are organized through nested mappings and sequences.

# 🗁 Practical Example: GitHub Actions

A GitHub Actions workflow contains nested mappings and sequences for triggers, jobs, steps, and configuration.

```yaml
name: Application CI

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

    strategy:
      matrix:
        node-version:
          - 20
          - 22

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

# 🗁 Practical Example: Kubernetes Configuration

Kubernetes manifests frequently contain deeply nested mappings and sequences.

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: backend-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: backend

  template:
    metadata:
      labels:
        app: backend

    spec:
      containers:
        - name: backend
          image: node:22

          ports:
            - containerPort: 3000

          env:
            - name: NODE_ENV
              value: production

            - name: PORT
              value: "3000"
```

# 🗁 Understanding YAML Nesting vs Programming Language Objects

After parsing, YAML nesting maps naturally to native programming language structures.

| YAML structure | JavaScript | Python |
| --- | --- | --- |
| Mapping | Object | Dictionary |
| Sequence | Array | List |
| Nested mapping | Nested object | Nested dictionary |
| Nested sequence | Nested array | Nested list |
| Sequence of mappings | Array of objects | List of dictionaries |

The parser converts YAML syntax into these native structures. You work with the resulting objects and collections rather than YAML indentation itself.

# 🗁 Best Practices

* Use consistent indentation, preferably two spaces.
* Keep related configuration values grouped together.
* Avoid unnecessarily deep nesting.
* Use descriptive mapping keys.
* Use sequences for ordered collections.
* Use mappings for named properties.
* Use intermediate variables when accessing deeply nested structures in code.
* Handle optional properties safely.
* Avoid duplicating large configuration sections unnecessarily.
* Validate YAML syntax and application-specific configuration.
* Keep sensitive values such as passwords and API keys out of committed configuration files.
* Use environment variables or a secrets-management system for sensitive configuration.

# 🗁 Summary

YAML nesting allows mappings and sequences to be combined into hierarchical data structures.

Key concepts:

* Mappings can contain other mappings.
* Mappings can contain sequences.
* Sequences can contain mappings and other sequences.
* Indentation determines nesting relationships.
* JavaScript represents nested mappings as objects and sequences as arrays.
* Python represents nested mappings as dictionaries and sequences as lists.
* Optional chaining and dictionary `get()` methods help access optional nested values safely.
* Nested structures are widely used in application and infrastructure configuration.
