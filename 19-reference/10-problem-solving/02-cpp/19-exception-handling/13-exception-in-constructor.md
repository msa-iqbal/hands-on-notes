# Exception in Constructor

A constructor can throw an exception when object initialization fails.

## Example

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

class Account {
private:
    double balance;

public:
    Account(double amount) {
        if (amount < 0) {
            throw invalid_argument("Balance cannot be negative");
        }

        balance = amount;
    }

    void display() {
        cout << "Balance: " << balance << endl;
    }
};

int main() {
    try {
        Account account(-500);

        account.display();
    }
    catch (const invalid_argument& error) {
        cout << "Error: " << error.what() << endl;
    }

    return 0;
}
```

## Expected Output

```text
Error: Balance cannot be negative
```
