# JavaScript Installation

JavaScript does not require a complicated installation process to get started. Modern web browsers already include JavaScript engines, allowing you to write and execute JavaScript directly in the browser.

For local development, you can install Node.js, use a code editor such as Visual Studio Code, and execute JavaScript programs from the terminal.

This guide covers browser-based JavaScript execution, Node.js installation, environment verification, and basic project setup.

# 🗁 JavaScript Development Requirements

The following tools are recommended for JavaScript development:

| Tool | Purpose | Required |
| --- | --- | --- |
| Web browser | Execute JavaScript in the browser | Yes, for browser development |
| Code editor | Write and edit JavaScript files | Recommended |
| Node.js | Execute JavaScript outside browsers | Recommended |
| npm | Install and manage packages | Included with Node.js |
| Terminal | Execute commands and scripts | Recommended |
| Git | Version control | Optional |

# 🗁 Using JavaScript Without Installation

You can execute JavaScript directly in a modern web browser without installing Node.js.

**Steps:**

* Open Google Chrome, Firefox, Edge, or another modern browser.
* Open the browser's Developer Tools.
* Navigate to the Console tab.
* Enter JavaScript code.
* Press Enter to execute it.

Example:

```javascript
console.log("Hello, JavaScript!");
```

Output:

```text
Hello, JavaScript!
```

You can also execute expressions directly:

```javascript
10 + 20;
```

Output:

```text
30
```

This is useful for learning JavaScript fundamentals and testing small code snippets.

# 🗁 Installing Visual Studio Code

Visual Studio Code (VS Code) is a popular code editor for JavaScript development.

Download it from the official website:

