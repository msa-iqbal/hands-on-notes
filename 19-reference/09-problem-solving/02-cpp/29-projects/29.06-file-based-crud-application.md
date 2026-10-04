# File-Based CRUD Application

> A complete C++ CRUD application that stores records in a text file.

### Purpose

Previous CRUD projects stored data in memory.

When the program closes, the data disappears.

This project adds **file persistence** so that records remain available after the program exits.

### CRUD Operations

```text
Create → Add record
Read   → List/search records
Update → Modify record
Delete → Remove record
```

### File Format

Records are stored using this format:

```text
ID|Name|Department|Marks
```

Example:

```text
101|Rahim Hasan|Computer Science|88.5
102|Karim Ahmed|Information Technology|91
103|Nadia Akter|Software Engineering|85.5
```

### Features

- Add record
- List records
- Search record
- Update record
- Delete record
- Persistent storage
- Text-file database
- Temporary file for update/delete

### Complete Program

```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <sstream>
#include <cstdio>

using namespace std;

const string FILE_NAME = "students.txt";
const string TEMP_FILE = "students.tmp";

struct Student {
    int id;
    string name;
    string department;
    double marks;
};

string serialize(const Student& student) {
    return to_string(student.id) + "|" +
           student.name + "|" +
           student.department + "|" +
           to_string(student.marks);
}

bool deserialize(const string& line, Student& student) {
    string idText;
    string marksText;

    stringstream ss(line);

    if (!getline(ss, idText, '|')) {
        return false;
    }

    if (!getline(ss, student.name, '|')) {
        return false;
    }

    if (!getline(ss, student.department, '|')) {
        return false;
    }

    if (!getline(ss, marksText, '|')) {
        return false;
    }

    try {
        student.id = stoi(idText);
        student.marks = stod(marksText);
    } catch (...) {
        return false;
    }

    return true;
}

vector<Student> loadStudents() {
    vector<Student> students;

    ifstream file(FILE_NAME);

    if (!file) {
        return students;
    }

    string line;

    while (getline(file, line)) {
        if (line.empty()) {
            continue;
        }

        Student student;

        if (deserialize(line, student)) {
            students.push_back(student);
        }
    }

    return students;
}

bool saveStudents(const vector<Student>& students) {
    ofstream file(FILE_NAME);

    if (!file) {
        return false;
    }

    for (const Student& student : students) {
        file << serialize(student) << '\n';
    }

    return true;
}

bool studentExists(const vector<Student>& students, int id) {
    for (const Student& student : students) {
        if (student.id == id) {
            return true;
        }
    }

    return false;
}

void addStudent() {
    vector<Student> students = loadStudents();

    Student student;

    cout << "\nEnter ID: ";
    cin >> student.id;

    if (studentExists(students, student.id)) {
        cout << "A student with this ID already exists.\n";
        return;
    }

    cin.ignore();

    cout << "Enter name: ";
    getline(cin, student.name);

    cout << "Enter department: ";
    getline(cin, student.department);

    cout << "Enter marks: ";
    cin >> student.marks;

    students.push_back(student);

    if (saveStudents(students)) {
        cout << "Student saved successfully.\n";
    } else {
        cout << "Failed to save student.\n";
    }
}

void listStudents() {
    vector<Student> students = loadStudents();

    if (students.empty()) {
        cout << "\nNo records found.\n";
        return;
    }

    cout << "\n===== Student Records =====\n";

    for (const Student& student : students) {
        cout << "\nID: " << student.id << endl;
        cout << "Name: " << student.name << endl;
        cout << "Department: " << student.department << endl;
        cout << "Marks: " << student.marks << endl;
    }
}

void searchStudent() {
    vector<Student> students = loadStudents();

    int id;

    cout << "\nEnter student ID: ";
    cin >> id;

    for (const Student& student : students) {
        if (student.id == id) {
            cout << "\nStudent Found\n";
            cout << "ID: " << student.id << endl;
            cout << "Name: " << student.name << endl;
            cout << "Department: " << student.department << endl;
            cout << "Marks: " << student.marks << endl;
            return;
        }
    }

    cout << "Student not found.\n";
}

void updateStudent() {
    vector<Student> students = loadStudents();

    int id;

    cout << "\nEnter student ID to update: ";
    cin >> id;

    for (Student& student : students) {
        if (student.id == id) {
            cin.ignore();

            cout << "Enter new name: ";
            getline(cin, student.name);

            cout << "Enter new department: ";
            getline(cin, student.department);

            cout << "Enter new marks: ";
            cin >> student.marks;

            if (saveStudents(students)) {
                cout << "Student updated successfully.\n";
            } else {
                cout << "Failed to update student.\n";
            }

            return;
        }
    }

    cout << "Student not found.\n";
}

void deleteStudent() {
    vector<Student> students = loadStudents();

    int id;

    cout << "\nEnter student ID to delete: ";
    cin >> id;

    vector<Student> remainingStudents;

    bool found = false;

    for (const Student& student : students) {
        if (student.id == id) {
            found = true;
        } else {
            remainingStudents.push_back(student);
        }
    }

    if (!found) {
        cout << "Student not found.\n";
        return;
    }

    if (saveStudents(remainingStudents)) {
        cout << "Student deleted successfully.\n";
    } else {
        cout << "Failed to delete student.\n";
    }
}

int main() {
    int choice;

    do {
        cout << "\n===== File-Based CRUD Application =====\n";
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
                addStudent();
                break;

            case 2:
                listStudents();
                break;

            case 3:
                searchStudent();
                break;

            case 4:
                updateStudent();
                break;

            case 5:
                deleteStudent();
                break;

            case 6:
                cout << "Application closed.\n";
                break;

            default:
                cout << "Invalid choice.\n";
        }

    } while (choice != 6);

    return 0;
}
```

