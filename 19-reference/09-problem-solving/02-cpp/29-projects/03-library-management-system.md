# Library Management System

> A console-based library management system using C++.

### Purpose

This project demonstrates how to manage books and their issue/return status.

### Features

- Add book
- List books
- Search book
- Issue book
- Return book
- Menu-driven interface
- Availability tracking

### Book Structure

Each book contains:

```text
Book ID
Title
Author
Availability
```

### Complete Program

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;

struct Book {
    int id;
    string title;
    string author;
    bool available;
};

void addBook(vector<Book>& books) {
    Book book;

    cout << "\nEnter book ID: ";
    cin >> book.id;

    cin.ignore();

    cout << "Enter title: ";
    getline(cin, book.title);

    cout << "Enter author: ";
    getline(cin, book.author);

    book.available = true;

    books.push_back(book);

    cout << "Book added successfully.\n";
}

void listBooks(const vector<Book>& books) {
    if (books.empty()) {
        cout << "\nNo books available.\n";
        return;
    }

    cout << "\n===== Library Books =====\n";

    cout << left
         << setw(8) << "ID"
         << setw(30) << "Title"
         << setw(25) << "Author"
         << setw(15) << "Status"
         << endl;

    cout << string(78, '-') << endl;

    for (const Book& book : books) {
        cout << left
             << setw(8) << book.id
             << setw(30) << book.title
             << setw(25) << book.author
             << setw(15)
             << (book.available ? "Available" : "Issued")
             << endl;
    }
}

void searchBook(const vector<Book>& books) {
    int id;

    cout << "\nEnter book ID: ";
    cin >> id;

    for (const Book& book : books) {
        if (book.id == id) {
            cout << "\nBook Found\n";
            cout << "ID: " << book.id << endl;
            cout << "Title: " << book.title << endl;
            cout << "Author: " << book.author << endl;
            cout << "Status: "
                 << (book.available ? "Available" : "Issued")
                 << endl;
            return;
        }
    }

    cout << "Book not found.\n";
}

void issueBook(vector<Book>& books) {
    int id;

    cout << "\nEnter book ID to issue: ";
    cin >> id;

    for (Book& book : books) {
        if (book.id == id) {
            if (!book.available) {
                cout << "Book is already issued.\n";
                return;
            }

            book.available = false;

            cout << "Book issued successfully.\n";
            return;
        }
    }

    cout << "Book not found.\n";
}

void returnBook(vector<Book>& books) {
    int id;

    cout << "\nEnter book ID to return: ";
    cin >> id;

    for (Book& book : books) {
        if (book.id == id) {
            if (book.available) {
                cout << "Book is already available.\n";
                return;
            }

            book.available = true;

            cout << "Book returned successfully.\n";
            return;
        }
    }

    cout << "Book not found.\n";
}

int main() {
    vector<Book> books;

    int choice;

    do {
        cout << "\n===== Library Management System =====\n";
        cout << "1. Add Book\n";
        cout << "2. List Books\n";
        cout << "3. Search Book\n";
        cout << "4. Issue Book\n";
        cout << "5. Return Book\n";
        cout << "6. Exit\n";
        cout << "Choose: ";
        cin >> choice;

        switch (choice) {
            case 1:
                addBook(books);
                break;

            case 2:
                listBooks(books);
                break;

            case 3:
                searchBook(books);
                break;

            case 4:
                issueBook(books);
                break;

            case 5:
                returnBook(books);
                break;

            case 6:
                cout << "Library system closed.\n";
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
===== Library Management System =====
1. Add Book
2. List Books
3. Search Book
4. Issue Book
5. Return Book
6. Exit
Choose: 1

Enter book ID: 1001
Enter title: The C++ Programming Language
Enter author: Bjarne Stroustrup
Book added successfully.
```

###### Issue Book

```text
Enter book ID to issue: 1001
Book issued successfully.
```

###### Return Book

```text
Enter book ID to return: 1001
Book returned successfully.
```

### Key Concepts

- Structures
- Vectors
- Boolean state
- Functions
- References
- Searching
- Menu-driven applications

### Possible Improvements

- Member management
- Borrower information
- Due dates
- Fine calculation
- File storage
- Book categories
- Book deletion
- Multiple copies of books
