# Create and Close File

C++ provides file streams through the `<fstream>` header.

The `ofstream` class is commonly used to create and write to files.

## Basic Example

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ofstream file("data.txt");

    if (!file.is_open()) {
        cout << "Failed to create file." << endl;
        return 1;
    }

    cout << "File created successfully." << endl;

    file.close();

    cout << "File closed successfully." << endl;

    return 0;
}
```

## Expected Output

```text
File created successfully.
File closed successfully.
```

A file named `data.txt` will be created in the program's current working directory.

## Using open()

A file can also be opened after creating the stream object.

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ofstream file;

    file.open("data.txt");

    if (!file.is_open()) {
        cout << "Failed to open file." << endl;
        return 1;
    }

    cout << "File opened successfully." << endl;

    file.close();

    return 0;
}
```

## Important Functions

|Function|Purpose|
|---|---|
|`open()`|Opens a file|
|`close()`|Closes a file|
|`is_open()`|Checks whether the file is open|

## Note

C++ stream objects use RAII, so a file is normally closed automatically when the stream object goes out of scope. Explicit `close()` can still be useful when you want to close the file at a specific point.
