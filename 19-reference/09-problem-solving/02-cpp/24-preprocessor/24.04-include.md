# `#include`

The `#include` directive tells the preprocessor to make the contents of another file available to the current source file.

## Basic Syntax

Standard header:

```cpp
#include <iostream>
```

Custom header:

```cpp
#include "my-header.h"
```

## Example 1: Standard Header

```cpp
#include <iostream>

int main() {
    std::cout << "Hello C++" << std::endl;

    return 0;
}
```

## Example 2: String Header

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name = "Alice";

    std::cout << name << std::endl;

    return 0;
}
```

## Example 3: Vector Header

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {
        10, 20, 30
    };

    for (int number : numbers) {
        std::cout << number << " ";
    }

    return 0;
}
```

### Output

```text
10 20 30
```

## `<header>` vs `"header"`

### Angle brackets

```cpp
#include <iostream>
```

This form is normally used for system/standard headers.

### Double quotes

```cpp
#include "math.h"
```

This form is commonly used for project headers.

## Example 4: Custom Header

### `math.h`

```cpp
#ifndef MATH_H
#define MATH_H

int add(int a, int b);

#endif
```

### `math.cpp`

```cpp
#include "math.h"

int add(int a, int b) {
    return a + b;
}
```

### `main.cpp`

```cpp
#include <iostream>
#include "math.h"

int main() {
    std::cout << add(10, 20) << std::endl;

    return 0;
}
```

Compile:

```bash
g++ main.cpp math.cpp -o app
```

## Example 5: Including Multiple Headers

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
```

Each header provides declarations for different standard library components.

## Include Guards

Custom headers should normally be protected against repeated inclusion.

```cpp
#ifndef MY_HEADER_H
#define MY_HEADER_H

// declarations

#endif
```

or:

```cpp
#pragma once

// declarations
```

## Avoid Including Unnecessary Headers

Prefer:

```cpp
#include <iostream>
#include <vector>
```

when those are the only facilities needed.

Avoid relying on another header indirectly including something your code uses.

For example, if you use `std::string`, explicitly include:

```cpp
#include <string>
```

## Header Inclusion Model

Conceptually, the preprocessor makes the included file's contents available at the inclusion point before compilation.

For example:

```cpp
#include "math.h"

int main() {
    return add(10, 20);
}
```

The compiler ultimately sees the relevant declarations from `math.h` as part of the translation unit.

## Key Points

- `#include` is a preprocessor directive.
- `<...>` is commonly used for standard/system headers.
- `"..."` is commonly used for project headers.
- Include the headers whose declarations your source directly uses.
- Header guards or `#pragma once` help prevent repeated inclusion.
