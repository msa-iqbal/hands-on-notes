# Custom Exception Class

You can create your own exception class by deriving it from `std::exception`.

## Example

```cpp
#include <iostream>
#include <exception>

using namespace std;

class InvalidAgeException : public exception {
public:
    const char* what() const noexcept override {
        return "Invalid age";
    }
};

int main() {
    try {
        int age = -5;

        if (age < 0) {
            throw InvalidAgeException();
        }

        cout << "Age: " << age << endl;
    }
    catch (const InvalidAgeException& error) {
        cout << "Error: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Error: Invalid age
```
