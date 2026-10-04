# Exception Handling Best Practices

Use exceptions for exceptional situations rather than ordinary program flow.

## 1. Catch by const reference

Prefer:

```cpp
catch (const exception& error) {
    cout << error.what();
}
```

instead of copying the exception object.

## 2. Catch specific exceptions first

```cpp
try {
    // code
}
catch (const invalid_argument& error) {
    // specific handling
}
catch (const exception& error) {
    // general handling
}
```

## 3. Use `what()`

Standard exceptions provide a descriptive message through `what()`.

```cpp
catch (const exception& error) {
    cout << error.what();
}
```

## 4. Avoid using exceptions for normal control flow

Do not use exceptions as a replacement for ordinary conditions such as:

```cpp
if (number == 0) {
    // handle condition
}
```

## 5. Preserve the original exception when rethrowing

Use:

```cpp
throw;
```

instead of:

```cpp
throw error;
```

when you need to propagate the currently handled exception without unnecessarily changing its type or information.

## 6. Prefer standard exception types when appropriate

Examples:

```cpp
invalid_argument
out_of_range
runtime_error
logic_error
```

## 7. Make exception messages useful

Prefer:

```cpp
throw runtime_error("Database connection failed");
```

over:

```cpp
throw runtime_error("Error");
```

## Complete Example

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

double calculate(double a, double b) {
    if (b == 0) {
        throw invalid_argument("Divisor cannot be zero");
    }

    return a / b;
}

int main() {
    try {
        cout << calculate(10, 0) << endl;
    }
    catch (const invalid_argument& error) {
        cout << "Invalid argument: " << error.what() << endl;
    }
    catch (const exception& error) {
        cout << "Exception: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Invalid argument: Divisor cannot be zero
```
