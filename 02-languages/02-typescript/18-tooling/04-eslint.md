# ESLint

ESLint is a static code analysis tool for JavaScript and TypeScript projects. It identifies potential bugs, enforces coding conventions, detects problematic patterns, and helps maintain consistent code quality.

It analyzes source code without executing the application.

ESLint is commonly used in modern frontend and backend projects to maintain code quality across teams and development environments.

# 🗁 What Is ESLint?

ESLint analyzes source code and reports problems based on a set of configurable rules.

For example:

```javascript
const userName = "Alice";

console.log(userName);
```

Depending on the configured rules, ESLint can identify issues such as:

* Unused variables.
* Undefined variables.
* Unnecessary expressions.
* Incorrect equality comparisons.
* Unreachable code.
* Potentially unsafe patterns.
* Inconsistent coding conventions.

A typical ESLint workflow:

```text
Source Code
     |
     v
  ESLint
     |
     v
Code Analysis
     |
     v
Configured Rules
     |
     v
Problems and Suggestions
     |
     v
Developer Fixes
```

# 🗁 Installing ESLint

Install ESLint as a development dependency:

```bash
npm install --save-dev eslint
```

Initialize ESLint configuration:

```bash
npx eslint --init
```

The initialization process asks questions about your project and generates a configuration based on your selections.

For modern ESLint projects, you can also create the configuration manually.

# 🗁 ESLint Configuration

ESLint supports the flat configuration system through an `eslint.config.js` file.

Example:

**eslint.config.js**

```javascript
import { defineConfig } from "eslint/config";

export default defineConfig([
  {
    files: ["**/*.js"],
    rules: {
      "no-unused-vars": "error",
      "no-console": "warn",
      eqeqeq: "error"
    }
  }
]);
```

This configuration:

* Applies the rules to JavaScript files.
* Reports unused variables as errors.
* Reports console statements as warnings.
* Requires strict equality comparisons.

The flat configuration system is the default configuration format in modern ESLint versions.

# 🗁 Understanding ESLint Rules

Rules determine which code patterns ESLint should detect and report.

Each rule can have one of three severity levels:

| Severity | Value | Description |
| --- | --- | --- |
| Off | `0` | Disables the rule |
| Warn | `1` | Reports a warning |
| Error | `2` | Reports an error |

Example:

```javascript
export default [
  {
    rules: {
      "no-unused-vars": "error",
      "no-console": "warn",
      eqeqeq: "off"
    }
  }
];
```

You can also provide rule-specific options:

```javascript
export default [
  {
    rules: {
      quotes: ["error", "single"],
      semi: ["error", "always"]
    }
  }
];
```

Rule availability depends on the ESLint version and installed plugins. Some formatting-related core rules are deprecated in modern ESLint versions, so dedicated formatting tools such as Prettier are often preferred.

# 🗁 Running ESLint

Analyze all JavaScript files:

```bash
npx eslint .
```

Analyze a specific file:

```bash
npx eslint src/index.js
```

Analyze a directory:

```bash
npx eslint src/
```

ESLint reports violations with their file locations, rule names, and severity levels.

# 🗁 Automatically Fixing Problems

Some ESLint rules support automatic fixes.

Run:

```bash
npx eslint . --fix
```

ESLint automatically fixes supported problems.

For example, a rule may automatically correct quote styles or remove unnecessary syntax.

Not every issue can be fixed automatically. Problems requiring changes in program logic or developer judgment must be handled manually.

# 🗁 Ignoring Files and Directories

Some files should not be analyzed by ESLint.

Common examples include:

* Build output.
* Dependencies.
* Generated files.
* Coverage reports.
* Temporary files.

You can exclude files using `ignores` in the flat configuration.

**eslint.config.js**

```javascript
export default [
  {
    ignores: [
      "node_modules/",
      "dist/",
      "coverage/",
      "build/"
    ]
  },
  {
    files: ["**/*.js"],
    rules: {
      "no-unused-vars": "error"
    }
  }
];
```

Ignored paths are generally resolved relative to the configuration file, subject to ESLint's configuration behavior.

# 🗁 Using ESLint with TypeScript

ESLint can analyze TypeScript through the TypeScript ESLint project.

Install the required packages:

```bash
npm install --save-dev eslint typescript typescript-eslint
```

Create or update:

**eslint.config.js**

```javascript
import { defineConfig } from "eslint/config";
import tseslint from "typescript-eslint";

export default defineConfig([
  {
    files: ["**/*.ts"],
    extends: [
      tseslint.configs.recommended
    ]
  }
]);
```

This enables TypeScript-aware parsing and recommended TypeScript linting rules.

Example:

**src/index.ts**

```typescript
const userName: string = "Alice";

console.log(userName);
```

Run ESLint:

```bash
npx eslint src/
```

# 🗁 Type-Aware Linting

Basic TypeScript linting analyzes syntax and TypeScript-specific patterns.

Type-aware linting can additionally use TypeScript's type information to detect more complex problems.

For example, it can identify unsafe operations involving values typed as `any`.

A type-aware configuration can use `typescript-eslint`'s recommended type-checked rules.

