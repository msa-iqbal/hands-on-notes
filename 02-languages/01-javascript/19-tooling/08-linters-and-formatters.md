# Linters and Formatters

Linters and formatters are development tools that help maintain code quality, consistency, readability, and maintainability in JavaScript projects.

A **linter** analyzes source code and identifies potential problems, suspicious patterns, and violations of coding rules.

A **formatter** automatically restructures source code to follow a consistent formatting style.

Together, they help development teams maintain clean, consistent, and predictable codebases.

# 🗁 What Is a Linter?

A linter is a static analysis tool that examines source code without executing the application.

It identifies issues such as:

* Undefined variables.
* Unused variables.
* Unreachable code.
* Potential programming mistakes.
* Inconsistent coding practices.
* Suspicious expressions.
* Violations of configured coding rules.

Example:

```javascript
const username = "Alice";

console.log(userName);
```

The variable `username` is declared, but `userName` is referenced instead.

A linter can identify this undefined variable before the application executes.

# 🗁 What Is a Code Formatter?

A code formatter automatically changes the layout and appearance of source code according to predefined formatting rules.

Example before formatting:

```javascript
function greet(name){console.log("Hello, "+name)}
```

After formatting:

```javascript
function greet(name) {
  console.log("Hello, " + name);
}
```

A formatter can standardize:

* Indentation.
* Spacing.
* Line breaks.
* Quotes.
* Semicolons.
* Trailing commas.
* Object and array formatting.
* Function and expression layout.

The primary purpose is to make code consistently readable.

# 🗁 Linter vs Formatter

Although both tools improve code quality, they solve different problems.

| Linter | Formatter |
| --- | --- |
| Analyzes code for potential problems | Standardizes code appearance |
| Identifies suspicious patterns | Adjusts whitespace and layout |
| Enforces coding rules | Enforces formatting rules |
| Can detect unused variables | Formats variable declarations |
| May suggest or apply fixes | Automatically rewrites formatting |
| Example: ESLint | Examples: Prettier, dprint |

A linter answers:

> Is this code potentially incorrect or inconsistent with our rules?

A formatter answers:

> Is this code formatted consistently?

# 🗁 Popular JavaScript Linters

**ESLint**

