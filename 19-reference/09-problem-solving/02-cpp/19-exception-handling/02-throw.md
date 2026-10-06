# throw

The `throw` keyword is used to generate an exception.

## Example

```cpp
#include <iostream>

using namespace std;

int main() {
    int age = 15;

    if (age < 18) {
        throw "Age must be 18 or older";
    }

    cout << "Access granted";

    return 0;
}
```

## Expected Output

```text
terminate called after throwing an instance of 'char const*'
```

A thrown exception should normally be handled with `try` and `catch`.

## Better Example

```cpp
#include <iostream>

using namespace std;

int main() {
    int age = 15;

    try {
        if (age < 18) {
            throw "Age must be 18 or older";
        }

        cout << "Access granted";
    }
    catch (const char* message) {
        cout << "Error: " << message << endl;
    }

    return 0;
}
```

## Expected Output

```text
Error: Age must be 18 or older
```
