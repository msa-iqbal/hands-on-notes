# Node.js on Ubuntu

# 1. Check your Ubuntu system

```bash
lsb_release -a
```

Also check architecture:

```bash
uname -m
```

Typical output:

```text
x86_64
```

Update your system:

```bash
sudo apt update
sudo apt upgrade -y
```

Install basic tools:

```bash
sudo apt install -y curl git build-essential
```

Verify:

```bash
curl --version
git --version
gcc --version
```

---

# 2. Recommended method: Install NVM

NVM allows you to manage multiple Node.js versions.

For example:

```text
Node 20
Node 22
Node 24
Node 25
```

and switch between them:

```bash
nvm use 22
```

### Install NVM

Use the official NVM installation script:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

Then reload your shell:

```bash
source ~/.bashrc
```

Verify:

```bash
nvm --version
```

You should get something similar to:

```text
0.40.3
```

If you're using Zsh:

```bash
source ~/.zshrc
```

---

# 3. Verify NVM

Run:

```bash
command -v nvm
```

Expected:

```text
nvm
```

You can also run:

```bash
nvm --version
```

---

# 4. See available Node.js versions

```bash
nvm ls-remote
```

This displays available Node.js releases.

If you only want LTS releases:

```bash
nvm ls-remote --lts
```

---

# 5. Install the latest LTS Node.js

For a development machine, this is usually the best choice:

```bash
nvm install --lts
```

Then verify:

```bash
node --version
```

```bash
npm --version
```

Example:

```text
v24.x.x
11.x.x
```

The exact versions will change over time.

---

# 6. Make LTS your default Node.js

Run:

```bash
nvm alias default 'lts/*'
```

Now open a new terminal:

```bash
node -v
```

Your LTS version should automatically be selected.

---

# 7. Install a specific Node.js version

For example:

```bash
nvm install 22
```

Or:

```bash
nvm install 24
```

Then:

```bash
nvm use 22
```

Check:

```bash
node -v
```

---

# 8. Install the latest Node.js

If you specifically want the newest current release:

```bash
nvm install node
```

Then:

```bash
node -v
```

`node` is NVM's alias for the latest Node.js release.

For production/development stability, however, I generally recommend **LTS**.

---

# 9. Install multiple Node versions

For example:

```bash
nvm install 20
nvm install 22
nvm install 24
```

See installed versions:

```bash
nvm ls
```

You'll see something similar to:

```text
       v20.x.x
->     v22.x.x
       v24.x.x
default -> 22
```

---

# 10. Switch Node.js versions

Switch to Node 20:

```bash
nvm use 20
```

Switch to Node 22:

```bash
nvm use 22
```

Switch to Node 24:

```bash
nvm use 24
```

Verify:

```bash
node -v
```

---

# 11. Set your default Node version

For example:

```bash
nvm alias default 22
```

Now every new terminal will use Node 22.

Check:

```bash
nvm current
```

---

# 12. Node.js + npm relationship

Installing Node.js through NVM automatically installs npm.

Check:

```bash
node -v
```

```bash
npm -v
```

You can also check their locations:

```bash
which node
```

```bash
which npm
```

With NVM, you should see paths similar to:

```text
/home/username/.nvm/versions/node/v22.x.x/bin/node
```

and:

```text
/home/username/.nvm/versions/node/v22.x.x/bin/npm
```

---

# 13. Test Node.js

Create a test file:

```bash
mkdir -p ~/node-test
cd ~/node-test
```

Create:

```bash
nano app.js
```

Put:

```javascript
console.log("Node.js is working!");
```

Save and run:

```bash
node app.js
```

Output:

```text
Node.js is working!
```

---

# 14. Test npm

Initialize a project:

```bash
npm init
```

Or skip the questions:

```bash
npm init -y
```

You'll get:

```text
package.json
```

View it:

```bash
cat package.json
```

---

# 15. Install an npm package

For example:

```bash
npm install lodash
```

You'll get:

```text
node_modules/
package-lock.json
package.json
```

Check:

```bash
ls
```

---

# 16. Install development dependencies

Example:

```bash
npm install --save-dev eslint
```

Or:

```bash
npm install -D prettier
```

The package will appear under:

```json
"devDependencies"
```

---

# 17. Install a package globally

Example:

```bash
npm install -g typescript
```

Check:

```bash
tsc --version
```

Because you're using NVM, global packages are installed under your NVM-managed Node environment instead of requiring:

```bash
sudo npm install -g ...
```

**Avoid `sudo npm install -g` when using NVM.**

---

# 18. Useful global developer tools

For your JavaScript/TypeScript development environment, you might want:

### TypeScript

```bash
npm install -g typescript
```

### tsx

```bash
npm install -g tsx
```

### pnpm

```bash
npm install -g pnpm
```

### Yarn

```bash
npm install -g yarn
```

### npm-check-updates

```bash
npm install -g npm-check-updates
```

Check:

```bash
tsc --version
tsx --version
pnpm --version
yarn --version
ncu --version
```

---

# 19. npm configuration

Check your configuration:

```bash
npm config list
```