[ESLint](https://eslint.org/) is one of the most widely used linting tools in the JavaScript ecosystem.

Key features:

* Detects potential programming errors.
* Identifies unused variables.
* Enforces coding conventions.
* Supports custom rules.
* Provides automatic fixes for supported issues.
* Supports plugins and shared configurations.
* Integrates with editors and build workflows.

**JSHint**

[JSHint](https://jshint.com/) identifies potential problems and suspicious patterns in JavaScript code.

It is a simpler alternative for projects that need basic JavaScript code analysis.

**Biome**

[Biome](https://biomejs.dev/) provides both linting and formatting capabilities.

Key features:

* JavaScript and TypeScript linting.
* Automatic formatting.
* Fast code analysis.
* JSON support.
* Editor integration.
* Unified configuration.

# 🗁 Popular JavaScript Formatters

**Prettier**

[Prettier](https://prettier.io/) is an opinionated code formatter that automatically formats source code according to a consistent set of rules.

Supported formats include:

* JavaScript.
* TypeScript.
* JSX and TSX.
* HTML.
* CSS.
* JSON.
* Markdown.
* YAML.

**dprint**

[dprint](https://dprint.dev/) is an extensible and high-performance code formatter that uses language-specific plugins.

It supports multiple programming and configuration languages.

# 🗁 Installing ESLint

ESLint can be installed as a development dependency.

```bash
npm install --save-dev eslint
```

For a modern project, initialize the configuration:

```bash
npx eslint --init
```

The initialization process helps configure ESLint according to the project requirements.

Modern ESLint uses the flat configuration format by default.

Create an `eslint.config.js` file:

```javascript
import { defineConfig } from "eslint/config";

export default defineConfig([
  {
    files: ["**/*.js"],
    rules: {
      "no-unused-vars": "warn",
      "no-undef": "error",
      "no-console": "off"
    }
  }
]);
```

This configuration:

* Warns about unused variables.
* Reports undefined variables as errors.
* Allows `console` statements.

Run ESLint:

```bash
npx eslint .
```

Check a specific file:

```bash
npx eslint src/index.js
```

# 🗁 ESLint Rule Severity

ESLint rules support three severity levels.

| Value | Meaning |
| --- | --- |
| `"off"` or `0` | Disable the rule |
| `"warn"` or `1` | Report a warning |
| `"error"` or `2` | Report an error |

Example:

```javascript
import { defineConfig } from "eslint/config";

export default defineConfig([
  {
    rules: {
      "no-unused-vars": "warn",
      "no-undef": "error",
      "eqeqeq": "error"
    }
  }
]);
```

The `eqeqeq` rule requires strict equality operators in applicable comparisons.

Example:

```javascript
// Incorrect according to eqeqeq
if (value == 10) {
  console.log("Matched");
}

// Preferred
if (value === 10) {
  console.log("Matched");
}
```

# 🗁 ESLint Automatic Fixing

ESLint can automatically fix problems when a rule provides a safe or supported fix.

Run:

```bash
npx eslint . --fix
```

For example, ESLint can fix certain:

* Spacing issues.
* Quote style violations.
* Unnecessary syntax.
* Consistently fixable coding patterns.

Not every linting issue can be fixed automatically.

Some problems require developer judgment.

# 🗁 ESLint Plugins

ESLint plugins extend linting capabilities with additional rules.

Common plugins include:

| Plugin | Purpose |
| --- | --- |
| `@typescript-eslint` | TypeScript-specific linting |
| `eslint-plugin-react` | React-related linting |
| `eslint-plugin-react-hooks` | React Hooks rules |
| `eslint-plugin-import` | Import and module analysis |
| `eslint-plugin-jsx-a11y` | JSX accessibility checks |
| `eslint-plugin-n` | Node.js-specific linting |

Install a plugin:

```bash
npm install --save-dev eslint-plugin-react
```

Plugins are configured through ESLint's flat configuration system or compatible shared configurations.

# 🗁 Installing Prettier

Install Prettier as a development dependency:

```bash
npm install --save-dev prettier
```

Format all supported files in the project:

```bash
npx prettier --write .
```

Check formatting without modifying files:

```bash
npx prettier --check .
```

Format a specific file:

```bash
npx prettier --write src/index.js
```

Check a specific file:

```bash
npx prettier --check src/index.js
```

# 🗁 Prettier Configuration

Prettier can be configured through a `.prettierrc` file.

Example:

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "useTabs": false,
  "trailingComma": "all",
  "printWidth": 100,
  "bracketSpacing": true
}
```

Common options:

| Option | Purpose |
| --- | --- |
| `semi` | Controls semicolon usage |
| `singleQuote` | Controls single versus double quotes |
| `tabWidth` | Number of spaces per indentation level |
| `useTabs` | Uses tabs instead of spaces |
| `trailingComma` | Controls trailing comma behavior |
| `printWidth` | Preferred line width |
| `bracketSpacing` | Controls spacing inside object literals |

For example:

```javascript
const user = { name: "Alice", age: 25 };
```

With `singleQuote: true` and `bracketSpacing: true`, the formatted output becomes:

```javascript
const user = { name: 'Alice', age: 25 };
```

# 🗁 Ignoring Files

Not every file should be linted or formatted.

Both ESLint and Prettier support ignore patterns.

**ESLint**

Older ESLint configurations commonly used `.eslintignore`.

Modern ESLint flat configuration supports ignore patterns through configuration objects.

Example `eslint.config.js`:

```javascript
import { defineConfig } from "eslint/config";

export default defineConfig([
  {
    ignores: [
      "node_modules/",
      "dist/",
      "coverage/"
    ]
  },
  {
    files: ["**/*.js"],
    rules: {
      "no-unused-vars": "warn"
    }
  }
]);
```

**Prettier**

Create a `.prettierignore` file:

```gitignore
node_modules/
dist/
coverage/
package-lock.json
```

Ignored files are excluded from normal Prettier formatting operations.

# 🗁 ESLint and Prettier Integration

ESLint and Prettier can work together.

ESLint focuses on code quality and potential programming errors.

Prettier focuses on formatting.

However, some ESLint rules may conflict with Prettier's formatting decisions.

The `eslint-config-prettier` package disables ESLint rules that conflict with Prettier formatting.

Install:

```bash
npm install --save-dev eslint-config-prettier
```

Add it at the end of the ESLint configuration:

```javascript
import { defineConfig } from "eslint/config";
import prettier from "eslint-config-prettier";

export default defineConfig([
  {
    files: ["**/*.js"],
    rules: {
      "no-unused-vars": "warn",
      "no-undef": "error"
    }
  },
  prettier
]);
```

The Prettier compatibility configuration should come after configurations whose formatting rules it needs to disable.

**Formatting through ESLint**

If you want ESLint to execute Prettier as part of linting, you can use `eslint-plugin-prettier`.

Install:

```bash
npm install --save-dev eslint-plugin-prettier
```

This integration reports formatting differences as ESLint issues.

It is optional. Many projects keep formatting and linting as separate commands for a simpler workflow.

# 🗁 TypeScript Linting

TypeScript projects need additional linting support.

The `typescript-eslint` project provides tools for integrating TypeScript with ESLint.

Install:

```bash
npm install --save-dev eslint @eslint/js typescript typescript-eslint
```

Example `eslint.config.js`:

```javascript
import js from "@eslint/js";
import tseslint from "typescript-eslint";
import { defineConfig } from "eslint/config";

export default defineConfig(
  {
    ignores: ["dist/", "node_modules/"]
  },
  {
    files: ["**/*.js"],
    extends: [js.configs.recommended]
  },
  {
    files: ["**/*.ts"],
    extends: [
      js.configs.recommended,
      ...tseslint.configs.recommended
    ]
  }
);
```

Run:

```bash
npx eslint .
```

For rules that require TypeScript type information, additional typed-linting configuration is necessary.

Documentation: <https://typescript-eslint.io/>

# 🗁 Editor Integration

Linters and formatters can integrate directly with code editors.

For Visual Studio Code, common extensions include:

* ESLint.
* Prettier - Code formatter.

Useful editor settings:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  }
}
```

This configuration:

* Formats files when saving.
* Uses Prettier as the default formatter.
* Allows ESLint automatic fixes through an explicit save action.

Project-level configuration helps ensure that team members use consistent tooling.

# 🗁 npm Scripts Integration

You can define linting and formatting commands in `package.json`.

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  }
}
```

Execute the scripts:

```bash
npm run lint
npm run lint:fix
npm run format
npm run format:check
```

This makes common code-quality operations easy to execute and integrate into automated workflows.

# 🗁 Git Hooks Integration

Git hooks can run linting and formatting checks before code is committed.

A common tool for this workflow is Husky.

Install:

```bash
npm install --save-dev husky
```

Initialize:

```bash
npx husky init
```

Modern Husky versions allow you to configure hooks by editing files inside the `.husky/` directory.

Example `.husky/pre-commit`:

```sh
npm run lint
```

For larger projects, `lint-staged` can run checks only on staged files.

Install:

```bash
npm install --save-dev lint-staged
```

Configure `package.json`:

```json
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ]
  }
}
```

This avoids unnecessarily processing the entire project during every commit.

# 🗁 Continuous Integration

Linting and formatting checks can run automatically in CI pipelines.

Example GitHub Actions workflow:

```yaml
name: Code Quality