```javascript
import { defineConfig } from "eslint/config";
import tseslint from "typescript-eslint";

export default defineConfig(
  tseslint.configs.recommendedTypeChecked,
  {
    languageOptions: {
      parserOptions: {
        projectService: true
      }
    }
  }
);
```

Type-aware linting requires additional TypeScript analysis and can increase linting time.

It is particularly useful in larger TypeScript applications where stronger static analysis is valuable.

# 🗁 ESLint with React

React projects often use ESLint plugins to detect React-specific problems.

Install:

```bash
npm install --save-dev eslint-plugin-react eslint-plugin-react-hooks
```

Example configuration:

```javascript
import { defineConfig } from "eslint/config";
import react from "eslint-plugin-react";
import reactHooks from "eslint-plugin-react-hooks";

export default defineConfig([
  {
    files: ["**/*.{js,jsx}"],
    plugins: {
      react,
      "react-hooks": reactHooks
    },
    settings: {
      react: {
        version: "detect"
      }
    },
    rules: {
      ...react.configs.recommended.rules,
      ...reactHooks.configs.recommended.rules
    }
  }
]);
```

These plugins help identify React-specific issues and enforce recommended Hooks practices.

# 🗁 ESLint with npm Scripts

You can create reusable linting commands in `package.json`.

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  },
  "devDependencies": {
    "eslint": "^9.0.0"
  }
}
```

Run:

```bash
npm run lint
```

Automatically fix supported issues:

```bash
npm run lint:fix
```

For TypeScript projects, configure the TypeScript ESLint packages and rules in addition to ESLint itself.

# 🗁 ESLint and Prettier

ESLint and Prettier are complementary tools.

ESLint primarily focuses on code quality, correctness, and potentially problematic patterns.

Prettier focuses on consistent code formatting.

| Feature | ESLint | Prettier |
| --- | --- | --- |
| Code quality analysis | Yes | No |
| Detect potential bugs | Yes | No |
| Enforce coding rules | Yes | Limited to formatting |
| Format code | Some rules can | Yes |
| Fix supported issues | Yes | Yes |
| TypeScript support | Through TypeScript ESLint | Yes |
| Custom linting rules | Yes | No |

A common workflow uses both tools:

```bash
npm run lint
npm run format
```

Prettier can also be integrated with ESLint using `eslint-config-prettier` to disable conflicting formatting rules.

# 🗁 ESLint in VS Code

The ESLint extension for Visual Studio Code provides editor integration.

It can:

* Display linting problems directly in the editor.
* Highlight rule violations.
* Show diagnostic messages.
* Apply supported fixes.
* Run ESLint automatically while editing.

Install the ESLint extension from the VS Code Extensions Marketplace.

For automatic fixes on save, configure:

**.vscode/settings.json**

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  }
}
```

This allows ESLint fixes to run when you explicitly save the file.

# 🗁 ESLint in CI/CD Pipelines

ESLint can be integrated into continuous integration workflows.

A typical CI pipeline runs linting before building or deploying an application.

Example:

```text
Developer Pushes Code
         |
         v
    CI Pipeline
         |
         v
    Install Packages
         |
         v
     Run ESLint
         |
         v
   Run Type Checking
         |
         v
       Build
         |
         v
      Deploy
```

Example GitHub Actions workflow:

**.github/workflows/ci.yml**

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lint:
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

      - name: Run ESLint
        run: npm run lint
```

If ESLint reports errors, the workflow fails and prevents the pipeline from proceeding successfully.

# 🗁 Common ESLint Errors

**Definition for rule was not found**

This usually means a rule is misspelled, unavailable in the installed ESLint version, or provided by a plugin that has not been configured.

Verify the rule name and installed plugins.

**Parsing error**

Possible causes:

* Invalid JavaScript syntax.
* Incorrect parser configuration.
* Unsupported language features.
* Missing TypeScript parser configuration.

Verify the source syntax and parser settings.

**Cannot find package**

Install the missing dependency:

```bash
npm install --save-dev eslint
```

For TypeScript projects, verify that `typescript-eslint` is installed.

**ESLint ignores a file unexpectedly**

Check:

* The `files` patterns.
* The `ignores` patterns.
* The configuration file location.
* Whether the target file matches any configured rules.

# 🗁 Best Practices

* Use the flat configuration format for modern ESLint projects.
* Keep ESLint configuration in version control.
* Use recommended rule sets as a starting point.
* Add project-specific rules gradually.
* Use TypeScript ESLint for TypeScript source files.
* Enable type-aware linting when its additional analysis is valuable.
* Use Prettier for consistent formatting.
* Avoid excessive custom rules that make development unnecessarily restrictive.
* Run linting before committing code.
* Integrate linting into CI/CD pipelines.
* Keep ESLint and its plugins updated.

# 🗁 Summary

ESLint is a static code analysis tool that helps developers maintain code quality and consistency.

Its main capabilities include:

* Detecting potential bugs and problematic patterns.
* Enforcing coding standards.
* Supporting JavaScript and TypeScript.
* Integrating with React and other frameworks through plugins.
* Automatically fixing supported issues.
* Integrating with editors and CI/CD pipelines.

ESLint is an important part of a professional JavaScript and TypeScript development workflow.