[Visual Studio Code](https://code.visualstudio.com/)

**Installation steps:**

* Open the official Visual Studio Code website.
* Download the installer for your operating system.
* Run the installer.
* Follow the installation instructions.
* Launch Visual Studio Code.

Recommended extensions:

| Extension | Purpose |
| --- | --- |
| ESLint | JavaScript code linting |
| Prettier | Code formatting |
| JavaScript (ES6) code snippets | JavaScript code snippets |
| Error Lens | Displays diagnostics directly in the editor |

These extensions are optional. VS Code already provides built-in JavaScript and TypeScript language support.

# 🗁 Installing Node.js

Node.js is a JavaScript runtime that allows you to execute JavaScript outside a web browser.

It also includes npm, the Node Package Manager, which is used to install and manage JavaScript packages.

Download Node.js from the official website:

[Node.js Official Website](https://nodejs.org/)

For most users, the **LTS (Long-Term Support)** release is the recommended choice.

# 🗁 Installing Node.js on Windows

Follow these steps to install Node.js on Windows.

**Download the installer**

* Open the [Node.js download page](https://nodejs.org/en/download).
* Select the Windows installer.
* Download the `.msi` installer for your system architecture.

**Run the installer**

* Open the downloaded installer.
* Accept the license agreement.
* Choose the installation directory.
* Keep the default installation options unless you have a specific requirement.
* Ensure npm is included in the installation.
* Complete the installation.

**Verify the installation**

Open PowerShell, Command Prompt, or the VS Code terminal.

Check the Node.js version:

```powershell
node --version
```

Check the npm version:

```powershell
npm --version
```

Both commands should display installed version numbers.

Example output:

```text
v22.x.x
10.x.x
```

The exact versions depend on the Node.js release you install.

# 🗁 Installing Node.js on Linux

Node.js can be installed on Linux using a distribution package manager, an official installation method, or a version manager.

**Ubuntu and Debian**

You can install the distribution-provided packages:

```bash
sudo apt update
sudo apt install nodejs npm
```

Verify:

```bash
node --version
npm --version
```

The versions provided by your distribution repositories may not be the latest available.

**Using Node Version Manager (nvm)**

For development environments, a Node.js version manager is useful because it allows you to install and switch between multiple Node.js versions.

Official project:

[Node Version Manager (nvm)](https://github.com/nvm-sh/nvm)

After installing nvm according to its official instructions, install the latest LTS release:

```bash
nvm install --lts
```

Activate the installed LTS version:

```bash
nvm use --lts
```

Check the versions:

```bash
node --version
npm --version
```

Using a version manager is especially useful when different projects require different Node.js versions.

# 🗁 Installing Node.js on macOS

Node.js can be installed on macOS using the official installer or a package manager.

**Using the official installer**

* Visit the [Node.js download page](https://nodejs.org/en/download).
* Download the macOS installer.
* Open the downloaded `.pkg` file.
* Follow the installation instructions.
* Complete the installation.

Verify:

```bash
node --version
npm --version
```

**Using Homebrew**

If Homebrew is installed, you can install Node.js with:

```bash
brew install node
```

Verify:

```bash
node --version
npm --version
```

# 🗁 Understanding Node.js and npm

Node.js and npm serve different purposes.

**Node.js**

Node.js is a JavaScript runtime used to execute JavaScript outside a browser.

Example:

```bash
node app.js
```

**npm**

npm is a package manager used to install, update, and manage JavaScript dependencies.

Example:

```bash
npm install express
```

This installs the Express package into the current project.

Check installed versions:

```bash
node --version
npm --version
```

# 🗁 Creating Your First JavaScript Project

After installing the required tools, create a directory for your JavaScript practice.

Create a project directory:

```bash
mkdir javascript-practice
```

Navigate into the directory:

```bash
cd javascript-practice
```

Open the directory in Visual Studio Code:

```bash
code .
```

Create a file named:

```text
app.js
```

Add the following JavaScript code:

```javascript
console.log("Hello, JavaScript!");
console.log("My JavaScript environment is ready.");
```

Your project structure should look like this:

```text
javascript-practice/
└── app.js
```

# 🗁 Running JavaScript with Node.js

Open the terminal inside your project directory.

Execute the JavaScript file:

```bash
node app.js
```

Output:

```text
Hello, JavaScript!
My JavaScript environment is ready.
```

Node.js reads and executes the JavaScript file.

You can run JavaScript files directly without compiling them manually.

# 🗁 Running JavaScript in an HTML File

You can also execute JavaScript through an HTML document.

Create an `index.html` file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript Practice</title>
</head>
<body>

    <h1>JavaScript Introduction</h1>

    <script>
        console.log("Hello from the browser!");
    </script>

</body>
</html>
```

Open the HTML file in a browser.

Open Developer Tools and navigate to the Console tab to see the output.

**Using an external JavaScript file**

For better code organization, JavaScript can be written in a separate file.

Project structure:

```text
javascript-practice/
├── index.html
└── app.js
```

`app.js`:

```javascript
console.log("JavaScript loaded from an external file.");
```

`index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript Practice</title>
</head>
<body>

    <h1>JavaScript Practice</h1>

    <script src="app.js"></script>
</body>
</html>
```

The `src` attribute specifies the path to the external JavaScript file.

# 🗁 Using the VS Code Terminal

Visual Studio Code provides an integrated terminal, allowing you to execute commands without switching applications.

Open the terminal using:

```text
Ctrl + `
```

On Linux and macOS, you can also open it through:

**Terminal → New Terminal**

On Windows, you can select PowerShell or Command Prompt as the terminal profile.

Useful commands:

| Command | Purpose |
| --- | --- |
| `pwd` | Display the current directory |
| `ls` | List directory contents |
| `cd folder-name` | Navigate into a directory |
| `mkdir folder-name` | Create a directory |
| `node --version` | Display Node.js version |
| `npm --version` | Display npm version |
| `node app.js` | Execute a JavaScript file |
| `code .` | Open the current directory in VS Code |

# 🗁 Checking the Development Environment

After completing the installation, verify that the required tools are available.

Run:

```bash
node --version
npm --version
code --version
```

If the commands return version information, the corresponding tools are available through your terminal.

You can also verify JavaScript execution with:

```bash
node -e "console.log('JavaScript environment is working!')"
```

Expected output:

```text
JavaScript environment is working!
```

# 🗁 Common Installation Problems

**Node.js command not found**

Possible causes:

* Node.js is not installed.
* The installation directory is missing from the system PATH.
* The terminal was opened before installation completed.

Solutions:

* Verify that Node.js is installed.
* Restart the terminal.
* Check the system PATH configuration.
* Reinstall Node.js if necessary.

**npm command not found**

Possible causes:

* npm was not installed correctly.
* The Node.js installation directory is missing from PATH.

Solutions:

* Verify the Node.js installation.
* Check whether npm is available in the installation directory.
* Reinstall Node.js with npm included.

**VS Code command not found**

The `code` command may not be available in your terminal PATH.

Solutions:

* Restart the terminal after installing VS Code.
* Enable the command-line launcher through your operating system's installation or PATH configuration.
* Open VS Code manually and use its integrated terminal.

**Different Node.js versions across projects**

Some projects require different Node.js versions.

A version manager such as nvm allows you to install and switch between versions without repeatedly reinstalling Node.js.

# 🗁 Recommended Development Setup

For a practical JavaScript learning environment, use:

* Visual Studio Code as your code editor.
* Node.js LTS as your JavaScript runtime.
* npm as your package manager.
* Chrome or Firefox for browser-based development.
* Browser Developer Tools for debugging.
* Git for version control.

This setup provides everything needed to learn JavaScript fundamentals and develop real-world applications.

# 🗁 Official References

* [Node.js Official Website](https://nodejs.org/)
* [Node.js Installation Documentation](https://nodejs.org/en/download)
* [npm Documentation](https://docs.npmjs.com/)
* [Visual Studio Code Documentation](https://code.visualstudio.com/docs)
* [nvm GitHub Repository](https://github.com/nvm-sh/nvm)