on:
  push:
  pull_request:

jobs:
  quality:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint
        run: npm run lint

      - name: Check formatting
        run: npm run format:check
```

This workflow helps prevent code-quality problems from entering the main branch.

# 🗁 Biome: Linter and Formatter in One Tool

Biome provides linting and formatting in a single toolchain.

Install:

```bash
npm install --save-dev --save-exact @biomejs/biome
```

Initialize:

```bash
npx biome init
```

This generates a `biome.json` configuration file.

Example configuration:

```json
{
  "$schema": "https://biomejs.dev/schemas/2.0.0/schema.json",
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true
    }
  }
}
```

Run linting:

```bash
npx biome lint .
```

Run formatting:

```bash
npx biome format --write .
```

Run both:

```bash
npx biome check .
```

Biome can be an alternative to maintaining separate linting and formatting tools, although its rule coverage and integrations differ from ESLint and Prettier.

# 🗁 Practical Project Configuration

A typical JavaScript project might use the following structure:

```text
my-project/
├── src/
│   ├── index.js
│   └── utils.js
├── .prettierignore
├── .prettierrc.json
├── eslint.config.js
├── package.json
└── package-lock.json
```

Example `package.json`:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "node --watch src/index.js",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "test": "node --test"
  },
  "devDependencies": {
    "@eslint/js": "^9.0.0",
    "eslint": "^9.0.0",
    "eslint-config-prettier": "^10.0.0",
    "prettier": "^3.0.0"
  }
}
```

Install dependencies:

```bash
npm install
```

Run code-quality checks:

```bash
npm run lint
npm run format:check
```

Automatically fix supported issues:

```bash
npm run lint:fix
npm run format
```

# 🗁 Best Practices

* Use a linter to detect potential bugs and enforce code-quality rules.
* Use a formatter to maintain consistent code style.
* Keep linting and formatting responsibilities clearly separated.
* Store configurations in version control.
* Use shared project configurations across teams.
* Avoid excessive custom linting rules.
* Configure ignore patterns for generated files and dependencies.
* Integrate checks into CI pipelines.
* Use editor integration for immediate feedback.
* Run automatic fixes before committing code.
* Prefer linting staged files in large repositories.
* Do not assume that formatting guarantees correctness.
* Review linting errors rather than automatically suppressing them.

# 🗁 Key Takeaways

* Linters analyze source code for potential problems and rule violations.
* Formatters automatically standardize source-code appearance.
* ESLint is a widely used JavaScript linting tool.
* Prettier is a widely used code formatter.
* Biome combines linting and formatting capabilities.
* ESLint rules support off, warning, and error severity levels.
* Plugins and presets extend linting capabilities.
* ESLint and Prettier can work together through compatibility configuration.
* Editor integration improves the development workflow.
* Git hooks and CI pipelines help enforce consistent code quality.
