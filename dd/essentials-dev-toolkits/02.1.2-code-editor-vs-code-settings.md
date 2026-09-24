Custom/User Settings for VS Code Editor:

- Open **VS Code Settings** (`Ctrl+,`). or Go to VS Code Settings
- Then, Open `settings.json`

![VS-Code-User-Settings](https://res.cloudinary.com/du7tjwbyi/image/upload/v1787296828/image_iicg1f.png)

Below is an example custom configuration:

## SETTING-1

```json
{
  // --- Font Settings ---
  "editor.fontFamily": "'Operator Mono Lig Light', 'JetBrains Mono Light', monospace",
  "editor.fontLigatures": true,
  "editor.fontSize": 14,
  "editor.lineHeight": 16,
  "editor.inlayHints.enabled": "on",
  "editor.inlayHints.fontFamily": "'Operator Mono Lig Light', 'JetBrains Mono Light', monospace",

  // --- Formatting ---
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.formatOnType": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",

  // --- Cursor & Scrolling ---
  "editor.cursorBlinking": "expand",
  "editor.smoothScrolling": true,
  "editor.mouseWheelZoom": true,

  // --- Editor Behavior ---
  "editor.bracketPairColorization.enabled": true,
  "editor.linkedEditing": true,
  "editor.accessibilitySupport": "off",
  "editor.detectIndentation": false,
  "editor.indentSize": 4,
  "editor.links": false,
  "editor.minimap.enabled": true,
  "editor.minimap.renderCharacters": false,
  "editor.minimap.size": "fit",
  "editor.stickyScroll.enabled": false, // Optional

  // Token Style Overrides (Italicize useful tokens)
  "editor.tokenColorCustomizations": {
    "textMateRules": [
      {
        "scope": [
          "keyword",
          "keyword.control",
          "keyword.operator",
          "storage.type",
          "entity.name.function",
          "variable.language",
          "support.function",
          "meta.function-call",
          "meta.class",
          "meta.import",
          "string.quoted",
          "string.template",
          "constant.language",
          "markup.bold"
        ],
        "settings": {
          "fontStyle": "italic"
        }
      }
    ]
  },

  // --- Workbench UI ---
  //"workbench.activityBar.location": "hidden",
  //"workbench.statusBar.visible": false,
  "workbench.sideBar.location": "right",
  "workbench.tips.enabled": false,
  "workbench.startupEditor": "none",
  "workbench.colorTheme": "Dracula Theme",
  "workbench.iconTheme": "material-icon-theme",
  "workbench.layoutControl.enabled": false,
  "workbench.tree.enableStickyScroll": false,
  "workbench.tree.renderIndentGuides": "none",
  "workbench.tree.indent": 8,
  "workbench.editor.showTabs": "multiple",
  "workbench.editor.enablePreview": false,
  "breadcrumbs.enabled": false,

  // --- Explorer ---
  "explorer.confirmDelete": false,
  "explorer.expandSingleFolderWorkspaces": false,
  "explorer.compactFolders": false,

  // --- File Behavior ---
  "files.autoSave": "afterDelay",
  "files.associations": {
    "*.css": "tailwindcss"
  },

  // --- Terminal ---
  "terminal.integrated.cursorStyle": "line",
  "terminal.integrated.cursorStyleInactive": "line",
  "terminal.integrated.cursorBlinking": true,
  "terminal.integrated.accessibleViewPreserveCursorPosition": true,

  // --- Extensions ---
  // @ Prettier //
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  // @ ESLint //
  "eslint.format.enable": true,
  "eslint.useESLintClass": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": "explicit",
    "source.fixAll.eslint": "explicit",
    "source.fixAll.tslint": "explicit",
    "source.fixAll.markdownlint": "explicit",
    "source.organizeImports": "explicit"
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],

  // @ Tailwind IntelliSense //
  "editor.quickSuggestions": {
    "strings": "on"
  },

  // @ MarkdownLint //
  "[markdown]": {
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true
  },

  // @ CodeGeeX //
  "Codegeex.Privacy": false,
  "Codegeex.License": "",
  "Codegeex.Chat.LanguagePreference": "English",
  "Codegeex.Comment.LanguagePreference": "English",
  "Codegeex.SidebarUI.LanguagePreference": "English",
  "Codegeex.CommitMessage.LanguagePreference": "English",

  // @ Better Comments //
  "better-comments.multilineComments": true,
  "better-comments.highlightPlainText": false,
  "better-comments.tags": [
    { "tag": "!", "color": "#1dd1a1" },
    { "tag": "?", "color": "#ff6b6b" },
    { "tag": "//", "color": "#474747", "strikethrough": true },
    { "tag": "---", "color": "#81ecec", "bold": true },
    { "tag": "todo", "color": "#222f3e", "backgroundColor": "#1dd1a1" },
    { "tag": "@", "color": "#222f3e", "backgroundColor": "#bced54" },
    { "tag": "fix", "color": "#222f3e", "backgroundColor": "#ff6b6b" },
    { "tag": "*", "color": "#98C379" }
  ],

  // = = = Misc = = =
  "extensions.ignoreRecommendations": true,
  "cSpell.userWords": ["Monokai"],
  "window.menuBarVisibility": "compact",
  "window.titleBarStyle": "custom",
  "chat.commandCenter.enabled": false,
  "window.commandCenter": false,
  "glassit.alpha": 245,
  "markdown.preview.scrollPreviewWithEditor": true,
  "markdown.preview.scrollEditorWithPreview": true
}
```

## SETTING-2 (Minimal)

```json
{
  // --- Font Settings ---
  "editor.fontFamily": "'Operator Mono Lig Light', 'JetBrains Mono Light', monospace",
  "editor.fontLigatures": true,
  "editor.fontSize": 14,
  "editor.lineHeight": 16,
  "editor.inlayHints.enabled": "on",
  "editor.inlayHints.fontFamily": "'Operator Mono Lig Light', 'JetBrains Mono Light', monospace",
  "editor.formatOnPaste": true,
  "editor.formatOnType": true,
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "emmet.triggerExpansionOnTab": true,
  "editor.tabCompletion": "onlySnippets",

  // --- Cursor & Scrolling ---
  "editor.cursorBlinking": "expand",
  "editor.smoothScrolling": true,
  "editor.mouseWheelZoom": true,

  // --- Editor Behavior ---
  "editor.bracketPairColorization.enabled": true,
  "editor.linkedEditing": true,
  "editor.accessibilitySupport": "off",
  "editor.detectIndentation": false,
  "editor.indentSize": 4,
  "editor.links": false,
  "editor.minimap.enabled": false,
  "editor.minimap.renderCharacters": false,
  "editor.minimap.size": "fit",
  "editor.stickyScroll.enabled": false, // Optional

  // Token Style Overrides (Italicize useful tokens)
  "editor.tokenColorCustomizations": {
    "textMateRules": [
      {
        "scope": [
          "keyword",
          "keyword.control",
          "keyword.operator",
          "storage.type",
          "entity.name.function",
          "variable.language",
          "support.function",
          "meta.function-call",
          "meta.class",
          "meta.import",
          "string.quoted",
          "string.template",
          "constant.language",
          "markup.bold"
        ],
        "settings": {
          "fontStyle": "italic"
        }
      }
    ]
  },

  // --- Workbench UI ---
  "workbench.activityBar.location": "hidden",
  "workbench.statusBar.visible": false,
  "workbench.sideBar.location": "left",
  "workbench.tips.enabled": false,
  "workbench.startupEditor": "none",
  "workbench.colorTheme": "Dracula Theme",
  "workbench.iconTheme": "material-icon-theme",
  "workbench.layoutControl.enabled": false,
  "workbench.tree.enableStickyScroll": false,
  "workbench.tree.renderIndentGuides": "none",
  "workbench.tree.indent": 8,
  "workbench.editor.showTabs": "multiple",
  "breadcrumbs.enabled": false,

  // --- Explorer ---
  "explorer.confirmDelete": false,
  "explorer.expandSingleFolderWorkspaces": false,
  "explorer.compactFolders": false,

  // --- File Behavior ---
  "files.autoSave": "afterDelay",
  "files.associations": {
    "*.css": "tailwindcss"
  },

  // --- Terminal ---
  "terminal.integrated.cursorStyle": "line",
  "terminal.integrated.cursorStyleInactive": "line",
  "terminal.integrated.cursorBlinking": true,
  "terminal.integrated.accessibleViewPreserveCursorPosition": true,

  // --- Extensions ---
  // @ Prettier //
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  // @ ESLint //
  "eslint.format.enable": true,
  "eslint.useESLintClass": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": "explicit",
    "source.fixAll.eslint": "explicit",
    "source.fixAll.tslint": "explicit",
    "source.fixAll.markdownlint": "explicit",
    "source.organizeImports": "explicit"
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],

  // @ Tailwind IntelliSense //
  "editor.quickSuggestions": {
    "strings": "on"
  },

  // @ MarkdownLint //
  "[markdown]": {
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true
  },

  // @ CodeGeeX //
  "Codegeex.Privacy": false,
  "Codegeex.License": "",
  "Codegeex.Chat.LanguagePreference": "English",
  "Codegeex.Comment.LanguagePreference": "English",
  "Codegeex.SidebarUI.LanguagePreference": "English",
  "Codegeex.CommitMessage.LanguagePreference": "English",

  // @ Better Comments //
  "better-comments.multilineComments": true,
  "better-comments.highlightPlainText": false,
  "better-comments.tags": [
    { "tag": "!", "color": "#1dd1a1" },
    { "tag": "?", "color": "#ff6b6b" },
    { "tag": "//", "color": "#474747", "strikethrough": true },
    { "tag": "---", "color": "#81ecec", "bold": true },
    { "tag": "todo", "color": "#222f3e", "backgroundColor": "#1dd1a1" },
    { "tag": "@", "color": "#222f3e", "backgroundColor": "#bced54" },
    { "tag": "fix", "color": "#222f3e", "backgroundColor": "#ff6b6b" },
    { "tag": "*", "color": "#98C379" },
    { "tag": "=====", "color": "#F0F0F0", "backgroundColor": "#91009E" },
    { "tag": "====", "color": "#F0F0F0", "backgroundColor": "#91009E" },
    { "tag": "===", "color": "#F0F0F0", "backgroundColor": "#91009E" },
    { "tag": "==", "color": "#F0F0F0", "backgroundColor": "#91009E" }
  ],

  // = = = Misc = = =
  "extensions.ignoreRecommendations": true,
  "cSpell.userWords": ["Monokai", "Noreturn", "strcmp", "Woorank"],
  "window.menuBarVisibility": "compact",
  "window.titleBarStyle": "custom",
  "chat.commandCenter.enabled": false,
  "window.commandCenter": false,
  "glassit.alpha": 255,
  "git.openRepositoryInParentFolders": "never",
  "markdown.preview.scrollPreviewWithEditor": true,
  "markdown.preview.scrollEditorWithPreview": true
}
```

## FONTS (UBUNTU)

### Fira Code

To install **`Fira Code`** via the terminal on **Ubuntu Linux**, follow these steps:

Ubuntu includes **Fira Code** in its package repository. Run:

```bash
sudo apt update
sudo apt install fonts-firacode -y
```

After installation, refresh the font cache:

```bash
fc-cache -fv
```

**❏ Verify Installation**

Check if Fira Code is installed with:

```bash
fc-list | grep "Fira Code"
```

If installed correctly, you should see output like:

```bash
/usr/share/fonts/truetype/firacode/FiraCode-Regular.ttf: Fira Code
```

**❏ Set `Fira Code` in VS Code**

Click **Edit in settings.json** and add:

```json
"editor.fontFamily": "'Fira Code', monospace"
```

Restart **VS Code**.

**🎯 Bonus: Enable Ligatures (Optional)**

Fira Code supports **ligatures** (e.g., `!==` → `≠`). To enable them in **VS Code**, add this to `settings.json`:

```json
"editor.fontLigatures": true
```

### JetBrains Mono

Ubuntu provides JetBrains Mono in its official repository. Simply run:

```bash
sudo apt update
sudo apt install fonts-jetbrains-mono -y
```

After installation, refresh the font cache:

```bash
fc-cache -fv
```

**❏ Verify Installation**

Check if JetBrains Mono is installed with:

```bash
fc-list | grep "JetBrains Mono"
```

If installed correctly, you should see output like:

```bash
/home/user/.fonts/JetBrainsMono-Regular.ttf: JetBrains Mono
```

**❏ Set `JetBrains Mono` in VS Code**

Click **Edit in settings.json** and add:

```json
"editor.fontFamily": "'JetBrains Mono', monospace"
```

Restart **VS Code**.

**🎯 Bonus: Enable Ligatures (Optional)**

JetBrains Mono supports **ligatures** (e.g., `!==` → `≠`). To enable them in **VS Code**, add this to `settings.json`:

```json
"editor.fontLigatures": true
```

### Patrick Hand

To install **Patrick Hand** via the terminal on **Ubuntu Linux**, follow these steps:

```bash
wget -P ~/.fonts <https://github.com/google/fonts/raw/main/ofl/patrickhand/PatrickHand-Regular.ttf>
```

After installation, refresh the font cache:

```bash
fc-cache -fv
```

**Set ‘Patrick Hand’ in VS Code**

Click **Edit in settings.json** and add:

```json
"editor.fontFamily": "'JetBrains Mono', monospace"
```

## EXTENSION

### Prettier

**🚸🚸🚸 STEP 1: Install Prettier Plugin (Prettier, Publisher by Prettier)**

**🚸🚸🚸 STEP 2: Install Prettier in Your Project**

To use Prettier as a dependency in your project, install it via npm/yarn/pnpm:

```bash
// using NPM
npm install --save-dev --save-exact prettier

// using YARN
yarn add --dev --exact prettier

// using PNPM
pnpm add -D prettier
```

After installation, a file need to be generated in your root directory:

- `.prettierrc.json`

**🚸🚸🚸 STEP 3: Configure** `.prettierrc.json` file by some rules

Below is an example **Prettier** configuration for a JavaScript project:

```json
{
    "printWidth": 80,
    "tabWidth": 4,
    "useTabs": false,
    "semi": true,
    "singleQuote": true,
    "trailingComma": "es5",
    "bracketSpacing": true,
    "bracketSameLine": true,
    "arrowParens": "avoid"
}
```

**🚸🚸🚸 STEP 4: Set Prettier as Auto Formatting and Default Formatter in VS Code**

- Open **VS Code Settings** (`Ctrl+,`).
- Search for **"default formatter"**.
- Select **Prettier - Code formatter** as the default formatter for:
    - JavaScript
    - TypeScript
    - JSON
    - CSS, SCSS, Less

Alternatively, modify `.vscode/settings.json`:

```json
{
    // Extension: ===== Prettier =====
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true,
    "editor.formatOnType": true,
    "[json]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.formatOnSave": true
    },
    "[javascriptreact]": {
        "editor.formatOnSave": true
    },

    // Extension: ===== ESLint =====
    "eslint.format.enable": true,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": "explicit"
    },
    "eslint.alwaysShowStatus": true,
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "typescript",
        "typescriptreact"
    ]
}
```

**🚸🚸🚸 STEP 5: Run Prettier Manually** - - - ❌❌❌ **Not Recommended**

If you need to manually format files, use:

```bash
npx prettier --write .
```

For specific file types:

```bash
npx prettier --write "**/*.js"
npx prettier --write "**/*.ts"
```

**🚸🚸🚸 STEP 6: Fix Prettier and ESLint Conflicts** - - - ❌❌❌ **Not Recommended**

If you are using ESLint, install the ESLint Prettier plugin:

```bash
npm install --save-dev eslint-config-prettier eslint-plugin-prettier
```

Then, update `.eslintrc.json`:

```json
{
  "extends": ["eslint:recommended", "plugin:prettier/recommended"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

**🚸🚸🚸 (Optional) Ignore Files from Prettier**

Create a **`.prettierignore`** file to exclude files:

```
node_modules/
dist/
build/
public/
```

**🚸🚸🚸 Verify Prettier is Working**

- Open a file and introduce inconsistent formatting.
- Save the file (`Ctrl+S`).
- The formatting should be fixed automatically.

**⛔ Troubleshooting**

If Prettier is not formatting, check:

- **Extensions**: Ensure "Prettier - Code formatter" is installed.
- **Default Formatter**: Make sure Prettier is set as the default formatter.
- **Settings Conflicts**: Verify there are no conflicting formatters in `.vscode/settings.json`.
- **Check Prettier Output**: Open the **Output** panel (`Ctrl+Shift+U`) and select **Prettier** to see logs.

### ESLint

**🚸🚸🚸 STEP 1: Install ESLint Plugin (ESLint, Publisher by Microsoft)**

**🚸🚸🚸 STEP 2: Install ESLint in Your Project**

Navigate to your project directory and run the following command to install ESLint locally:

```bash
npm install eslint --save-dev

// For TypeScript projects, also install:
npm install @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev

// For React projects, also install:
npm install eslint-plugin-react eslint-plugin-react-hooks --save-dev

// For Next.js projects, also install:
npm install eslint-config-next --save-dev
```

**🚸🚸🚸 STEP 3: Initialize ESLint Configuration**

Run the following command to create an ESLint config file:

```bash
npx eslint --init
```

After Initialize, this will generate one of the following files in your root directory:

- `.eslintrc.json`

**🚸🚸🚸 STEP 4: Configure** `.eslintrc.json` file.

Below is an example ESLint configuration for a JavaScript project: (React + React Hooks + TypeScript)

```json
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:prettier/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "plugins": ["react", "@typescript-eslint", "eslint-plugin-react-compiler"],
    "rules": {
        // ⚠️⚠️⚠️ Notes: Key Value -> off/0, warning/1, error/2
        "indent": ["error", 2],
        "quotes": ["error", "double"],
        "semi": ["error", "always"],
        "no-unused-vars": "warn",
        "react/prop-types": "off",
        "@typescript-eslint/no-unused-vars": [
            "warn",
            { "vars": "all", "args": "after-used", "ignoreRestSiblings": false }
        ],
        "react-compiler/react-compiler": "error",
        "prettier/prettier": "error"
        // "no-alert": "error",
        // "no-console": "warn",
        // "camelcase": "off",
        // "no-var": "warn", // No Var
        // "no-empty": "error", // deprecated
        // "no-unreadable": "off", // Unreadable Code
        // "eqeqeq": "error",  // Equal Equal (==)
        // "max-lines":["error", 3], // Max Lines
        // "max-depth": ["error", 3] // Max Depth of Nesting
    }
}
```

**🚸🚸🚸 STEP 5: Add ESLint Script to** `package.json` file. - - - ❌❌❌ Not Recommended

In your `package.json`, add the following script:

```json
"scripts": {
  "lint": "eslint . --ext .js,.jsx,.ts,.tsx"
}
```

Run ESLint manually using:

```bash
npm run lint
```

To automatically fix issues:

```bash
npm run lint -- --fix
```

**🚸🚸🚸 STEP 6: Enable Auto-Fix on Save in VS Code**

1. Open **VS Code Settings** (`Ctrl+,`).
2. Search for `"eslint"` and enable:
    - **ESLint: Enable**
    - **ESLint › Format: Enable**
    - **Editor: Format On Save**.
3. Alternatively, add the following to `.vscode/settings.json`:

```json
{
    // Extension: ===== Prettier =====
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true,
    "editor.formatOnType": true,
    "[json]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.formatOnSave": true
    },
    "[javascriptreact]": {
        "editor.formatOnSave": true
    },

    // Extension: ===== ESLint =====
    "eslint.format.enable": true,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": "explicit"
    },
    "eslint.alwaysShowStatus": true,
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "typescript",
        "typescriptreact"
    ]
}
```

**🔳🔳🔳 (Optional) Ignore Files from ESLint**

Create a `.eslintignore` file to exclude files:

```
node_modules/
dist/
build/
public/
```

**🔳🔳🔳 Verify ESLint is Working**

- Open a JavaScript/TypeScript file and introduce an intentional error.
- ESLint should underline issues in red.
- If auto-fix is enabled, saving the file should correct the issue.