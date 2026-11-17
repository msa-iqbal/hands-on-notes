# Global Augmentation

Global augmentation is a TypeScript feature that allows you to extend declarations in the global scope from within a module.

It is useful when you need to add custom types or properties to globally available objects without modifying their original declarations.

Common use cases include:

* Extending the global `Window` interface.
* Adding custom properties to the `globalThis` object.
* Extending built-in JavaScript interfaces.
* Adding global application configuration types.
* Integrating custom properties into browser and Node.js environments.

Global augmentation uses the `declare global` syntax.

# 🗁 Basic Global Augmentation

Consider an application that needs a custom property on the browser's `Window` object.

You can extend the existing `Window` interface:

```typescript
// types/global.d.ts

export {};

declare global {
  interface Window {
    appVersion: string;
  }
}
```

Now TypeScript recognizes the additional property:

```typescript
// app.ts

window.appVersion = "1.0.0";

console.log(window.appVersion);
```

Without the augmentation, TypeScript would report that `appVersion` does not exist on the `Window` interface.

The `export {}` statement makes the declaration file an external module, allowing `declare global` to augment the global scope.

# 🗁 Understanding declare global

The `declare global` syntax allows declarations inside a module to contribute to the global scope.

```typescript
export {};

declare global {
  interface SomeGlobalInterface {
    property: string;
  }
}
```

The declarations inside the block become available globally within the TypeScript compilation context.

Unlike module augmentation, global augmentation does not target a specific external module.

# 🗁 Extending the Window Interface

Browser applications frequently need custom properties on `window`.

For example, an application may expose a runtime configuration object.

```typescript
// types/window.d.ts

export {};

declare global {
  interface Window {
    appConfig: {
      apiUrl: string;
      environment: "development" | "production";
      debug: boolean;
    };
  }
}
```

Usage:

```typescript
const apiUrl = window.appConfig.apiUrl;

console.log(apiUrl);
```

TypeScript now understands the structure of `window.appConfig`.

However, the application must still initialize the object at runtime:

```typescript
window.appConfig = {
  apiUrl: "https://api.example.com",
  environment: "production",
  debug: false,
};
```

The declaration only provides compile-time type information.

# 🗁 Extending globalThis

`globalThis` provides a standard way to access the global object across JavaScript environments.

You can extend its type using global augmentation.

```typescript
// types/global.d.ts

export {};

declare global {
  var applicationName: string;
}
```

Usage:

```typescript
globalThis.applicationName = "My Application";

console.log(globalThis.applicationName);
```

The `var` declaration is used here to describe a global variable binding.

As with other global augmentations, declaring the type does not automatically create the runtime variable.

# 🗁 Adding Global Utility Types

Global augmentation can also introduce custom type declarations.

```typescript
// types/global.d.ts

export {};

declare global {
  type Environment = "development" | "staging" | "production";

  interface AppMetadata {
    name: string;
    version: string;
  }
}
```

These types can now be used without importing them:

```typescript
const environment: Environment = "development";

const metadata: AppMetadata = {
  name: "My Application",
  version: "1.0.0",
};
```

Global utility types can be convenient, but excessive use may create naming conflicts and make dependencies less explicit.

# 🗁 Extending Built-in Interfaces

Global augmentation can extend built-in JavaScript interfaces.

For example, you can add a custom method to the `String` interface.

```typescript
// types/string.d.ts

export {};

declare global {
  interface String {
    toTitleCase(): string;
  }
}
```

TypeScript now recognizes the method:

```typescript
const message = "hello world";

message.toTitleCase();
```

However, the method does not exist at runtime until you implement it.

```typescript
String.prototype.toTitleCase = function (): string {
  return this.toLowerCase()
    .replace(/\b\w/g, (character) => character.toUpperCase());
};
```

Now the method can be executed:

```typescript
console.log("hello world".toTitleCase());
// Hello World
```

The type declaration and runtime implementation serve different purposes.

# 🗁 Global Augmentation in Node.js