Check npm prefix:

```bash
npm config get prefix
```

Check cache:

```bash
npm config get cache
```

Check registry:

```bash
npm config get registry
```

Normally:

```text
https://registry.npmjs.org/
```

---

# 20. npm cache

Check cache:

```bash
npm cache verify
```

If you genuinely need to clear it:

```bash
npm cache clean --force
```

Normally, you shouldn't need to clear npm's cache.

---

# 21. Update npm

You can update npm:

```bash
npm install -g npm@latest
```

Check:

```bash
npm -v
```

However, don't blindly update npm across every Node version. Your Node/npm combination should remain compatible.

---

# 22. Update Node.js

With NVM, this is easy.

First see installed versions:

```bash
nvm ls
```

Install the newest LTS:

```bash
nvm install --lts
```

Then:

```bash
nvm use --lts
```

Make it default:

```bash
nvm alias default 'lts/*'
```

---

# 23. Update an existing Node version

Suppose you're using Node 22:

```bash
nvm install 22
```

NVM will install the latest available Node 22 release.

Then:

```bash
nvm use 22
```

---

# 24. Remove an old Node.js version

See installed versions:

```bash
nvm ls
```

Suppose you have:

```text
v20.x.x
v22.x.x
v24.x.x
```

Remove Node 20:

```bash
nvm uninstall 20
```

Or a specific version:

```bash
nvm uninstall 20.19.0
```

---

# 25. Project-specific Node.js versions

This is one of NVM's best features.

Go into your project:

```bash
cd ~/Projects/my-project
```

Set Node 22:

```bash
nvm use 22
```

Create:

```bash
echo "22" > .nvmrc
```

Now whenever you enter the project:

```bash
nvm use
```

NVM reads `.nvmrc` and selects Node 22.

A better `.nvmrc` can be:

```text
22
```

or:

```text
24
```

depending on your project requirements.

---

# 26. Recommended project setup

For a new JavaScript project:

```bash
mkdir my-project
cd my-project

nvm use 22
npm init -y
```

Then:

```bash
git init
```

Create:

```bash
.gitignore
```

Add:

```gitignore
node_modules/
.env
.env.*
dist/
build/
coverage/
```

---

# 27. npm scripts

Your `package.json` can contain:

```json
{
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node --test"
  }
}
```

Run:

```bash
npm run dev
```

or:

```bash
npm start
```

or:

```bash
npm test
```

Modern Node.js has built-in capabilities such as `--watch` and the built-in test runner, so you don't necessarily need a package for everything.

---

# 28. Node.js REPL

Simply run:

```bash
node
```

You'll get:

```text
>
```

Try:

```javascript
2 + 2
```

Output:

```text
4
```

Try:

```javascript
console.log("Hello Ubuntu");
```

Exit:

```text
.exit
```

Or press:

```text
Ctrl+D
```

---

# 29. Run JavaScript directly

You can execute:

```bash
node -e "console.log('Hello World')"
```

Output:

```text
Hello World
```

Useful for quick testing.

---

# 30. Environment variables

Linux:

```bash
export NODE_ENV=development
```

Check:

```bash
echo $NODE_ENV
```

Inside Node:

```javascript
console.log(process.env.NODE_ENV);
```

For project-specific environment variables, `.env` files are commonly used.

Example:

```env
PORT=3000
DATABASE_URL=...
```

Never commit secrets:

```gitignore
.env
```

---

# 31. Check Node.js executable location

```bash
which node
```

```bash
readlink -f "$(which node)"
```

Check npm:

```bash
which npm
```

This is particularly useful when diagnosing multiple Node installations.

---

# 32. Check NVM environment

```bash
echo $NVM_DIR
```

Normally:

```text
/home/username/.nvm
```

Check:

```bash
ls ~/.nvm
```

---

# 33. If `nvm: command not found`

Run:

```bash
source ~/.bashrc
```

Then:

```bash
nvm --version
```

If you're using Zsh:

```bash
source ~/.zshrc
```

You can check:

```bash
grep -n "NVM" ~/.bashrc
```

You should have NVM initialization lines.

---

# 34. If Node works but npm doesn't

Check:

```bash
which node
which npm
```

Then:

```bash
nvm current
```

Try:

```bash
nvm use default
```

Then:

```bash
node -v
npm -v
```

---

# 35. If Ubuntu has an old Node.js installation

Check:

```bash
which node
```

If you see:

```text
/usr/bin/node
```

you may have an APT-installed Node.js competing with NVM.

Check:

```bash
apt policy nodejs
```

You can also inspect:

```bash
dpkg -l | grep nodejs
```

If you're migrating completely to NVM, remove the APT Node.js installation:

```bash
sudo apt remove nodejs npm
```

Then:

```bash
sudo apt autoremove
```

Reload:

```bash
source ~/.bashrc
```

Then:

```bash
nvm use --lts
```

Verify:

```bash
which node
```

It should now point somewhere under:

```text
~/.nvm/versions/node/
```

---

# 36. Don't mix installation methods

I recommend **not** doing this:

