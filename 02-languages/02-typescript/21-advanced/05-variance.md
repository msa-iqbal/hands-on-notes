# Variance

Variance describes how the assignability of generic types changes when their type arguments have an inheritance or compatibility relationship.

In TypeScript, variance determines whether a generic type using a more specific type argument can be assigned to the same generic type using a more general type argument, and vice versa.

Consider these types:

```typescript
type Animal = {
  name: string;
};

type Dog = Animal & {
  bark(): void;
};

type Cat = Animal & {
  meow(): void;
};
```

`Dog` and `Cat` are more specific types of `Animal`.

```typescript
const dog: Dog = {
  name: "Buddy",
  bark() {
    console.log("Woof!");
  },
};

const animal: Animal = dog; // Valid
```

A `Dog` can be used where an `Animal` is expected because it contains all the required members.

Variance describes how this relationship behaves when these types appear inside generic types.

# 🗁 The Four Main Variance Categories

Type systems commonly describe four variance categories:

* Covariance
* Contravariance
* Invariance
* Bivariance

# 🗁 Covariance

Covariance preserves the direction of the subtype relationship.

If `Dog` is assignable to `Animal`, then a covariant generic type allows:

```typescript
Generic<Dog> → Generic<Animal>
```

Example:

```typescript
type Producer<T> = {
  getValue(): T;
};

const dogProducer: Producer<Dog> = {
  getValue: () => dog,
};

const animalProducer: Producer<Animal> = dogProducer;
```

This is safe because `animalProducer` only produces values that satisfy `Animal`.

It never promises to produce a value more specific than `Animal`.

Covariance commonly occurs when a type parameter is used in output positions.

Examples include:

* Function return types
* Read-only data providers
* Data retrieval interfaces
* Observable values

# 🗁 Contravariance

Contravariance reverses the direction of the subtype relationship.

If `Dog` is assignable to `Animal`, a contravariant generic type allows:

```typescript
Generic<Animal> → Generic<Dog>
```

Example:

```typescript
type Consumer<T> = {
  consume: (value: T) => void;
};

const animalConsumer: Consumer<Animal> = {
  consume(value) {
    console.log(value.name);
  },
};

const dogConsumer: Consumer<Dog> = animalConsumer;
```

This is safe because a function capable of accepting any `Animal` can also accept a `Dog`.

The opposite assignment is unsafe:

```typescript
const specificDogConsumer: Consumer<Dog> = {
  consume(value) {
    value.bark();
  },
};

// Error under strictFunctionTypes
const generalAnimalConsumer: Consumer<Animal> =
  specificDogConsumer;
```

The dog consumer requires a `Dog`, but a caller using `Consumer<Animal>` could pass a `Cat`.

Contravariance commonly occurs when a type parameter is used in input positions.

Examples include:

* Function parameters
* Event handlers
* Callback arguments
* Consumer interfaces

# 🗁 Invariance

Invariance means that neither direction of assignment is allowed.

If `Dog` is assignable to `Animal`, an invariant generic type does not automatically allow either:

```typescript
Generic<Dog> → Generic<Animal>
Generic<Animal> → Generic<Dog>
```

Consider a mutable container:

```typescript
type MutableBox<T> = {
  value: T;
};
```

If TypeScript allowed a `MutableBox<Dog>` to be treated as a `MutableBox<Animal>`, code could insert a `Cat` into a box that is supposed to contain only dogs.

```typescript
const dogBox: MutableBox<Dog> = {
  value: dog,
};
```

TypeScript's actual variance analysis is more nuanced than this simplified example. In particular, ordinary mutable object properties can be checked bivariantly in certain structural assignments.

When strict invariance is required, a type can use a type parameter in both function input and output positions:

```typescript
type InvariantBox<T> = {
  get: () => T;
  set: (value: T) => void;
};
```

With `strictFunctionTypes`, this combination generally makes `T` invariant.

# 🗁 Bivariance

Bivariance allows compatibility in both directions, even when only one direction would be safe under ordinary variance rules.

TypeScript permits bivariance in certain contexts, notably method and constructor parameter positions.

```typescript
type AnimalHandler = {
  handle(value: Animal): void;
};

type DogHandler = {
  handle(value: Dog): void;
};

const dogHandler: DogHandler = {
  handle(value) {
    value.bark();
  },
};

const animalHandler: AnimalHandler = dogHandler;
```

Method parameter checking is intentionally more permissive than function-property checking.

This behavior exists partly for compatibility with common JavaScript and TypeScript APIs.

Bivariance can be convenient, but it can also allow unsafe assignments.

# 🗁 Function Parameter Variance

Function parameter variance is particularly important when working with callbacks.

Consider these function types:

```typescript
type AnimalCallback = (value: Animal) => void;
type DogCallback = (value: Dog) => void;
```

A callback that accepts any `Animal` can safely be used where a callback accepting `Dog` is expected.

```typescript
const handleAnimal: AnimalCallback = (value) => {
  console.log(value.name);
};

const handleDog: DogCallback = handleAnimal;
```

