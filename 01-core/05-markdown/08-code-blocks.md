# Markdown Code Blocks

Markdown provides several ways to display source code and technical commands. Code formatting is essential for programming tutorials, API documentation, installation guides, configuration examples, and technical notes.

There are two primary ways to display code in Markdown:

1. Inline code
2. Fenced code blocks

# 🗁 Inline Code

Inline code displays a short piece of code within a normal paragraph.

Use a single backtick (`` ` ``) before and after the code.

**Syntax**

```markdown
Use `console.log()` to print a message in JavaScript.
```

Output:

Use `console.log()` to print a message in JavaScript.

**More Examples**

```markdown
- Run `npm install` to install dependencies.
- Use `git status` to check repository changes.
- The `main()` function is the application entry point.
- Store the username in the `username` variable.
```

Output:

* Run `npm install` to install dependencies.
* Use `git status` to check repository changes.
* The `main()` function is the application entry point.
* Store the username in the `username` variable.

**When to use inline code:**

* Function names
* Variable names
* File names
* Directory paths
* Commands
* Configuration keys
* Short code expressions

# 🗁 Fenced Code Blocks

Fenced code blocks display multiple lines of code in a separate block.

Use three backticks before and after the code.

**Syntax**

````markdown
```
function greet() {
    console.log("Hello, World!");
}
```
````

Output:

```
function greet() {
    console.log("Hello, World!");
}
```

Unlike inline code, fenced code blocks preserve line breaks and indentation.

# 🗁 Syntax Highlighting

Syntax highlighting applies language-specific colors to code.

To enable it, specify the programming language immediately after the opening backticks.

### 🖆 JavaScript

````markdown
```javascript
function greet(name) {
    console.log(`Hello, ${name}!`);
}

greet("Rahim");
```
````

### 🖆 Python

````markdown
```python
def greet(name):
    print(f"Hello, {name}!")

greet("Rahim")
```
````

### 🖆 PHP

````markdown
```php
<?php

function greet($name) {
    echo "Hello, " . $name;
}

greet("Rahim");

?>
```
````

### 🖆 HTML

````markdown
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```
````

### 🖆 CSS

````markdown
```css
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background-color: #f5f5f5;
}

h1 {
    color: blue;
}
```
````

### 🖆 SQL

````markdown
```sql
SELECT
    employee_name,
    department,
    salary
FROM employees
WHERE salary > 30000;
```
````

### 🖆 Bash

````markdown
```bash
#!/bin/bash

echo "Hello, World!"
mkdir projects
cd projects
```
````

### 🖆 JSON

````markdown
```json
{
    "name": "Rahim",
    "role": "Developer",
    "active": true
}
```
````

**Important:** Language identifiers are commonly case-insensitive in Markdown renderers, but lowercase identifiers are a good convention. Highlighting depends on the renderer's supported languages and highlighting engine.

# 🗁 Indented Code Blocks

Markdown also supports code blocks using indentation.

Place four spaces or one tab before each code line.

**Syntax**

```markdown
    function greet() {
        console.log("Hello, World!");
    }
```

Output:

    function greet() {
        console.log("Hello, World!");
    }

**Limitations:**

* Language-specific syntax highlighting is generally unavailable.
* Indentation can be inconvenient for large code examples.
* Blank lines and surrounding Markdown content can affect formatting.

For most developer documentation, fenced code blocks are preferable.

# 🗁 Choosing a Fence Length

Three backticks are the standard choice, but you can use more backticks when your code contains triple backticks.

**Example**

Suppose you want to show a Markdown code block containing another fenced code block.

Use four backticks for the outer fence:

````markdown
````markdown
```javascript
console.log("Hello");
```
````

````

This allows the inner triple-backtick fence to remain part of the displayed example.

You can also use tildes (`~~~`) as an alternative fence.

### 🖆 Example

````markdown
~~~python
print("Hello, World!")
~~~
````

Both backticks and tildes are supported by common Markdown implementations.

# 🗁 Displaying Commands

Code blocks are useful for documenting terminal commands.

### 🖆 Linux Commands

````markdown
```bash
pwd
ls -la
mkdir my-project
cd my-project
```
````

### 🖆 Git Commands

````markdown
```bash
git init
git add .
git commit -m "Initial commit"
git status
```
````

### 🖆 NPM Commands

````markdown
```bash
npm install
npm run dev
npm run build
```
````

### 🖆 Output

```bash
npm install
npm run dev
npm run build
```

Use the appropriate language identifier when possible. For shell commands, `bash` is common, while `shell` or `sh` may be more suitable for commands that are not Bash-specific.

# 🗁 Displaying Configuration Files

Code blocks are frequently used for configuration examples.

### 🖆 `.env`

````markdown
```dotenv
APP_NAME=MyApplication
APP_ENV=development
APP_DEBUG=true
APP_URL=http://localhost:8000
```
````

### 🖆 `package.json`

````markdown
```json
{
    "name": "my-project",
    "version": "1.0.0",
    "scripts": {
        "dev": "vite",
        "build": "vite build"
    }
}
```
````

### 🖆 `nginx.conf`

````markdown
```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        root /var/www/html;
        index index.html;
    }
}
```
````

**Note:** Syntax highlighting for configuration languages varies between Markdown renderers.

# 🗁 Displaying File Paths and Directory Structures

Use inline code for short paths and fenced code blocks for directory structures.

### 🖆 Single File Path

```markdown
The configuration file is located at `config/database.php`.
```

### 🖆 Directory Structure

````markdown
```text
my-project/
├── src/
│   ├── index.js
│   └── app.js
├── public/
│   └── index.html
├── package.json
└── README.md
```
````

Output:

```text
my-project/
├── src/
│   ├── index.js
│   └── app.js
├── public/
│   └── index.html
├── package.json
└── README.md
```

Use `text` when you want to display a directory tree without programming-language syntax highlighting.

# 🗁 Escaping Backticks

Sometimes you need to display a literal backtick inside inline code.

You can use multiple backticks as delimiters.

**Example**

```markdown
``Use `console.log()` to print output.``
```

Output:

``Use `console.log()` to print output.``

The number of opening and closing backticks must match.

If the code content begins or ends with a backtick or contains delimiter-like sequences, use an appropriate longer delimiter and, when necessary, surrounding spaces.

# 🗁 Code Blocks Inside Lists

You can include code blocks inside list items by indenting the fenced block relative to the list item.

**Example**

````markdown
1. Create a project directory.

   ```bash
   mkdir my-project
   cd my-project
   ```

2. Initialize the project.

   ```bash
   npm init -y
   ```

3. Install dependencies.

   ```bash
   npm install
   ```
````

Output:

1. Create a project directory.

   ```bash
   mkdir my-project
   cd my-project
   ```

2. Initialize the project.

   ```bash
   npm init -y
   ```

3. Install dependencies.

   ```bash
   npm install
   ```

Correct indentation is important for keeping code blocks associated with their list items.

# 🗁 Code Blocks Inside Tables

Short code examples can be placed inside Markdown table cells using inline code.

**Example**

```markdown
| Command | Purpose |
|---------|---------|
| `npm install` | Install dependencies |
| `npm run dev` | Start development server |
| `npm run build` | Build application |
```

Output:

| Command | Purpose |
| --------- | --------- |
| `npm install` | Install dependencies |
| `npm run dev` | Start development server |
| `npm run build` | Build application |

Avoid placing large fenced code blocks inside table cells. Standard Markdown tables do not support arbitrary block-level content reliably.

# 🗁 GitHub Code Features

GitHub supports several useful features around code blocks.

### 🖆 Syntax Highlighting

GitHub uses language identifiers to highlight source code.

```javascript
const message = "Hello GitHub!";
console.log(message);
```

### 🖆 Code Line References

GitHub allows users to link to specific lines in a source file.

Example URL structure:

```text
https://github.com/username/repository/blob/main/src/app.js#L10
```

You can also link to a range of lines:

```text
https://github.com/username/repository/blob/main/src/app.js#L10-L20
```

Replace the example username, repository, branch, and file path with actual values.

### 🖆 Diff Code Blocks

Use `diff` to demonstrate code changes.

````markdown
```diff
- const port = 3000;
+ const port = 8000;
```
````

Output:

```diff
- const port = 3000;
+ const port = 8000;
```

The `-` and `+` prefixes represent removed and added lines in a diff-style example.

**Note:** Diff highlighting is supported by GitHub and many other renderers, but exact colors and presentation can vary.

# 🖆 Common Mistakes

| Mistake                             | Problem                                       | Solution                         |
| ----------------------------------- | --------------------------------------------- | -------------------------------- |
| Missing closing fence               | Following content may become code             | Add a matching closing fence     |
| Incorrect language identifier       | Syntax highlighting may fail                  | Use a supported identifier       |
| Incorrect indentation               | Code formatting changes                       | Preserve original indentation    |
| Using inline code for long examples | Poor readability                              | Use fenced code blocks           |
| Using too few outer backticks       | Nested examples may close prematurely         | Use a longer outer fence         |
| Missing blank lines around fences   | Some renderers may parse content unexpectedly | Separate blocks with blank lines |
| Using unsupported language names    | No syntax highlighting                        | Check renderer language support  |

# 🖆 Best Practices

1. Use inline code for short technical references.
2. Use fenced code blocks for multiple lines.
3. Specify a language identifier for syntax highlighting.
4. Preserve the original code indentation.
5. Use `text` for directory trees and plain-text examples.
6. Use `bash` for Bash-specific commands.
7. Keep code examples focused and executable where practical.
8. Add explanations before or after complex examples.
9. Use longer fences for nested code examples.
10. Avoid unnecessary code inside tables.
11. Do not expose real passwords, API keys, tokens, or other secrets in documentation.
12. Test examples before publishing technical documentation.
