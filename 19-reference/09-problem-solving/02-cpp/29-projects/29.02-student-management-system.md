# Student Management System

> A console-based student management system using C++.

### Purpose

This project demonstrates CRUD operations:

- Create
- Read
- Update
- Delete

It stores student records in a `vector`.

### Student Information

Each student contains:

- ID
- Name
- Age
- Department
- Marks

### Features

- Add student
- Display students
- Search student
- Update student
- Delete student
- Menu-driven interface

### Complete Program

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;

struct Student {
    int id;
    string name;
    int age;
    string department;
    double marks;
};

void addStudent(vector<Student>& students) {
    Student student;

    cout << "\nEnter student ID: ";
    cin >> student.id;

    cin.ignore();

    cout << "Enter name: ";
    getline(cin, student.name);

    cout << "Enter age: ";
    cin >> student.age;

    cin.ignore();

    cout << "Enter department: ";
    getline(cin, student.department);

    cout << "Enter marks: ";
    cin >> student.marks;

    students.push_back(student);

    cout << "Student added successfully.\n";
}

void listStudents(const vector<Student>& students) {
    if (students.empty()) {
        cout << "\nNo students found.\n";
        return;
    }

    cout << "\n===== Student List =====\n";

    cout << left
         << setw(8) << "ID"
         << setw(25) << "Name"
         << setw(8) << "Age"
         << setw(20) << "Department"
         << setw(10) << "Marks"
         << endl;

    cout << string(71, '-') << endl;

    for (const Student& student : students) {
        cout << left
             << setw(8) << student.id
             << setw(25) << student.name
             << setw(8) << student.age
             << setw(20) << student.department
             << setw(10) << student.marks
             << endl;
    }
}

void searchStudent(const vector<Student>& students) {
    int id;

    cout << "\nEnter student ID: ";
    cin >> id;

    for (const Student& student : students) {
        if (student.id == id) {
            cout << "\nStudent Found\n";
            cout << "ID: " << student.id << endl;
            cout << "Name: " << student.name << endl;
            cout << "Age: " << student.age << endl;
            cout << "Department: " << student.department << endl;
            cout << "Marks: " << student.marks << endl;
            return;
        }
    }

    cout << "Student not found.\n";
}

void updateStudent(vector<Student>& students) {
    int id;

    cout << "\nEnter student ID: ";
    cin >> id;

    for (Student& student : students) {
        if (student.id == id) {
            cin.ignore();

            cout << "Enter new name: ";
            getline(cin, student.name);

            cout << "Enter new age: ";
            cin >> student.age;

            cin.ignore();

            cout << "Enter new department: ";
            getline(cin, student.department);

            cout << "Enter new marks: ";
            cin >> student.marks;

            cout << "Student updated successfully.\n";
            return;
        }
    }

    cout << "Student not found.\n";
}

void deleteStudent(vector<Student>& students) {
    int id;

    cout << "\nEnter student ID: ";
    cin >> id;

    for (auto it = students.begin(); it != students.end(); ++it) {
        if (it->id == id) {
            students.erase(it);
            cout << "Student deleted successfully.\n";
            return;
        }
    }

    cout << "Student not found.\n";
}

int main() {
    vector<Student> students;

    int choice;

    do {
        cout << "\n===== Student Management System =====\n";
        cout << "1. Add Student\n";
        cout << "2. List Students\n";
        cout << "3. Search Student\n";
        cout << "4. Update Student\n";
        cout << "5. Delete Student\n";
        cout << "6. Exit\n";
        cout << "Choose: ";
        cin >> choice;

        switch (choice) {
            case 1:
                addStudent(students);
                break;

            case 2:
                listStudents(students);
                break;

            case 3:
                searchStudent(students);
                break;

            case 4:
                updateStudent(students);
                break;

            case 5:
                deleteStudent(students);
                break;

            case 6:
                cout << "Program closed.\n";
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
===== Student Management System =====
1. Add Student
2. List Students
3. Search Student
4. Update Student
5. Delete Student
6. Exit
Choose: 1

Enter student ID: 101
Enter name: Rahim Hasan
Enter age: 21
Enter department: Computer Science
Enter marks: 88
Student added successfully.
```

### Search Example

```text
Enter student ID: 101

Student Found
ID: 101
Name: Rahim Hasan
Age: 21
Department: Computer Science
Marks: 88
```

### CRUD Mapping

|Operation|Function|
|---|---|
|Create|`addContact()`|
|Read|`listContacts()`|
|Search|`searchContact()`|
|Update|`updateContact()`|
|Delete|`deleteContact()`|

### Key Concepts

- `struct`
- `vector`
- Functions
- References
- Iterators
- `getline()`
- CRUD operations
- Menu-driven applications

### Possible Improvements

- Save students to a file
- Load students at startup
- Sort by marks
- Calculate grades
- Search by name
- Prevent duplicate IDs
- Add login system
- Add attendance records