```text
APT Node.js
+
Snap Node.js
+
NVM Node.js
+
Flatpak Node.js
```

That can create PATH conflicts and extremely confusing npm behavior.

For a developer workstation:

```text
Ubuntu
   │
   └── NVM
        ├── Node 20
        ├── Node 22
        └── Node 24
```

is a much cleaner architecture.

---

# 37. NVM + VS Code

VS Code terminals should normally inherit your shell environment.

Check inside VS Code terminal:

```bash
node -v
```

```bash
npm -v
```

```bash
nvm current
```

If VS Code doesn't recognize NVM, restart VS Code after installing NVM.

---

# 38. NVM + Git

A good project workflow:

```bash
git clone https://github.com/example/project.git
cd project
```

If the repository has:

```text
.nvmrc
```

run:

```bash
nvm use
```

Then:

```bash
npm install
```

Then:

```bash
npm run dev
```

This makes the Node version explicit per project.

---

# 39. npm install vs npm ci

### Development

Usually:

```bash
npm install
```

### CI/CD / production

Prefer:

```bash
npm ci
```

`npm ci` uses `package-lock.json` and is designed for clean, reproducible installations.

---

# 40. Update project dependencies

Check outdated packages:

```bash
npm outdated
```

Update according to your package.json ranges:

```bash
npm update
```

For more aggressive dependency updates, tools such as `npm-check-updates` can help:

```bash
ncu
```

---

# 41. Security audit

Run:

```bash
npm audit
```

Try automatic fixes:

```bash
npm audit fix
```

Review changes before using:

```bash
npm audit fix --force
```

`--force` can introduce breaking dependency changes, so don't use it casually.

---

# 42. Find globally installed packages

```bash
npm list -g --depth=0
```

Example:

```text
├── npm
├── pnpm
├── typescript
└── tsx
```

---

# 43. Remove global npm packages

Example:

```bash
npm uninstall -g typescript
```

Check:

```bash
npm list -g --depth=0
```

---

# 44. Clean `node_modules`

If a project becomes problematic:

```bash
rm -rf node_modules
```

Then:

```bash
npm install
```

For a clean lockfile-based installation:

```bash
rm -rf node_modules
npm ci
```

Don't delete `package-lock.json` unless you intentionally want to regenerate dependency resolution.

---

# 45. Check Node.js system information

Run:

```bash
node
```

Then:

```javascript
process.versions
```

Or directly:

```bash
node -p "process.versions"
```

Check architecture:

```bash
node -p "process.arch"
```

Check OS:

```bash
node -p "process.platform"
```

---

# 46. Node.js memory information

```bash
node -e "console.log(require('os').totalmem())"
```

You can also inspect:

```bash
node -p "require('os').cpus().length"
```

---

# 47. Useful Node.js commands

|Task|Command|
|---|---|
|Node version|`node -v`|
|npm version|`npm -v`|
|NVM version|`nvm -v`|
|Current Node|`nvm current`|
|Installed Nodes|`nvm ls`|
|Available LTS|`nvm ls-remote --lts`|
|Install LTS|`nvm install --lts`|
|Use LTS|`nvm use --lts`|
|Install Node 22|`nvm install 22`|
|Use Node 22|`nvm use 22`|
|Default Node|`nvm alias default 22`|
|Remove Node|`nvm uninstall 20`|
|npm packages|`npm list`|
|Global packages|`npm list -g --depth=0`|
|Audit|`npm audit`|
|Outdated|`npm outdated`|
|Project init|`npm init -y`|
|Install dependencies|`npm install`|
|CI install|`npm ci`|

---

# 48. My recommended Ubuntu developer setup

For your JavaScript/TypeScript + React/Node development environment, I'd structure it like this:

```text
Ubuntu
│
├── Git
│
├── NVM
│   │
│   ├── Node.js 20 LTS
│   ├── Node.js 22 LTS
│   └── Node.js 24 LTS
│
├── npm
│
├── pnpm
│
├── TypeScript
│
├── VS Code
│
├── React
│
├── Next.js
│
├── Node.js APIs
│
├── PostgreSQL
│
└── MongoDB
```

For most new projects, I'd use:

```bash
nvm install --lts
nvm alias default 'lts/*'
```

Then:

```bash
npm install -g pnpm typescript tsx
```

Verify the whole environment:

```bash
node -v
npm -v
pnpm -v
tsc -v
tsx -v
git --version
```

---

# 49. One-command recommended installation

If you want the clean setup without all the explanations:

```bash
sudo apt update && \
sudo apt install -y curl git build-essential && \
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash && \
source ~/.bashrc && \
nvm install --lts && \
nvm alias default 'lts/*'
```

Then:

```bash
node -v
npm -v
nvm -v
```

---

## My recommendation

For **Ubuntu + professional JavaScript/TypeScript development**, use:

**NVM → Node.js LTS → npm/pnpm → project-level `.nvmrc`**

rather than:

**`sudo apt install nodejs npm`**

The NVM approach is substantially better when you work across different React, Next.js, Node.js, and TypeScript projects because each project can pin its own Node version without fighting Ubuntu's system packages.