Node.js applications can use global augmentation to describe custom properties on the Node.js global object.

For example:

```typescript
// types/node-global.d.ts

export {};

declare global {
  var requestId: string | undefined;
}
```

Usage:

```typescript
globalThis.requestId = "req-123";

console.log(globalThis.requestId);
```

Node.js-specific global types may require the `@types/node` package, depending on the project configuration.

# 🗁 Global Augmentation vs Module Augmentation

Both features extend existing declarations, but they target different scopes.

| Feature | Global augmentation | Module augmentation |
| --- | --- | --- |
| Purpose | Extends global declarations | Extends declarations from a specific module |
| Syntax | `declare global` | `declare module` |
| Target | Global scope | Existing module |
| Common use | `Window`, `globalThis`, built-in interfaces | Third-party library interfaces |
| Import requirement | No import needed for augmented global declarations | The augmented module is imported normally |

**Global augmentation:**

```typescript
export {};

declare global {
  interface Window {
    appVersion: string;
  }
}
```

**Module augmentation:**

```typescript
import "some-library";

declare module "some-library" {
  interface User {
    role: string;
  }
}
```

# 🗁 Global Augmentation vs Ambient Declarations

Global augmentation and ambient declarations are related but serve different purposes.

An ambient declaration describes an existing JavaScript value or API that TypeScript cannot otherwise understand.

```typescript
// types/legacy-library.d.ts

declare function legacyFunction(value: string): void;
```

Global augmentation extends declarations that already exist in the global type environment.

```typescript
// types/global.d.ts

export {};

declare global {
  interface Window {
    appVersion: string;
  }
}
```

Use ambient declarations to describe existing untyped APIs and global augmentation to extend existing global declarations.

# 🗁 Organizing Global Declaration Files

A dedicated directory helps keep global declarations organized.

```text
src/
├── app.ts
├── types/
│   ├── global.d.ts
│   ├── window.d.ts
│   └── string.d.ts
└── services/
    └── api.ts
```

Make sure TypeScript includes the declaration files.

```json
{
  "compilerOptions": {
    "strict": true
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.d.ts"
  ]
}
```

If a declaration file is excluded from the compilation context, its global augmentation will not be available.

# 🗁 Common Mistakes

**Forgetting to make the declaration file a module**

```typescript
// types/global.d.ts

export {};

declare global {
  interface Window {
    appVersion: string;
  }
}
```

The `export {}` ensures the file is treated as a module.

**Assuming declarations create runtime properties**

```typescript
declare global {
  interface Window {
    appVersion: string;
  }
}
```

This only changes TypeScript's understanding of `window`. The application must assign the property at runtime.

**Creating unnecessary global names**

Global declarations are available throughout the compilation context. Generic names can conflict with other libraries or application code.

Prefer descriptive names and module-scoped types when global access is unnecessary.

**Extending built-in prototypes without implementation**

Adding a method to `String` or `Array` through an interface declaration does not implement that method. A corresponding runtime implementation is required.

# 🗁 Best Practices

* Use global augmentation only when a declaration genuinely belongs in the global scope.
* Prefer module-scoped types when global access is unnecessary.
* Place global declarations in dedicated `.d.ts` files.
* Use `export {}` when using `declare global` in a declaration file.
* Ensure declaration files are included in the TypeScript compilation context.
* Keep runtime implementations separate from type declarations.
* Avoid generic global names that may conflict with dependencies.
* Be careful when extending built-in JavaScript prototypes.
* Document custom global APIs and their initialization requirements.

# 🗁 Key Takeaways

* Global augmentation extends declarations in the global scope.
* It uses the `declare global` syntax.
* It is commonly used to extend `Window`, `globalThis`, and built-in interfaces.
* Global declarations can be added from within modules.
* `export {}` makes a declaration file an external module.
* Global augmentation changes compile-time type information, not runtime behavior.
* Runtime properties and methods must be implemented separately.
* Use global augmentation sparingly to avoid unnecessary global coupling.
