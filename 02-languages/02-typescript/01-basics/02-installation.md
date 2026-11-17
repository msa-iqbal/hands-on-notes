# TypeScript Installation

Installing TypeScript involves setting up a JavaScript development environment and installing the TypeScript compiler.

TypeScript requires a JavaScript runtime or execution environment for running the generated JavaScript.

A typical TypeScript development environment includes:

* Node.js
* npm
* TypeScript compiler (`tsc`)
* Code editor
* Terminal or command-line interface

This guide covers TypeScript installation on Linux, Windows, and macOS, including global and project-local installation.

# 🗁 Prerequisites

Before installing TypeScript, make sure Node.js and npm are available.

Node.js provides the JavaScript runtime, while npm manages JavaScript packages.

Check Node.js:

```bash
node --version
```

Check npm:

```bash
npm --version
```

Example output:

```text
v22.x.x
10.x.x
```

The actual versions depend on your installed environment.

If these commands are unavailable, install Node.js first.

# 🗁 Install Node.js on Linux

On Ubuntu and other Debian-based distributions, you can install Node.js and npm through the package manager.

Update package information:

```bash
sudo apt update
```

Install Node.js and npm:

```bash
sudo apt install nodejs npm
```

Verify installation:

```bash
node --version
npm --version
```

For modern development projects, a current Node.js LTS release is generally preferable to an older distribution repository version.

You can also use a Node.js version manager such as `nvm` to install and switch between Node.js versions.

# 🗁 Install Node.js on Windows

Download the Node.js installer from the official website:

<https://nodejs.org/>

Installation steps:

* Download the Windows installer.
* Run the installer.
* Accept the license agreement.
* Select the installation directory.
* Keep the npm package manager option enabled.
* Complete the installation.
* Open a new terminal.

Verify the installation:

```powershell
node --version
npm --version
```

# 🗁 Install Node.js on macOS

You can install Node.js using the official installer or a package manager such as Homebrew.

Using Homebrew:

```bash
brew install node
```

Verify installation:

```bash
node --version
npm --version
```

# 🗁 Install TypeScript Globally

A global installation makes the TypeScript compiler available from different project directories.

Install TypeScript globally:

```bash
npm install --global typescript
```

Verify installation:

```bash
tsc --version
```

Example output:

```text
Version 5.x.x
```

The exact version depends on the package installed.

You can now execute the TypeScript compiler from your terminal.

Example:

```bash
tsc app.ts
```

**Advantages:**

* Easy access to `tsc` from any directory.
* Convenient for experimentation and learning.
* No need to install the compiler separately in every project.

**Disadvantages:**

* Different projects may require different compiler versions.
* Team members may have inconsistent global installations.
* Builds may behave differently across development environments.

For professional projects, local installation is generally recommended.

# 🗁 Install TypeScript Locally

A local installation adds TypeScript as a development dependency of a specific project.

Create a project directory:

```bash
mkdir typescript-project

cd typescript-project
```

Initialize npm:

```bash
npm init -y
```

Install TypeScript:

```bash
npm install --save-dev typescript
```

This installs TypeScript inside the project's `node_modules` directory.

It also adds TypeScript to the `devDependencies` section of `package.json`.

Example:

```json
{
    "devDependencies": {
        "typescript": "^5.0.0"
    }
}
```

The version shown here is illustrative. npm records the actual version installed.

Check the local compiler version:

```bash
npx tsc --version
```

Run the compiler:

```bash
npx tsc
```

`npx` can execute the project's locally installed TypeScript compiler.

**Advantages:**

* Each project controls its compiler version.
* Team members can install the same project dependencies.
* Build environments become more predictable.
* CI/CD systems can use the project's declared dependencies.

# 🗁 Global vs Local Installation

| Feature | Global Installation | Local Installation |
| --- | --- | --- |
| Installation command | `npm install -g typescript` | `npm install -D typescript` |
| Scope | Entire system or user environment | Individual project |
| Compiler command | `tsc` | `npx tsc` |
| Version management | Shared global version | Project-specific version |
| Team consistency | Less predictable | More predictable |
| Recommended usage | Learning and experimentation | Professional projects |

You can have both global and local installations on the same system.

When using `npx tsc` inside a project, npm generally uses the project's local compiler when available.

# 🗁 Create a TypeScript Project

Create a new project:

```bash
mkdir my-typescript-app

cd my-typescript-app
```

Initialize npm:

```bash
npm init -y
```

Install TypeScript:

```bash
npm install --save-dev typescript
```

Create the source directory:

```bash
mkdir src
```

Create a TypeScript file:

```bash
touch src/index.ts
```

On Windows PowerShell:

```powershell
New-Item -ItemType Directory -Path src
New-Item -ItemType File -Path src/index.ts
```

# 🗁 Configure TypeScript

Generate the TypeScript configuration file:

```bash
npx tsc --init
```

This creates:

```text
tsconfig.json
```

The configuration file controls TypeScript compiler behavior.

A basic example:

```json
{
    "compilerOptions": {
        "target": "ES2022",
        "module": "NodeNext",
        "moduleResolution": "NodeNext",
        "rootDir": "./src",
        "outDir": "./dist",
        "strict": true,
        "skipLibCheck": true
    },
    "include": ["src/**/*.ts"],
    "exclude": ["node_modules", "dist"]
}
```

The `NodeNext` module configuration is intended for Node.js projects using modern Node.js module resolution. Other runtimes and bundlers may require different settings.

# 🗁 Create Your First TypeScript File

Open `src/index.ts` and add:

```typescript
const message: string = "Hello, TypeScript!";

console.log(message);
```

Your project structure should look like this:

```text
my-typescript-app/
│
├── node_modules/
│
├── src/
│   └── index.ts
│
├── package.json
├── package-lock.json
└── tsconfig.json
```

