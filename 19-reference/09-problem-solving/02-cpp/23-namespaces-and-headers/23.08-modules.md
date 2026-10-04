# Modules

**C++ modules** provide a modern mechanism for organizing and importing C++ code.

They were standardized in **C++20**.

Traditional C++ commonly uses:

```cpp
#include "math.h"
```

Modules use:

```cpp
import math;
```

## Basic Module Structure

A module interface can use:

```cpp
export module math;

export int add(int a, int b) {
    return a + b;
}
```

Another source file can import it:

```cpp
import math;
#include <iostream>

int main() {
    std::cout << add(10, 20) << std::endl;

    return 0;
}
```

### Output

```text
30
```

## Example 1: Export Function

### `math.cpp`

```cpp
export module math;

export int add(int a, int b) {
    return a + b;
}

export int multiply(int a, int b) {
    return a * b;
}
```

### `main.cpp`

```cpp
import math;

#include <iostream>

int main() {
    std::cout << add(10, 20) << std::endl;
    std::cout << multiply(5, 4) << std::endl;

    return 0;
}
```

## Exported vs Non-Exported

Only declarations marked with `export` are available to importers.

```cpp
export module math;

int internalFunction() {
    return 100;
}

export int add(int a, int b) {
    return a + b;
}
```

Here `add()` is exported, while `internalFunction()` is not part of the module's exported interface.

## Example 2: Module With Class

```cpp
export module person;

#include <string>

export class Person {
private:
    std::string name;

public:
    Person(std::string name)
        : name(std::move(name)) {}

    std::string getName() const {
        return name;
    }
};
```

An importing source can use:

```cpp
import person;

#include <iostream>

int main() {
    Person person("Alice");

    std::cout << person.getName() << std::endl;

    return 0;
}
```

## Modules vs Headers

|Headers|Modules|
|---|---|
|`#include`|`import`|
|Textual inclusion|Module interface|
|Include guards commonly needed|No traditional include guard|
|Can cause repeated parsing|Designed to improve dependency handling|
|Long-established|C++20 feature|

## Important Compiler Note

C++ module support varies by compiler and build system. The exact command required to compile modules depends on the compiler version and build configuration.

For this reason, ordinary header/source files remain common in many C++ projects.

## Key Points

- Modules are a C++20 feature.
- Modules use `export module` and `import`.
- `export` controls what is exposed.
- Modules provide an alternative to traditional header inclusion.
- Compiler and build-system support should be checked before adopting modules in a project.
