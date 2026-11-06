# Macro

A **macro** is a preprocessor definition that performs textual substitution before compilation.

Macros can represent values or function-like expressions.

## Example 1: Object-Like Macro

```cpp
#include <iostream>
using namespace std;

#define PI 3.14159

int main() {
    cout << PI << endl;

    return 0;
}
```

The preprocessor replaces `PI` with `3.14159`.

## Example 2: Function-Like Macro

```cpp
#include <iostream>
using namespace std;

#define SQUARE(x) ((x) * (x))

int main() {
    cout << SQUARE(5) << endl;

    return 0;
}
```

### Output

```text
25
```

## Example 3: Addition Macro

```cpp
#include <iostream>
using namespace std;

#define ADD(a, b) ((a) + (b))

int main() {
    cout << ADD(10, 20) << endl;

    return 0;
}
```

### Output

```text
30
```

## Why Parentheses Matter

Consider:

```cpp
#define SQUARE(x) x * x
```

Then:

```cpp
SQUARE(2 + 3)
```

can expand to:

```cpp
2 + 3 * 2 + 3
```

which does not calculate the intended square.

Better:

```cpp
#define SQUARE(x) ((x) * (x))
```

## Example 4: Stringification

The `#` operator inside a macro converts an argument into a string literal.

```cpp
#include <iostream>
using namespace std;

#define TO_STRING(x) #x

int main() {
    cout << TO_STRING(Hello World) << endl;

    return 0;
}
```

### Output

```text
Hello World
```

## Example 5: Token Concatenation

The `##` operator combines tokens.

```cpp
#include <iostream>
using namespace std;

#define CREATE_VARIABLE(name) int variable_##name = 100

int main() {
    CREATE_VARIABLE(test);

    cout << variable_test << endl;

    return 0;
}
```

### Output

```text
100
```

The macro creates:

```cpp
int variable_test = 100;
```

## Example 6: Multi-Line Macro

A backslash can continue a macro onto another line.

```cpp
#include <iostream>
using namespace std;

#define PRINT_INFO(name, age) \
    cout << "Name: " << name << endl; \
    cout << "Age: " << age << endl;

int main() {
    PRINT_INFO("Alice", 25);

    return 0;
}
```

### Output

```text
Name: Alice
Age: 25
```

## Macro Pitfall

Macros do not behave like normal functions.

```cpp
#define DOUBLE(x) ((x) + (x))
```

Calling:

```cpp
int number = 5;

DOUBLE(number++);
```

can increment `number` more than once because the macro substitutes the expression multiple times.

A normal function is often safer.

## Modern C++ Alternative

Instead of:

```cpp
#define SQUARE(x) ((x) * (x))
```

prefer:

```cpp
template <typename T>
constexpr T square(T x) {
    return x * x;
}
```

## Key Points

- Macros perform preprocessor substitution.
- Function-like macros accept arguments.
- Always parenthesize macro parameters and expressions carefully.
- `#` performs stringification.
- `##` performs token concatenation.
- Prefer functions, templates, `constexpr`, and typed constants when they provide a suitable alternative.
