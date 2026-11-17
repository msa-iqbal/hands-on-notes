# Large Project Patterns

Large project patterns are architectural practices used to organize, maintain, and scale TypeScript applications as their codebases grow.

A small application can often work with a few source files and a simple `tsconfig.json`. However, larger applications require deliberate organization to manage dependencies, shared types, compilation, testing, and development workflows.

Common challenges include:

* Managing multiple applications and packages.
* Sharing types without creating tight coupling.
* Controlling module dependencies.
* Maintaining consistent compiler configurations.
* Organizing domain-specific functionality.
* Improving build and type-checking performance.
* Supporting multiple developers and teams.

TypeScript provides features such as project references, declaration files, module resolution, and strict type checking to help address these challenges.

# 🗁 Organize Code by Feature or Domain

A large application should be organized around meaningful features or business domains rather than placing every file into generic folders.

A feature-oriented structure:

```text
src/
├── features/
│   ├── users/
│   │   ├── user.types.ts
│   │   ├── user.service.ts
│   │   ├── user.repository.ts
│   │   ├── user.controller.ts
│   │   └── index.ts
│   ├── products/
│   │   ├── product.types.ts
│   │   ├── product.service.ts
│   │   ├── product.repository.ts
│   │   ├── product.controller.ts
│   │   └── index.ts
│   └── orders/
│       ├── order.types.ts
│       ├── order.service.ts
│       ├── order.repository.ts
│       ├── order.controller.ts
│       └── index.ts
├── shared/
│   ├── types/
│   ├── utils/
│   └── errors/
└── main.ts
```

Each feature owns its related types and implementation.

Benefits include:

* Clear feature boundaries.
* Easier maintenance.
* Reduced unrelated dependencies.
* Better team collaboration.
* Easier testing and refactoring.

# 🗁 Separate Application Layers

Layered architecture separates responsibilities into different parts of an application.

A typical backend application might use:

```text
src/
├── controllers/
├── services/
├── repositories/
├── models/
├── types/
└── main.ts
```

Each layer has a distinct responsibility.

| Layer | Responsibility |
| --- | --- |
| Controller | Handles incoming requests and responses |
| Service | Implements business logic |
| Repository | Handles data access |
| Model | Represents domain or persistence structures |
| Types | Defines shared contracts |

Example:

```typescript
// user.types.ts

export interface User {
  id: number;
  name: string;
  email: string;
}
```

```typescript
// user.repository.ts

import type { User } from "./user.types";

export class UserRepository {
  private users: User[] = [];

  findById(id: number): User | undefined {
    return this.users.find((user) => user.id === id);
  }

  save(user: User): void {
    this.users.push(user);
  }
}
```

```typescript
// user.service.ts

import { UserRepository } from "./user.repository";

export class UserService {
  constructor(
    private readonly repository: UserRepository,
  ) {}

  getUser(id: number) {
    return this.repository.findById(id);
  }
}
```

The service depends on the repository rather than directly implementing data storage logic.

For larger systems, interfaces or other abstractions can be introduced to reduce coupling between layers.

# 🗁 Use Shared Types Carefully

Large applications often need types shared across multiple features.

Create a dedicated location for genuinely shared contracts.

```text
src/
├── shared/
│   ├── types/
│   │   ├── api-response.ts
│   │   ├── pagination.ts
│   │   └── identifiers.ts
│   ├── errors/
│   └── utils/
└── features/
```

Example:

```typescript
// shared/types/api-response.ts

export interface ApiResponse<T> {
  data: T;
  success: boolean;
  message?: string;
}
```

Usage:

```typescript
import type { ApiResponse } from "../../shared/types/api-response";

interface User {
  id: number;
  name: string;
}

const response: ApiResponse<User> = {
  data: {
    id: 1,
    name: "Alice",
  },
  success: true,
};
```

Avoid turning the shared directory into a collection of unrelated application types.

A type should be shared when multiple independent parts of the application genuinely depend on the same contract.

# 🗁 Use Barrel Files Strategically

