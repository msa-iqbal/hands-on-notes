# Encapsulation

Write a C++ program to demonstrate encapsulation by keeping data members private and accessing them through public member functions.

## Program

```cpp
#include <iostream>
using namespace std;

class BankAccount {
private:
    double balance;

public:
    BankAccount(double initialBalance) {
        balance = initialBalance;
    }

    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    double getBalance() const {
        return balance;
    }
};

int main() {
    BankAccount account(1000);

    account.deposit(500);

    cout << "Account balance: "
         << account.getBalance() << endl;

    return 0;
}
```

## Sample Output

```text
Account balance: 1500
```
