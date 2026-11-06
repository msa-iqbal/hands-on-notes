# Contact Management System

> A simple console-based contact management application using C++.

### Purpose

This project demonstrates CRUD operations using a contact list.

### Features

- Add contact
- List contacts
- Search contact
- Update contact
- Delete contact
- Menu-driven interface

### Contact Information

```text
ID
Name
Phone
Email
```

### Complete Program

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;

struct Contact {
    int id;
    string name;
    string phone;
    string email;
};

void addContact(vector<Contact>& contacts) {
    Contact contact;

    cout << "\nEnter contact ID: ";
    cin >> contact.id;

    cin.ignore();

    cout << "Enter name: ";
    getline(cin, contact.name);

    cout << "Enter phone: ";
    getline(cin, contact.phone);

    cout << "Enter email: ";
    getline(cin, contact.email);

    contacts.push_back(contact);

    cout << "Contact added successfully.\n";
}

void listContacts(const vector<Contact>& contacts) {
    if (contacts.empty()) {
        cout << "\nNo contacts found.\n";
        return;
    }

    cout << "\n===== Contact List =====\n";

    cout << left
         << setw(8) << "ID"
         << setw(25) << "Name"
         << setw(20) << "Phone"
         << setw(30) << "Email"
         << endl;

    cout << string(83, '-') << endl;

    for (const Contact& contact : contacts) {
        cout << left
             << setw(8) << contact.id
             << setw(25) << contact.name
             << setw(20) << contact.phone
             << setw(30) << contact.email
             << endl;
    }
}

void searchContact(const vector<Contact>& contacts) {
    int id;

    cout << "\nEnter contact ID: ";
    cin >> id;

    for (const Contact& contact : contacts) {
        if (contact.id == id) {
            cout << "\nContact Found\n";
            cout << "ID: " << contact.id << endl;
            cout << "Name: " << contact.name << endl;
            cout << "Phone: " << contact.phone << endl;
            cout << "Email: " << contact.email << endl;
            return;
        }
    }

    cout << "Contact not found.\n";
}

void updateContact(vector<Contact>& contacts) {
    int id;

    cout << "\nEnter contact ID: ";
    cin >> id;

    for (Contact& contact : contacts) {
        if (contact.id == id) {
            cin.ignore();

            cout << "Enter new name: ";
            getline(cin, contact.name);

            cout << "Enter new phone: ";
            getline(cin, contact.phone);

            cout << "Enter new email: ";
            getline(cin, contact.email);

            cout << "Contact updated successfully.\n";
            return;
        }
    }

    cout << "Contact not found.\n";
}

void deleteContact(vector<Contact>& contacts) {
    int id;

    cout << "\nEnter contact ID: ";
    cin >> id;

    for (auto it = contacts.begin(); it != contacts.end(); ++it) {
        if (it->id == id) {
            contacts.erase(it);
            cout << "Contact deleted successfully.\n";
            return;
        }
    }

    cout << "Contact not found.\n";
}

int main() {
    vector<Contact> contacts;

    int choice;

    do {
        cout << "\n===== Contact Management System =====\n";
        cout << "1. Add Contact\n";
        cout << "2. List Contacts\n";
        cout << "3. Search Contact\n";
        cout << "4. Update Contact\n";
        cout << "5. Delete Contact\n";
        cout << "6. Exit\n";
        cout << "Choose: ";
        cin >> choice;

        switch (choice) {
            case 1:
                addContact(contacts);
                break;

            case 2:
                listContacts(contacts);
                break;

            case 3:
                searchContact(contacts);
                break;

            case 4:
                updateContact(contacts);
                break;

            case 5:
                deleteContact(contacts);
                break;

            case 6:
                cout << "Contact manager closed.\n";
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
===== Contact Management System =====
1. Add Contact
2. List Contacts
3. Search Contact
4. Update Contact
5. Delete Contact
6. Exit
Choose: 1

Enter contact ID: 1
Enter name: Rahim Hasan
Enter phone: 01700000000
Enter email: rahim@example.com
Contact added successfully.
```

### Search Example

```text
Enter contact ID: 1

Contact Found
ID: 1
Name: Rahim Hasan
Phone: 01700000000
Email: rahim@example.com
```

### CRUD Mapping

|Operation|Function|
|---|---|
|Create|`addContact()`|
|Read|`listContacts()`|
|Search|`searchContact()`|
|Update|`updateContact()`|
|Delete|`deleteContact()`|

## Key Concepts
### Key Concepts

- Structures
- Vectors
- Iterators
- References
- Functions
- CRUD operations
- String input
- Menu-driven programs

### Possible Improvements

- Search by name
- Search by phone
- Sort contacts
- Duplicate detection
- Groups/categories
- File storage
- Import/export
- Favorites
