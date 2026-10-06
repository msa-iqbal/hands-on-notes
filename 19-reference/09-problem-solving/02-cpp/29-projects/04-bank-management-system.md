# Bank Management System

> A simple educational console-based bank account management system.

### Important Note

This project is intended for learning C++ programming.

It is **not suitable for real financial or banking use**. Real banking systems require strong authentication, authorization, encryption, auditing, transaction integrity, regulatory controls, and secure persistent storage.

### Features

- Create account
- Display accounts
- Search account
- Deposit money
- Withdraw money
- Check balance
- Menu-driven interface

### Account Structure

```text
Account Number
Account Holder
Balance
```

### Complete Program

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;

class Account {
private:
    int accountNumber;
    string holderName;
    double balance;

public:
    Account(int number, const string& name, double initialBalance)
        : accountNumber(number),
          holderName(name),
          balance(initialBalance) {}

    int getAccountNumber() const {
        return accountNumber;
    }

    string getHolderName() const {
        return holderName;
    }

    double getBalance() const {
        return balance;
    }

    void deposit(double amount) {
        if (amount <= 0) {
            cout << "Amount must be greater than zero.\n";
            return;
        }

        balance += amount;

        cout << "Deposit successful.\n";
    }

    void withdraw(double amount) {
        if (amount <= 0) {
            cout << "Amount must be greater than zero.\n";
            return;
        }

        if (amount > balance) {
            cout << "Insufficient balance.\n";
            return;
        }

        balance -= amount;

        cout << "Withdrawal successful.\n";
    }

    void display() const {
        cout << "\nAccount Number: " << accountNumber << endl;
        cout << "Holder Name: " << holderName << endl;
        cout << fixed << setprecision(2);
        cout << "Balance: " << balance << endl;
    }
};

void createAccount(vector<Account>& accounts) {
    int number;
    string name;
    double initialBalance;

    cout << "\nEnter account number: ";
    cin >> number;

    cin.ignore();

    cout << "Enter account holder name: ";
    getline(cin, name);

    cout << "Enter initial balance: ";
    cin >> initialBalance;

    if (initialBalance < 0) {
        cout << "Initial balance cannot be negative.\n";
        return;
    }

    accounts.emplace_back(number, name, initialBalance);

    cout << "Account created successfully.\n";
}

void listAccounts(const vector<Account>& accounts) {
    if (accounts.empty()) {
        cout << "\nNo accounts found.\n";
        return;
    }

    cout << "\n===== Accounts =====\n";

    for (const Account& account : accounts) {
        account.display();
    }
}

Account* findAccount(vector<Account>& accounts, int number) {
    for (Account& account : accounts) {
        if (account.getAccountNumber() == number) {
            return &account;
        }
    }

    return nullptr;
}

void depositMoney(vector<Account>& accounts) {
    int number;
    double amount;

    cout << "\nEnter account number: ";
    cin >> number;

    Account* account = findAccount(accounts, number);

    if (account == nullptr) {
        cout << "Account not found.\n";
        return;
    }

    cout << "Enter deposit amount: ";
    cin >> amount;

    account->deposit(amount);
}

void withdrawMoney(vector<Account>& accounts) {
    int number;
    double amount;

    cout << "\nEnter account number: ";
    cin >> number;

    Account* account = findAccount(accounts, number);

    if (account == nullptr) {
        cout << "Account not found.\n";
        return;
    }

    cout << "Enter withdrawal amount: ";
    cin >> amount;

    account->withdraw(amount);
}

void searchAccount(vector<Account>& accounts) {
    int number;

    cout << "\nEnter account number: ";
    cin >> number;

    Account* account = findAccount(accounts, number);

    if (account == nullptr) {
        cout << "Account not found.\n";
        return;
    }

    account->display();
}

int main() {
    vector<Account> accounts;

    int choice;

    do {
        cout << "\n===== Bank Management System =====\n";
        cout << "1. Create Account\n";
        cout << "2. List Accounts\n";
        cout << "3. Search Account\n";
        cout << "4. Deposit\n";
        cout << "5. Withdraw\n";
        cout << "6. Exit\n";
        cout << "Choose: ";
        cin >> choice;

        switch (choice) {
            case 1:
                createAccount(accounts);
                break;

            case 2:
                listAccounts(accounts);
                break;

            case 3:
                searchAccount(accounts);
                break;

            case 4:
                depositMoney(accounts);
                break;

            case 5:
                withdrawMoney(accounts);
                break;

            case 6:
                cout << "Bank system closed.\n";
                break;

            default:
                cout << "Invalid choice.\n";
        }

    } while (choice != 6);

    return 0;
}
```

### Example Run

```text
===== Bank Management System =====
1. Create Account
2. List Accounts
3. Search Account
4. Deposit
5. Withdraw
6. Exit
Choose: 1

Enter account number: 10001
Enter account holder name: Rahim Hasan
Enter initial balance: 5000
Account created successfully.
```

###### Deposit

```text
Enter account number: 10001
Enter deposit amount: 1500
Deposit successful.
```

###### Withdraw

```text
Enter account number: 10001
Enter withdrawal amount: 2000
Withdrawal successful.
```

###### Balance

```text
Account Number: 10001
Holder Name: Rahim Hasan
Balance: 4500.00
```

### Key Concepts

- Classes
- Encapsulation
- Constructors
- `vector`
- Pointers
- Object-oriented programming
- Input validation
- CRUD-style operations

### Possible Improvements

- Persistent file storage
- Transaction history
- Transfer between accounts
- Account deletion
- Account types
- Authentication
- Audit logging
- Better monetary representation using integer minor units