The reverse is unsafe:

```typescript
const handleSpecificDog: DogCallback = (value) => {
  value.bark();
};

// Error with strictFunctionTypes
const handleAnyAnimal: AnimalCallback = handleSpecificDog;
```

A caller could pass a `Cat` to `handleAnyAnimal`, but the assigned function expects a `Dog`.

# 🗁 The strictFunctionTypes Compiler Option

The `strictFunctionTypes` compiler option controls how function parameter types are checked.

It is enabled by the `strict` compiler option.

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

When enabled:

* Function parameters in function types are checked contravariantly.
* Unsafe parameter assignments are rejected.
* Method and constructor declarations retain their bivariance exception.

Example using function properties:

```typescript
type AnimalHandler = {
  handle: (value: Animal) => void;
};

type DogHandler = {
  handle: (value: Dog) => void;
};

declare const dogHandler: DogHandler;

// Error with strictFunctionTypes
const animalHandler: AnimalHandler = dogHandler;
```

Compare this with method syntax:

```typescript
type AnimalHandler = {
  handle(value: Animal): void;
};

type DogHandler = {
  handle(value: Dog): void;
};

declare const dogHandler: DogHandler;

// Allowed because method parameters are checked bivariantly
const animalHandler: AnimalHandler = dogHandler;
```

The difference between method syntax and function-property syntax is important when designing strongly typed APIs.

# 🗁 Variance in Generic Interfaces

Variance also applies to generic interfaces.

```typescript
interface Producer<T> {
  produce(): T;
}

interface Consumer<T> {
  consume(value: T): void;
}

interface Transformer<T> {
  transform(value: T): T;
}
```

Their intended variance behavior is:

| Interface | Type parameter usage | Variance |
| --- | --- | --- |
| `Producer<T>` | Output | Covariant |
| `Consumer<T>` | Input | Contravariant |
| `Transformer<T>` | Input and output | Invariant |

These descriptions assume ordinary strict function-property semantics. Actual compatibility depends on the complete structure and TypeScript's variance rules.

# 🗁 Variance Annotations

TypeScript supports variance annotations for generic type parameters using `in` and `out`.

* `out` indicates covariance.
* `in` indicates contravariance.
* Using both indicates invariance.

```typescript
interface Producer<out T> {
  produce(): T;
}

interface Consumer<in T> {
  consume(value: T): void;
}

interface Transformer<in out T> {
  transform(value: T): T;
}
```

These annotations document and constrain variance behavior in supported generic type comparisons.

They are not general-purpose runtime features and do not change JavaScript output.

Important considerations:

* Variance annotations apply to type parameters on generic aliases and interfaces.
* They are primarily relevant to instantiation-based comparisons.
* They do not override every structural compatibility rule.
* They should reflect the actual usage of the type parameter.
* They are not a replacement for designing safe APIs.

# 🗁 Practical Example: Event Handling

Suppose an application has different event types:

```typescript
type Event = {
  timestamp: number;
};

type ClickEvent = Event & {
  x: number;
  y: number;
};

type KeyEvent = Event & {
  key: string;
};
```

A handler that accepts every event can safely handle a click event.

```typescript
type EventHandler<T> = (event: T) => void;

const handleEvent: EventHandler<Event> = (event) => {
  console.log(event.timestamp);
};

const handleClick: EventHandler<ClickEvent> = handleEvent;
```

This demonstrates contravariance in callback parameters.

A general event handler can be reused for more specific event types because it requires fewer assumptions about the input.

# 🗁 Practical Example: Repository Interfaces

Variance is useful when designing data access abstractions.

```typescript
interface RepositoryReader<out T> {
  findById(id: string): T | undefined;
}

interface RepositoryWriter<in T> {
  save(value: T): void;
}
```

A repository reader produces values, while a repository writer consumes values.

This separation makes the intended variance clearer and helps avoid unnecessary coupling.

The annotations should only be used when the complete type structure satisfies the declared variance.

# 🗁 Best Practices

* Understand covariance and contravariance when designing callback-heavy APIs.
* Enable `strictFunctionTypes` through the `strict` compiler option.
* Prefer function-property syntax when strict parameter variance is important.
* Be aware that method and constructor parameters have a bivariance exception.
* Use separate producer and consumer interfaces when their responsibilities differ.
* Use variance annotations only when they accurately describe the type parameter's usage.
* Avoid assuming every mutable generic type is automatically invariant.
* Test important generic assignments with compile-time examples.

# 🗁 Key Takeaways

* Variance describes how generic type compatibility changes with related type arguments.
* Covariance preserves the direction of assignability.
* Contravariance reverses the direction of assignability.
* Invariance allows neither direction.
* Bivariance allows both directions.
* Function return types are generally covariant.
* Function parameters are checked contravariantly under `strictFunctionTypes`.
* Method and constructor parameters retain a bivariance exception.
* TypeScript supports `in` and `out` variance annotations for generic type parameters.