### Example Run

###### Add Record

```text
===== File-Based CRUD Application =====
1. Add Student
2. List Students
3. Search Student
4. Update Student
5. Delete Student
6. Exit
Choose: 1

Enter ID: 101
Enter name: Rahim Hasan
Enter department: Computer Science
Enter marks: 88.5
Student saved successfully.
```

The file becomes:

```text
101|Rahim Hasan|Computer Science|88.500000
```

### Add Another Record

```text
Choose: 1

Enter ID: 102
Enter name: Karim Ahmed
Enter department: Information Technology
Enter marks: 91
Student saved successfully.
```

The file becomes:

```text
101|Rahim Hasan|Computer Science|88.500000
102|Karim Ahmed|Information Technology|91.000000
```

### List Records

```text
Choose: 2

===== Student Records =====

ID: 101
Name: Rahim Hasan
Department: Computer Science
Marks: 88.5

ID: 102
Name: Karim Ahmed
Department: Information Technology
Marks: 91
```

### Search Record

```text
Choose: 3

Enter student ID: 102

Student Found
ID: 102
Name: Karim Ahmed
Department: Information Technology
Marks: 91
```

### Update Record

```text
Choose: 4

Enter student ID to update: 101
Enter new name: Rahim Hasan
Enter new department: Software Engineering
Enter new marks: 93
Student updated successfully.
```

### Delete Record

```text
Choose: 5

Enter student ID to delete: 102
Student deleted successfully.
```

### Serialization

Serialization converts a C++ object into a text representation.

```cpp
string serialize(const Student& student) {
    return to_string(student.id) + "|" +
           student.name + "|" +
           student.department + "|" +
           to_string(student.marks);
}
```

Example:

```text
Student object
     |
     v
101, Rahim Hasan, Computer Science, 88.5
     |
     v
101|Rahim Hasan|Computer Science|88.500000
```

### Deserialization

Deserialization converts the stored text back into a C++ object.

```cpp
Student student;

deserialize(
    "101|Rahim Hasan|Computer Science|88.500000",
    student
);
```

The result is approximately:

```text
student.id         = 101
student.name       = "Rahim Hasan"
student.department  = "Computer Science"
student.marks       = 88.5
```

### File Operations

###### Open for Reading

```cpp
ifstream file("students.txt");
```

###### Open for Writing

```cpp
ofstream file("students.txt");
```

###### Read Lines

```cpp
string line;

while (getline(file, line)) {
    cout << line << endl;
}
```

###### Write Lines

```cpp
file << "101|Rahim Hasan|Computer Science|88.5\n";
```

### Why Use a Temporary File?

For many text-file CRUD designs, updating or deleting a record can be implemented by:

```text
Original file
     |
     v
Read records
     |
     v
Modify/remove target record
     |
     v
Write temporary file
     |
     v
Replace original file
```

For larger or concurrent applications, a database is generally more appropriate than repeatedly rewriting a text file.

### Key Concepts

- `fstream`
- `ifstream`
- `ofstream`
- File persistence
- Serialization
- Deserialization
- `stringstream`
- `vector`
- CRUD
- Exception-safe parsing
- Text-file storage

### Limitations

This simple format has limitations.

For example, the `|` character is being used as a delimiter. Therefore, unrestricted use of `|` inside fields would require escaping or another serialization format.

The application also rewrites the complete dataset when modifying or deleting records.

### Possible Improvements

- Use CSV with proper escaping
- Use JSON
- Use SQLite
- Add file locking
- Add transaction-like recovery
- Add sorting
- Add pagination
- Add validation
- Separate database logic from UI
- Create a reusable repository class
- Add automated tests

### Project Architecture

A larger version could be organized as:

```text
application/
├── main.cpp
├── student.hpp
├── student.cpp
├── file-storage.hpp
├── file-storage.cpp
└── ui.hpp
```

This separates:

```text
User Interface
      |
      v
Application Logic
      |
      v
Storage Layer
      |
      v
students.txt
```

### Final Learning Outcome

After completing this project, you have practiced:

```text
C++ Basics
    ↓
Structures
    ↓
Vectors
    ↓
Functions
    ↓
CRUD
    ↓
File I/O
    ↓
Serialization
    ↓
Persistent Application
```
