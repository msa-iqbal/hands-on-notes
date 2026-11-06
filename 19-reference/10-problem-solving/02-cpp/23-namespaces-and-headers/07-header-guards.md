# Header Guards

A **header guard** prevents a header file from being included multiple times in the same translation unit.

## Basic Syntax

```cpp
#ifndef MY_HEADER_H
#define MY_HEADER_H

// declarations

#endif
```

## Example 1: Header Guard

### `math.h`

```cpp
#ifndef MATH_H
#define MATH_H

int add(int a, int b);

#endif
```

If the header is included more than once, the declarations are processed only once.

## Example 2: Multiple Includes

```cpp
#include "math.h"
#include "math.h"

int main() {
    return 0;
}
```

The header guard prevents duplicate processing.

## Example 3: `#pragma once`

Many compilers support:

```cpp
#pragma once

int add(int a, int b);
```

This is a simpler alternative to traditional include guards.

## Traditional Guard vs pragma once

Traditional:

```cpp
#ifndef PERSON_H
#define PERSON_H

class Person {
};

#endif
```

Modern common alternative:

```cpp
#pragma once

class Person {
};
```

## Example 4: Header With Class

```cpp
#ifndef PERSON_H
#define PERSON_H

#include <string>

class Person {
private:
    std::string name;

public:
    Person(std::string name);
};

#endif
```

## Naming Convention

A guard can use the project and filename:

```cpp
#ifndef MY_PROJECT_PERSON_H
#define MY_PROJECT_PERSON_H

// ...

#endif
```

The exact naming convention can vary by project.

## Why Header Guards Matter

Without protection, the same declaration or definition may be processed multiple times through different include paths.

Header guards provide an include-once mechanism for a translation unit.

## Key Points

- Header guards prevent repeated inclusion.
- `#ifndef`, `#define`, and `#endif` form the traditional pattern.
- `#pragma once` is widely supported and simpler.
- Header guards are especially important for reusable headers.
