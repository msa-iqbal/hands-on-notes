
Code snippets are templates that make it easier to enter repeating code patterns, such as loops or conditional statements.

- VS Code Editor (Setting Path): **`File > Preferences > Configure User Snippets`**
- VS Code Shortcut: The Extensions view (**`Ctrl+Shift+X`**) using the @category:"snippets" filter.

> Content Ref.: [https://code.visualstudio.com/docs/editor/userdefinedsnippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets)

Below is an example custom configuration:

```jsx
{
  // Snippet for 'Console Log()'
  // ===========================
  "Print to console": {
   "prefix": "log",
   "body": [
   "console.log($TM_SELECTED_TEXT);",
      "$2"
     ],
     "description": "Log output to console"
   }
}
```

## MARKDOWN

- Open “VS Code Editor”
- Press `Ctrl + Shift + P`
- Type: **Configure User Snippets**
- Select: **`markdown.json`**

```json
{
  // ===========================
  // Headings
  // ===========================
  "Heading-1": {
    "prefix": "h",
    "body": [
      "# $1"
    ],
    "description": "H1 heading"
  },

  "Heading-2": {
    "prefix": "hh",
    "body": [
      "## 🗁 $1"
    ],
    "description": "H2 heading"
  },

  "Heading-3": {
    "prefix": "hhh",
    "body": [
      "### ၊၊||၊ $1"
    ],
    "description": "H3 heading"
  },

  "Heading-4": {
    "prefix": "hhhh",
    "body": [
      "#### ⧉⧉⧉ $1"
    ],
    "description": "H4 heading"
  },

  "Conclusion": {
    "prefix": "c",
    "body": [
      "## ☰ $1"
    ],
    "description": "Conclusion section"
  },

  // ===========================
  // Text Formatting
  // ===========================
  "Bold-Title": {
    "prefix": "b",
    "body": [
      "**❏❏❏ $1**"
    ],
    "description": "Bold title"
  },

  "Example": {
    "prefix": "e",
    "body": [
      "🕮 Example: $1"
    ],
    "description": "Example label"
  },

  // ===========================
  // Code Blocks
  // ===========================
  "Syntax Plaintext Block": {
    "prefix": "s",
    "body": [
      "Syntax:",
      "",
      "```plaintext",
      "$1",
      "```"
    ],
    "description": "Syntax block"
  },

  "Plaintext": {
    "prefix": "pt",
    "body": [
      "```plaintext",
      "$1",
      "```"
    ],
    "description": "Plain text code block"
  },

  "Markdown": {
    "prefix": "ma",
    "body": [
      "```markdown",
      "$1",
      "```"
    ],
    "description": "Markdown code block"
  },

  "HTML": {
    "prefix": "ht",
    "body": [
      "```html",
      "$1",
      "```"
    ],
    "description": "HTML code block"
  },

  "CSS": {
    "prefix": "cs",
    "body": [
      "```css",
      "$1",
      "```"
    ],
    "description": "CSS code block"
  },

  "JavaScript": {
    "prefix": "js",
    "body": [
      "```javascript",
      "$1",
      "```"
    ],
    "description": "JavaScript code block"
  },

  "TypeScript": {
    "prefix": "ts",
    "body": [
      "```typescript",
      "$1",
      "```"
    ],
    "description": "TypeScript code block"
  },

  "Linux": {
    "prefix": "li",
    "body": [
      "```bash",
      "$1",
      "```"
    ],
    "description": "Linux/bash code block"
  },

  "Windows": {
    "prefix": "ps",
    "body": [
      "```powershell",
      "$1",
      "```"
    ],
    "description": "PowerShell code block"
  },

  // ===========================
  // Callouts / Alerts
  // ===========================
  "NOTE": {
    "prefix": "n",
    "body": [
      "> [!NOTE]",
      "> $1"
    ],
    "description": "Note callout"
  },

  "TIP": {
    "prefix": "t",
    "body": [
      "> [!TIP]",
      "> $1"
    ],
    "description": "Tip callout"
  },

  "IMPORTANT": {
    "prefix": "i",
    "body": [
      "> [!IMPORTANT]",
      "> $1"
    ],
    "description": "Important callout"
  },

  "WARNING": {
    "prefix": "w",
    "body": [
      "> [!WARNING]",
      "> $1"
    ],
    "description": "Warning callout"
  },

  "CAUTION": {
    "prefix": "ca",
    "body": [
      "> [!CAUTION]",
      "> $1"
    ],
    "description": "Caution callout"
  }
}
```

## JAVASCRIPT

- Open “VS Code Editor”
- Press `Ctrl + Shift + P`
- Type: **Configure User Snippets**
- Select: **`javascript.json`**

```json
{
  // ===========================
  // Debugging
  // ===========================
  "Console Log": {
    "prefix": "log",
    "body": [
      "// Debug: logs variable with label",
      "console.log('$1:', $1);"
    ],
    "description": "Log variable with label"
  },

  // ===========================
  // Functions
  // ===========================
  "Arrow Function": {
    "prefix": "af",
    "body": [
      "// Arrow function (clean & modern)",
      "const $1 = ($2) => {",
      "  // logic here",
      "  $3",
      "};"
    ],
    "description": "Arrow function"
  },

  "Async Function": {
    "prefix": "asyncf",
    "body": [
      "// Async function with error handling",
      "const $1 = async ($2) => {",
      "  try {",
      "    // await async code here",
      "    $3",
      "  } catch (error) {",
      "    // handle error safely",
      "    console.error(error);",
      "  }",
      "};"
    ],
    "description": "Async function with try/catch"
  },

  // ===========================
  // API & Async
  // ===========================
  "Fetch API": {
    "prefix": "fetch",
    "body": [
      "// Fetch data from API",
      "const fetchData = async () => {",
      "  try {",
      "    const res = await fetch('$1'); // API URL",
      "    const data = await res.json(); // convert to JSON",
      "    console.log(data); // inspect result",
      "  } catch (error) {",
      "    console.error(error); // handle error",
      "  }",
      "};"
    ],
    "description": "Fetch API template"
  },

  // ===========================
  // Conditions
  // ===========================
  "If Condition": {
    "prefix": "if",
    "body": [
      "// Basic condition check",
      "if ($1) {",
      "  $2",
      "}"
    ],
    "description": "If statement"
  },

  "If Else": {
    "prefix": "ife",
    "body": [
      "// Conditional branching",
      "if ($1) {",
      "  $2 // true case",
      "} else {",
      "  $3 // false case",
      "}"
    ],
    "description": "If else statement"
  },

  // ===========================
  // Loops & Iteration
  // ===========================
  "For Loop": {
    "prefix": "for",
    "body": [
      "// Classic loop (index-based)",
      "for (let i = 0; i < $1; i++) {",
      "  $2",
      "}"
    ],
    "description": "For loop"
  },

  "ForEach Loop": {
    "prefix": "foreach",
    "body": [
      "// Loop through array items",
      "$1.forEach(($2) => {",
      "  $3",
      "});"
    ],
    "description": "Array forEach loop"
  },

  "Map Function": {
    "prefix": "map",
    "body": [
      "// Transform array into new array",
      "const $1 = $2.map(($3) => {",
      "  return $4; // return new value",
      "});"
    ],
    "description": "Array map"
  },

  // ===========================
  // Objects & Utilities
  // ===========================
  "Destructure Object": {
    "prefix": "dob",
    "body": [
      "// Extract values from object",
      "const { $1 } = $2;"
    ],
    "description": "Object destructuring"
  },

  "Set Timeout": {
    "prefix": "timeout",
    "body": [
      "// Run code after delay (ms)",
      "setTimeout(() => {",
      "  $1",
      "}, $2);"
    ],
    "description": "setTimeout"
  },

  "Try Catch": {
    "prefix": "try",
    "body": [
      "// Error handling block",
      "try {",
      "  $1",
      "} catch (error) {",
      "  console.error(error); // log error",
      "}"
    ],
    "description": "Try catch block"
  }
}
```

## TYPESCRIPT

- Open “VS Code Editor”
- Press `Ctrl + Shift + P`
- Type: **Configure User Snippets**
- Select: `typescript**.json**`

```bash
{
  // ===========================
  // Types & Interfaces
  // ===========================
  "Interface": {
    "prefix": "int",
    "body": [
      "// Define object shape using interface",
      "interface $1 {",
      "  $2",
      "}"
    ],
    "description": "Create TypeScript interface"
  },

  "Type Alias": {
    "prefix": "type",
    "body": [
      "// Create reusable type",
      "type $1 = {",
      "  $2",
      "};"
    ],
    "description": "Type alias"
  },

  // ===========================
  // Functions
  // ===========================
  "Typed Function": {
    "prefix": "tf",
    "body": [
      "// Function with parameter & return types",
      "const $1 = ($2: $3): $4 => {",
      "  $5",
      "};"
    ],
    "description": "Typed function"
  },

  "Async Function TS": {
    "prefix": "asyncf",
    "body": [
      "// Async function with typed return",
      "const $1 = async ($2: $3): Promise<$4> => {",
      "  try {",
      "    $5",
      "  } catch (error) {",
      "    console.error(error);",
      "  }",
      "};"
    ],
    "description": "Async typed function"
  },

  // ===========================
  // Variables & Generics
  // ===========================
  "Typed Variable": {
    "prefix": "tv",
    "body": [
      "// Strongly typed variable",
      "const $1: $2 = $3;"
    ],
    "description": "Typed variable"
  },

  "Generic Function": {
    "prefix": "gen",
    "body": [
      "// Generic function (flexible types)",
      "function $1<T>($2: T): T {",
      "  return $2;",
      "}"
    ],
    "description": "Generic function"
  },

  // ===========================
  // Arrays & Objects
  // ===========================
  "Typed Array": {
    "prefix": "arr",
    "body": [
      "// Array with type",
      "const $1: $2[] = [$3];"
    ],
    "description": "Typed array"
  },

  "Readonly Object": {
    "prefix": "ro",
    "body": [
      "// Readonly object (immutable)",
      "const $1: Readonly<$2> = {",
      "  $3",
      "};"
    ],
    "description": "Readonly object"
  },

  // ===========================
  // Utility Types
  // ===========================
  "Partial Type": {
    "prefix": "partial",
    "body": [
      "// Make all properties optional",
      "type $1 = Partial<$2>;"
    ],
    "description": "Partial utility type"
  },

  "Pick Type": {
    "prefix": "pick",
    "body": [
      "// Pick specific properties",
      "type $1 = Pick<$2, '$3'>;"
    ],
    "description": "Pick utility type"
  },

  "Omit Type": {
    "prefix": "omit",
    "body": [
      "// Remove properties from type",
      "type $1 = Omit<$2, '$3'>;"
    ],
    "description": "Omit utility type"
  },

  // ===========================
  // Enums
  // ===========================
  "Enum": {
    "prefix": "enum",
    "body": [
      "// Define fixed set of values",
      "enum $1 {",
      "  $2",
      "}"
    ],
    "description": "Enum"
  },

  // ===========================
  // Classes
  // ===========================
  "Class": {
    "prefix": "class",
    "body": [
      "// TypeScript class with constructor",
      "class $1 {",
      "  constructor($2) {",
      "    $3",
      "  }",
      "}"
    ],
    "description": "Class"
  },

  // ===========================
  // Error Handling
  // ===========================
  "Try Catch TS": {
    "prefix": "try",
    "body": [
      "// Error handling",
      "try {",
      "  $1",
      "} catch (error) {",
      "  console.error(error);",
      "}"
    ],
    "description": "Try catch block"
  }
}
```

## REACT (WITH JAVASCRIPT)

- Open “VS Code Editor”
- Press `Ctrl + Shift + P`
- Type: **Configure User Snippets**
- Select: **`javascriptreact.json`**

```json
{
  // ===========================
  // Functional Components
  // ===========================
  "React Functional Component": {
    "prefix": "rfc",
    "body": [
      "// Basic React functional component",
      "const $1 = () => (",
      "  <div>",
      "    $2",
      "  </div>",
      ");",
      "",
      "export default $1;"
    ],
    "description": "Create a React functional component"
  },

  "React Arrow Component": {
    "prefix": "rac",
    "body": [
      "// React arrow function component",
      "const $1 = ($2) => {",
      "  return (",
      "    <div>$3</div>",
      "  );",
      "};",
      "",
      "export default $1;"
    ],
    "description": "React arrow component"
  },

  // ===========================
  // React Hooks
  // ===========================
  "useState Hook": {
    "prefix": "us",
    "body": [
      "// useState hook for local state",
      "const [$1, set$1] = useState($2);"
    ],
    "description": "React useState hook"
  },

  "useEffect Hook": {
    "prefix": "ue",
    "body": [
      "// useEffect hook for side effects",
      "useEffect(() => {",
      "  $1",
      "}, [$2]);"
    ],
    "description": "React useEffect hook"
  },

  "useRef Hook": {
    "prefix": "ur",
    "body": [
      "// useRef for persistent mutable reference",
      "const $1 = useRef($2);"
    ],
    "description": "React useRef hook"
  },

  "useContext Hook": {
    "prefix": "uc",
    "body": [
      "// useContext to consume context",
      "const $1 = useContext($2);"
    ],
    "description": "React useContext hook"
  },

  // ===========================
  // Event Handlers
  // ===========================
  "onClick Handler": {
    "prefix": "oc",
    "body": [
      "// Click handler",
      "const handleClick = () => {",
      "  $1",
      "};"
    ],
    "description": "onClick event handler"
  },

  "onChange Handler": {
    "prefix": "och",
    "body": [
      "// Input change handler",
      "const handleChange = (e) => {",
      "  const { value } = e.target;",
      "  $1",
      "};"
    ],
    "description": "onChange event handler"
  },

  // ===========================
  // JSX & Rendering
  // ===========================
  "JSX Fragment": {
    "prefix": "frag",
    "body": [
      "<>",
      "  $1",
      "</>"
    ],
    "description": "JSX fragment shorthand"
  },

  "Conditional Rendering": {
    "prefix": "cond",
    "body": [
      "{ $1 ? (",
      "  $2",
      ") : (",
      "  $3",
      ") }"
    ],
    "description": "Ternary conditional rendering"
  },

  "Map JSX": {
    "prefix": "mapjsx",
    "body": [
      "{ $1.map(($2, index) => (",
      "  <div key={index}>",
      "    $3",
      "  </div>",
      ")) }"
    ],
    "description": "Render list with map"
  }
}
```

## REACT (WITH TYPESCRIPT)

- Open “VS Code Editor”
- Press `Ctrl + Shift + P`
- Type: **Configure User Snippets**
- Select: **`typescriptreact.json`**

```json
{
  // ===========================
  // Functional Components
  // ===========================
  "React Functional Component TS": {
    "prefix": "rfc",
    "body": [
      "// Basic typed React functional component",
      "import React from 'react';",
      "",
      "interface $1Props {",
      "  $2",
      "}",
      "",
      "const $1: React.FC<$1Props> = ({ $3 }) => (",
      "  <div>",
      "    $4",
      "  </div>",
      ");",
      "",
      "export default $1;"
    ],
    "description": "Typed React functional component"
  },

  "Arrow Component TS": {
    "prefix": "rac",
    "body": [
      "// Arrow function component with types",
      "import React from 'react';",
      "",
      "interface $1Props {",
      "  $2",
      "}",
      "",
      "const $1 = ({ $3 }: $1Props) => {",
      "  return (",
      "    <div>$4</div>",
      "  );",
      "};",
      "",
      "export default $1;"
    ],
    "description": "Typed React arrow component"
  },

  // ===========================
  // Hooks
  // ===========================
  "useState TS": {
    "prefix": "us",
    "body": [
      "// useState hook with type",
      "const [$1, set$1] = React.useState<$2>($3);"
    ],
    "description": "Typed useState hook"
  },

  "useEffect TS": {
    "prefix": "ue",
    "body": [
      "// useEffect hook",
      "React.useEffect(() => {",
      "  $1",
      "}, [$2]);"
    ],
    "description": "Typed useEffect hook"
  },

  "useRef TS": {
    "prefix": "ur",
    "body": [
      "// useRef with type",
      "const $1 = React.useRef<$2>($3);"
    ],
    "description": "Typed useRef hook"
  },

  "useContext TS": {
    "prefix": "uc",
    "body": [
      "// useContext typed",
      "const $1 = React.useContext<$2>($3);"
    ],
    "description": "Typed useContext hook"
  },

  // ===========================
  // Event Handlers
  // ===========================
  "onClick TS": {
    "prefix": "oc",
    "body": [
      "// Click handler with MouseEvent type",
      "const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {",
      "  $1",
      "};"
    ],
    "description": "onClick event handler"
  },

  "onChange TS": {
    "prefix": "och",
    "body": [
      "// Input change handler with type",
      "const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {",
      "  const { value } = e.target;",
      "  $1",
      "};"
    ],
    "description": "onChange event handler"
  },

  // ===========================
  // JSX Patterns
  // ===========================
  "JSX Fragment TS": {
    "prefix": "frag",
    "body": [
      "<>",
      "  $1",
      "</>"
    ],
    "description": "JSX fragment shorthand"
  },

  "Conditional Rendering TS": {
    "prefix": "cond",
    "body": [
      "{ $1 ? (",
      "  $2",
      ") : (",
      "  $3",
      ") }"
    ],
    "description": "Ternary conditional rendering"
  },

  "Map JSX TS": {
    "prefix": "mapjsx",
    "body": [
      "{ $1.map(($2: $3, index: number) => (",
      "  <div key={index}>",
      "    $4",
      "  </div>",
      ")) }"
    ],
    "description": "Render list with map and types"
  }
}
```