Barrel files expose multiple modules through a single entry point, usually an `index.ts` file.

Example:

```text
users/
├── user.types.ts
├── user.service.ts
├── user.repository.ts
└── index.ts
```

The barrel file:

```typescript
// users/index.ts

export type { User } from "./user.types";
export { UserService } from "./user.service";
export { UserRepository } from "./user.repository";
```

Consumers can import from the feature entry point:

```typescript
import {
  UserService,
  UserRepository,
  type User,
} from "./users";
```

Benefits:

* Cleaner import statements.
* A clear public API for a feature.
* Easier control over exported members.

However, excessive barrel files can introduce circular dependencies and make dependency relationships less obvious.

Prefer explicit internal imports when they improve dependency clarity.

# 🗁 Define Public and Internal APIs

Not every implementation detail should be accessible to other parts of an application.

A feature should expose only the functionality that external consumers need.

For example:

```typescript
// users/index.ts

export type { User } from "./user.types";
export { UserService } from "./user.service";
```

Internal implementation files can remain unexported:

```text
users/
├── index.ts
├── user.types.ts
├── user.service.ts
├── user.repository.ts
└── user.mapper.ts
```

Other features should depend on the public entry point rather than reaching into internal files.

This reduces coupling and makes refactoring easier.

# 🗁 Use Strict Type Checking

Strict type checking helps detect potential problems early.

A recommended starting point:

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitOverride": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true
  }
}
```

Important options:

| Option | Purpose |
| --- | --- |
| `strict` | Enables the strict type-checking family |
| `noImplicitOverride` | Requires explicit `override` for overridden methods |
| `noFallthroughCasesInSwitch` | Detects unintended switch fallthrough |
| `noUncheckedIndexedAccess` | Adds `undefined` to unchecked indexed access results |
| `noImplicitReturns` | Detects functions with inconsistent return paths |

Strict checking improves reliability, particularly when multiple developers work on the same codebase.

# 🗁 Separate Build and Development Configurations

Large applications often have different requirements for development, testing, and production builds.

A base configuration can contain shared compiler settings.

```json
// tsconfig.base.json

