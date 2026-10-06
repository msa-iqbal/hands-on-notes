# invalid_argument

`invalid_argument` is used when a function receives an invalid argument.

## Example

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

int squareRoot(int number) {
    if (number < 0) {
        throw invalid_argument("Number cannot be negative");
    }

    return number;
}

int main() {
    try {
        cout << squareRoot(-10) << endl;
    }
    catch (const invalid_argument& error) {
        cout << "Error: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Error: Number cannot be negative
```
