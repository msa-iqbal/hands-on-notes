# Write File

The `ofstream` class is used to write text data to a file.

## Example

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ofstream file("data.txt");

    if (!file) {
        cout << "Failed to open file." << endl;
        return 1;
    }

    file << "Hello, C++!" << endl;
    file << "This is a file." << endl;
    file << "File handling is useful." << endl;

    file.close();

    cout << "Data written successfully." << endl;

    return 0;
}
```

## Expected Output

```text
Data written successfully.
```

## File Content

After running the program, `data.txt` contains:

```text
Hello, C++!
This is a file.
File handling is useful.
```

## Writing Different Data Types

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ofstream file("student.txt");

    string name = "Muhammad";
    int age = 25;
    double marks = 95.5;

    file << "Name: " << name << endl;
    file << "Age: " << age << endl;
    file << "Marks: " << marks << endl;

    file.close();

    return 0;
}
```

## File Content

```text
Name: Muhammad
Age: 25
Marks: 95.5
```

## Checking for Errors

```cpp
ofstream file("data.txt");

if (!file) {
    cerr << "Unable to open file." << endl;
    return 1;
}

file << "Hello";
```

Checking the stream before writing helps detect file-opening failures.