# 🗁 Compile TypeScript

Compile the project:

```bash
npx tsc
```

The compiler reads `tsconfig.json` and generates JavaScript files in the configured output directory.

Generated project structure:

```text
my-typescript-app/
│
├── dist/
│   └── index.js
│
├── node_modules/
│
├── src/
│   └── index.ts
│
├── package.json
├── package-lock.json
└── tsconfig.json
```

# 🗁 Run the Generated JavaScript

Execute the generated JavaScript using Node.js:

```bash
node dist/index.js
```

Output:

```text
Hello, TypeScript!
```

The complete development workflow is:

```text
TypeScript Source
       |
       v
   Type Checking
       |
       v
   JavaScript Output
       |
       v
   JavaScript Runtime
       |
       v
    Application
```

# 🗁 Add npm Scripts

You can define reusable commands in `package.json`.

Example:

```json
{
    "scripts": {
        "build": "tsc",
        "typecheck": "tsc --noEmit",
        "start": "node dist/index.js"
    },
    "devDependencies": {
        "typescript": "^5.0.0"
    }
}
```

Now execute:

Build the project:

```bash
npm run build
```

Check types without generating JavaScript:

```bash
npm run typecheck
```

Run the generated application:

```bash
npm start
```

These scripts make common development tasks easier to execute.

# 🗁 Install TypeScript in Visual Studio Code

<text>Visual Studio Code</text> provides built-in TypeScript language support.

Download:

<https://code.visualstudio.com/>

Installation steps:

* Download and install Visual Studio Code.
* Open your TypeScript project.
* Open a `.ts` or `.tsx` file.
* Check that TypeScript language support is active.
* Configure the workspace to use the project's TypeScript version when necessary.

VS Code provides features such as:

* IntelliSense.
* Type checking diagnostics.
* Code completion.
* Go to Definition.
* Find References.
* Rename Symbol.
* Automatic imports.
* Refactoring support.
* Inline documentation.

The editor's TypeScript language service can provide diagnostics even before running the compiler manually.

# 🗁 Use the Workspace TypeScript Version

VS Code may use its bundled TypeScript version by default.

To use the version installed in the project:

* Open a TypeScript file.
* Open the Command Palette.
* Search for `TypeScript: Select TypeScript Version`.
* Choose `Use Workspace Version`.

This helps align editor diagnostics with the compiler version used by the project.

# 🗁 Install TypeScript in WebStorm

WebStorm provides built-in TypeScript support.

Download:

<https://www.jetbrains.com/webstorm/>

After installation:

* Open the TypeScript project.
* Ensure the project dependencies are installed.
* Open the TypeScript settings.
* Configure the TypeScript package path if necessary.
* Select the project's local TypeScript installation.

Typical local package path:

```text
node_modules/typescript
```

WebStorm provides TypeScript inspections, code completion, navigation, and refactoring tools.

# 🗁 Verify TypeScript Installation

Use the following commands to verify your development environment:

Check Node.js:

```bash
node --version
```

Check npm:

```bash
npm --version
```

Check global TypeScript:

```bash
tsc --version
```

Check local TypeScript:

```bash
npx tsc --version
```

Check the installed package:

```bash
npm list typescript
```

Check the compiler configuration:

```bash
npx tsc --showConfig
```

These commands help identify installation and configuration problems.

# 🗁 Common Installation Problems

**`tsc: command not found`**

Possible causes:

* TypeScript is not installed globally.
* The global npm executable directory is not in `PATH`.
* The terminal was opened before installation.

Solution:

```bash
npm install --global typescript
```

For a project-local installation:

```bash
npx tsc --version
```

**`npm: command not found`**

Node.js or npm may not be installed correctly.

Verify the installation and make sure the executable directory is included in the system `PATH`.

**Permission Errors on Linux**

Global npm installation may fail because of directory permissions.

Prefer configuring npm to use a user-owned global package directory or use a Node.js version manager rather than repeatedly running npm with elevated privileges.

**Incorrect TypeScript Version**

Check the installed version:

```bash
npx tsc --version
```

Check the project dependency:

```bash
npm list typescript
```

Install a specific version when required:

```bash
npm install --save-dev typescript@5.9
```

The version number is an example; choose the version required by your project.

**Compilation Errors**

Check the compiler configuration:

```bash
npx tsc --showConfig
```

Then inspect the reported errors and verify that the source files and compiler options are correct.

# 🗁 Recommended Installation Practices

* Use a supported Node.js LTS release.
* Prefer project-local TypeScript installation.
* Commit `package.json` and `package-lock.json`.
* Use `npm ci` in automated build environments.
* Keep the TypeScript version consistent across the team.
* Use `tsconfig.json` for compiler configuration.
* Enable strict type checking for new projects.
* Use npm scripts for common development commands.
* Avoid unnecessary global package installations.
* Use a compatible editor with TypeScript language support.

# 🗁 Key Takeaways

* TypeScript requires a JavaScript development environment.
* Node.js provides the runtime for executing generated JavaScript.
* npm manages TypeScript and other project dependencies.
* The TypeScript compiler is installed through npm.
* Global installation provides system-wide compiler access.
* Local installation provides project-specific version management.
* `tsconfig.json` controls compiler behavior.
* `npx tsc` executes the locally installed compiler.
* VS Code and WebStorm provide built-in TypeScript development support.
* A project-local compiler is generally the best choice for team development.

# 🗁 Related Topics

* TypeScript Introduction
* First TypeScript Program
* TypeScript vs JavaScript
* TypeScript Compilation
* TypeScript Compiler
* TSC
* Node.js
* npm
* package.json
* tsconfig.json
* TypeScript Tooling
* Development Environment