{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

A development configuration can extend the base:

```json
// tsconfig.json

{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "noEmit": true
  },
  "include": ["src/**/*.ts"]
}
```

A build configuration can define output settings:

```json
// tsconfig.build.json

{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist",
    "declaration": true,
    "sourceMap": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["src/**/*.test.ts"]
}
```

Run the appropriate configuration:

```bash
# Development type checking
tsc --noEmit

# Production compilation
tsc -p tsconfig.build.json
```

This separates type checking from JavaScript generation.

# 🗁 Use Project References in Monorepositories

A monorepository stores multiple related packages or applications in one repository.

Example:

```text
monorepo/
├── apps/
│   ├── web/
│   └── api/
├── packages/
│   ├── shared/
│   ├── database/
│   └── config/
├── tsconfig.json
└── package.json
```

Project references allow TypeScript projects to depend on one another through explicit build relationships.

Root configuration:

```json
{
  "files": [],
  "references": [
    { "path": "./packages/shared" },
    { "path": "./packages/database" },
    { "path": "./apps/api" },
    { "path": "./apps/web" }
  ]
}
```

A referenced package:

```json
// packages/shared/tsconfig.json

{
  "compilerOptions": {
    "composite": true,
    "declaration": true,
    "declarationMap": true,
    "outDir": "dist",
    "rootDir": "src"
  },
  "include": ["src"]
}
```

A dependent package can reference it:

```json
// apps/api/tsconfig.json

{
  "compilerOptions": {
    "composite": true,
    "declaration": true,
    "outDir": "dist",
    "rootDir": "src"
  },
  "references": [
    { "path": "../../packages/shared" }
  ],
  "include": ["src"]
}
```

Build the dependency graph:

```bash
tsc --build
```

Project references improve build organization and allow TypeScript to work with declaration outputs at project boundaries.

# 🗁 Use Package Boundaries

In a monorepository, each package should have a clearly defined responsibility.

Example:

```text
packages/
├── shared-types/
│   ├── src/
│   └── package.json
├── logger/
│   ├── src/
│   └── package.json
└── database/
    ├── src/
    └── package.json
```

A package should expose a deliberate public API.

Example package metadata:

```json
{
  "name": "@app/shared-types",
  "version": "1.0.0",
  "type": "module",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js"
    }
  }
}
```

The package must actually build these output files, and its export configuration must match the runtime and TypeScript module-resolution strategy.

Package boundaries help prevent applications from depending on another package's private implementation files.

# 🗁 Avoid Circular Dependencies

Circular dependencies occur when modules depend on one another directly or indirectly.

Example:

```text
user.service.ts
    ↓
order.service.ts
    ↓
user.service.ts
```

These relationships make code harder to reason about and can create runtime initialization problems.

A better approach is to introduce a shared abstraction or move common functionality into an independent module.

```text
user.service.ts ──┐
                  ├──> shared/
order.service.ts ─┘
```

For example:

```typescript
// shared/notification.ts

export function sendNotification(message: string): void {
  console.log(message);
}
```

Both services can depend on the shared module without depending on each other.

# 🗁 Use Dependency Injection

Dependency injection allows components to receive their dependencies rather than constructing them internally.

This makes services easier to test and reduces coupling.

```typescript
interface UserRepository {
  findById(id: number): User | undefined;
}

interface User {
  id: number;
  name: string;
}

class UserService {
  constructor(
    private readonly repository: UserRepository,
  ) {}

  getUser(id: number): User | undefined {
    return this.repository.findById(id);
  }
}
```

An implementation can be injected:

```typescript
class InMemoryUserRepository implements UserRepository {
  private readonly users: User[] = [
    { id: 1, name: "Alice" },
  ];

  findById(id: number): User | undefined {
    return this.users.find((user) => user.id === id);
  }
}

const repository = new InMemoryUserRepository();
const userService = new UserService(repository);

console.log(userService.getUser(1));
```

The service depends on a contract instead of a specific storage implementation.

# 🗁 Use Type-Only Imports

Type-only imports make it clear when a dependency is required only for type checking.

```typescript
import type { User } from "./user.types";
```

They are particularly useful for:

* Shared interfaces.
* Generic constraints.
* Function signatures.
* Type aliases.
* Cross-package type dependencies.

Use regular imports when runtime values are required:

```typescript
import { UserService } from "./user.service";
```

Separating type dependencies from runtime dependencies helps make module relationships clearer.

# 🗁 Separate Runtime Validation from Static Types

TypeScript types are removed during compilation.

They cannot validate untrusted data at runtime.

For example:

```typescript
interface User {
  id: number;
  name: string;
}
```

This does not guarantee that an API response actually contains a valid `User`.

Use runtime validation at system boundaries.

Example using a schema-validation library:

```typescript
import { z } from "zod";

const UserSchema = z.object({
  id: z.number(),
  name: z.string(),
});

type User = z.infer<typeof UserSchema>;

const input: unknown = {
  id: 1,
  name: "Alice",
};

const user = UserSchema.parse(input);
```

This validates external data and provides a TypeScript type derived from the schema.

Runtime validation is especially important for:

* API responses.
* Environment variables.
* User input.
* Database records.
* External service integrations.

# 🗁 Establish Consistent Error Handling

Large applications benefit from consistent error types and error-handling conventions.

```typescript
export class AppError extends Error {
  constructor(
    message: string,
    public readonly statusCode: number,
    public readonly code: string,
  ) {
    super(message);
    this.name = "AppError";
  }
}
```

A specialized error:

```typescript
export class NotFoundError extends AppError {
  constructor(resource: string) {
    super(
      `${resource} not found`,
      404,
      "RESOURCE_NOT_FOUND",
    );

    this.name = "NotFoundError";
  }
}
```

Usage:

```typescript
function getUser(id: number) {
  const user = users.find((item) => item.id === id);

  if (!user) {
    throw new NotFoundError("User");
  }

  return user;
}
```

A centralized error-handling layer can translate application errors into consistent HTTP responses or other application-level results.

# 🗁 Organize Testing Around Project Boundaries

Testing should follow the same logical boundaries as the application.

Example:

```text
src/
├── features/
│   └── users/
│       ├── user.service.ts
│       ├── user.service.test.ts
│       ├── user.repository.ts
│       └── user.repository.test.ts
└── shared/
```

Different test levels serve different purposes:

* Unit tests validate individual functions and classes.
* Integration tests validate interactions between components.
* End-to-end tests validate complete application workflows.
* Type tests validate compile-time API contracts.

Use mocks or test doubles when external dependencies need to be isolated.

# 🗁 Use Consistent Naming Conventions

Consistent naming makes large codebases easier to navigate.

Example conventions:

| Element | Convention | Example |
| --- | --- | --- |
| Interface | PascalCase | `User` |
| Type alias | PascalCase | `ApiResponse` |
| Class | PascalCase | `UserService` |
| Function | camelCase | `getUser()` |
| Variable | camelCase | `userName` |
| Constant | camelCase or UPPER_SNAKE_CASE when appropriate | `apiUrl` |
| Enum | PascalCase | `UserStatus` |
| Generic parameter | Short descriptive PascalCase | `TData` |
| File | kebab-case | `user-service.ts` |

Consistency matters more than adopting a particular naming convention.

# 🗁 Practical Example: A Scalable TypeScript Backend

A feature-oriented backend might use this structure:

```text
backend/
├── src/
│   ├── app.ts
│   ├── main.ts
│   ├── config/
│   │   └── environment.ts
│   ├── shared/
│   │   ├── errors/
│   │   ├── types/
│   │   └── utils/
│   ├── infrastructure/
│   │   ├── database/
│   │   └── logging/
│   └── features/
│       ├── users/
│       │   ├── user.types.ts
│       │   ├── user.repository.ts
│       │   ├── user.service.ts
│       │   ├── user.controller.ts
│       │   └── index.ts
│       ├── products/
│       │   ├── product.types.ts
│       │   ├── product.repository.ts
│       │   ├── product.service.ts
│       │   ├── product.controller.ts
│       │   └── index.ts
│       └── orders/
│           ├── order.types.ts
│           ├── order.repository.ts
│           ├── order.service.ts
│           ├── order.controller.ts
│           └── index.ts
├── tests/
├── tsconfig.json
├── tsconfig.build.json
└── package.json
```

This architecture separates:

* Application entry points.
* Configuration.
* Shared utilities and contracts.
* Infrastructure integrations.
* Business features.
* Testing.

The structure can evolve as the application grows without requiring every feature to depend on every other feature.

# 🗁 Best Practices

* Organize code around business features and clear responsibilities.
* Define explicit public APIs for reusable modules.
* Keep shared types focused and meaningful.
* Use strict compiler settings consistently.
* Separate development and production compiler configurations.
* Use project references for large multi-package projects.
* Establish package boundaries in monorepositories.
* Avoid unnecessary circular dependencies.
* Prefer dependency injection for components with external dependencies.
* Separate compile-time types from runtime validation.
* Use consistent error-handling and testing strategies.
* Measure compiler and build performance before optimizing.
* Document architectural decisions that affect multiple teams.

# 🗁 Key Takeaways

* Large TypeScript projects require deliberate architecture and dependency management.
* Feature-oriented organization keeps related functionality together.
* Layered architecture separates business logic, data access, and request handling.
* Shared types should represent genuinely shared contracts.
* Barrel files can simplify imports but may introduce circular dependencies.
* Project references help organize monorepositories and multi-package builds.
* Package boundaries protect internal implementation details.
* Dependency injection reduces coupling and improves testability.
* Runtime validation is necessary for untrusted external data.
* Consistent compiler settings, testing, and naming conventions improve long-term maintainability.
