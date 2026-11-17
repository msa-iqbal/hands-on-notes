# Prettier

Prettier is an opinionated code formatter that automatically formats source code into a consistent style.

It supports JavaScript, TypeScript, JSX, TSX, JSON, CSS, HTML, Markdown, YAML, and several other languages.

Prettier focuses on code formatting rather than identifying programming errors or enforcing application logic.

# 🗁 What Is Prettier?

Different developers often write code using different formatting styles.

For example, these JavaScript snippets behave identically but use different formatting:

**Without consistent formatting:**

```javascript
const user={name:"Alice",age:25};
function greet(name){return "Hello "+name;}
```

**With Prettier:**

```javascript
const user = { name: "Alice", age: 25 };

function greet(name) {
  return "Hello " + name;
}
```

Prettier automatically formats code according to a consistent set of rules.

A typical workflow:

```text
Source Code
     |
     v
  Prettier
     |
     v
Code Formatting
     |
     v
Formatted Source Code
```

# 🗁 Installing Prettier

Install Prettier as a development dependency:

```bash
npm install --save-dev prettier
```

Verify the installation:

```bash
npx prettier --version
```

# 🗁 Formatting Files

Format a JavaScript file:

```bash
npx prettier --write src/index.js
```

Format a TypeScript file:

```bash
npx prettier --write src/index.ts
```

Format all supported files in a directory:

```bash
npx prettier --write .
```

The `--write` option modifies files directly and saves the formatted result.

# 🗁 Checking Formatting Without Modifying Files

Sometimes you want to check whether files follow the configured formatting rules without changing them.

Use:

```bash
npx prettier --check .
```

Prettier reports files that are not correctly formatted.

This is particularly useful in CI/CD pipelines.

# 🗁 Prettier Configuration

Prettier supports configuration through files such as:

* `.prettierrc`
* `.prettierrc.json`
* `.prettierrc.yaml`
* `.prettierrc.yml`
* `.prettierrc.js`
* `.prettierrc.mjs`
* `prettier.config.js`
* `prettier.config.mjs`

Example:

**.prettierrc.json**

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "useTabs": false,
  "trailingComma": "all",
  "printWidth": 80,
  "bracketSpacing": true,
  "arrowParens": "always"
}
```

# 🗁 Understanding Prettier Options

| Option | Description | Example |
| --- | --- | --- |
| `semi` | Adds semicolons | `true` |
| `singleQuote` | Uses single quotes where applicable | `true` |
| `tabWidth` | Number of spaces per indentation level | `2` |
| `useTabs` | Uses tabs instead of spaces | `false` |
| `trailingComma` | Controls trailing commas | `"all"` |
| `printWidth` | Preferred maximum line width | `80` |
| `bracketSpacing` | Adds spaces inside object brackets | `true` |
| `arrowParens` | Controls parentheses around arrow parameters | `"always"` |
| `endOfLine` | Controls line endings | `"lf"` |

Prettier uses `printWidth` as a formatting preference rather than a strict maximum line length.

# 🗁 Formatting Multiple File Types

Prettier supports many common development file formats.

**JavaScript**

```javascript
const user = {
  name: "Alice",
  age: 25,
};
```

**TypeScript**

```typescript
interface User {
  id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice",
};
```

**JSON**

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "private": true
}
```

**CSS**

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

**Markdown**

```markdown
# TypeScript

TypeScript adds static typing to JavaScript.

- Type safety
- Interfaces
- Generics
```

# 🗁 Ignoring Files

Some files should not be formatted automatically.

Create a `.prettierignore` file:

```text
node_modules/
dist/
build/
coverage/
.next/
*.min.js
package-lock.json
```

Prettier ignores matching files and directories when processing supported input paths.

You can also ignore specific code sections using comments.

**JavaScript:**

```javascript
// prettier-ignore
const data = {    name:    "Alice",age:25 };
```

**TypeScript:**

```typescript
// prettier-ignore
const user: User = { name: "Alice", age: 25 };
```

These comments tell Prettier to leave the next relevant AST node unchanged.

# 🗁 Using Prettier with npm Scripts

Define reusable formatting commands in `package.json`:

```json
{
  "scripts": {
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  },
  "devDependencies": {
    "prettier": "^3.0.0"
  }
}
```

Format the project:

```bash
npm run format
```

Check formatting:

```bash
npm run format:check
```

The version shown is illustrative.

# 🗁 Using Prettier with ESLint

ESLint and Prettier serve different purposes.

* ESLint identifies problematic code patterns and enforces code-quality rules.
* Prettier automatically formats source code.

Both can be used together.

Install the integration package:

```bash
npm install --save-dev eslint-config-prettier
```

Add it to your ESLint flat configuration:

**eslint.config.js**

```javascript
import { defineConfig } from "eslint/config";
import eslintConfigPrettier from "eslint-config-prettier";

export default defineConfig([
  {
    files: ["**/*.{js,jsx,ts,tsx}"],
    rules: {
      "no-unused-vars": "error",
    },
  },
  eslintConfigPrettier,
]);
```

`eslint-config-prettier` disables ESLint rules that conflict with Prettier's formatting decisions.

It does not run Prettier automatically.

You can run both tools separately:

```bash
npx eslint .
npx prettier --write .
```

# 🗁 Using Prettier with TypeScript

Prettier supports TypeScript syntax directly.

You do not need a separate TypeScript formatter plugin for ordinary `.ts` and `.tsx` files.

Example:

**Before formatting:**

```typescript
interface User{name:string;age:number}
const getUser=(name:string):User=>({name,age:25})
```

**After formatting:**

```typescript
interface User {
  name: string;
  age: number;
}

const getUser = (name: string): User => ({ name, age: 25 });
```

Run:

```bash
npx prettier --write "src/**/*.{ts,tsx}"
```

# 🗁 Using Prettier in VS Code

The Prettier extension for Visual Studio Code provides editor integration.

It can:

* Format files directly in the editor.
* Format code automatically on save.
* Apply project-specific formatting rules.
* Format selected code.
* Support multiple programming languages.

Install the Prettier extension from the VS Code Extensions Marketplace.

Configure the project or editor:

**.vscode/settings.json**

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true
}
```

You can also specify language-specific formatters:

```json
{
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

When working in a team, keep the project's Prettier configuration in version control so that everyone follows the same formatting rules.

# 🗁 Prettier in CI/CD Pipelines

Prettier can verify formatting consistency during continuous integration.

Example GitHub Actions workflow:

**.github/workflows/format.yml**

```yaml
name: Format Check

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  format:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Check formatting
        run: npm run format:check
```

If formatting violations are detected, the workflow fails.

This helps prevent inconsistently formatted code from entering the main branch.

# 🗁 Common Prettier Options

| Command | Purpose |
| --- | --- |
| `prettier --write .` | Format supported files |
| `prettier --check .` | Check formatting |
| `prettier --list-different .` | List files with formatting differences |
| `prettier --find-config-path src/index.ts` | Find the configuration file for a path |
| `prettier --config .prettierrc.json --write src/` | Use a specific configuration |
| `prettier --ignore-path .prettierignore --write .` | Specify an ignore file |

You can inspect available options using:

```bash
npx prettier --help
```

# 🗁 Common Problems

**Prettier does not format a file**

Possible causes:

* The file extension is unsupported.
* The file is excluded by `.prettierignore`.
* The file contains syntax errors.
* The editor is using another formatter.

Check the file extension, ignore rules, and editor configuration.

**Different formatting between developers**

Possible causes:

* Different Prettier versions.
* Missing shared configuration.
* Different editor formatter settings.
* Uncommitted configuration files.

Use a local project dependency and commit the shared configuration.

**ESLint and Prettier conflicts**

Some ESLint formatting rules may conflict with Prettier.

Install and configure:

```bash
npm install --save-dev eslint-config-prettier
```

**Formatting changes unexpectedly**

Check the project's Prettier configuration and ensure that the editor is using the project's local Prettier installation.

# 🗁 Best Practices

* Install Prettier locally as a development dependency.
* Commit the Prettier configuration to version control.
* Use `.prettierignore` for generated and unnecessary files.
* Enable format-on-save in your editor.
* Use `prettier --check` in CI/CD pipelines.
* Use ESLint for code-quality analysis and Prettier for formatting.
* Keep the Prettier version consistent across the development team.
* Avoid unnecessary formatting overrides.
* Run formatting checks before merging pull requests.

# 🗁 Summary

Prettier is an opinionated code formatter that automatically maintains consistent source-code formatting.

Its main capabilities include:

* Automatic code formatting.
* Support for JavaScript and TypeScript.
* Formatting JSX, TSX, JSON, CSS, Markdown, and YAML.
* Configurable formatting rules.
* Integration with VS Code.
* Integration with ESLint.
* Formatting checks in CI/CD pipelines.

Prettier helps development teams maintain a consistent code style without manually enforcing formatting conventions.
