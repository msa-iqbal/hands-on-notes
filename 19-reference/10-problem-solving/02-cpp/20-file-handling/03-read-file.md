# Read File

The `ifstream` class is used to read data from a file.

## Example

Suppose `data.txt` contains:

```text
Hello, C++!
File handling is important.
C++ provides file streams.
```

The following program reads the file line by line.

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ifstream file("data.txt");

    if (!file) {
        cout << "Failed to open file." << endl;
        return 1;
    }

    string line;

    while (getline(file, line)) {
        cout << line << endl;
    }

    file.close();

    return 0;
}
```

## Expected Output

```text
Hello, C++!
File handling is important.
C++ provides file streams.
```

## Reading Word by Word

The extraction operator `>>` reads whitespace-separated values.

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ifstream file("data.txt");

    string word;

    while (file >> word) {
        cout << word << endl;
    }

    file.close();

    return 0;
}
```

If the file contains:

```text
C++ is powerful
```

The output is:

```text
C++
is
powerful
```

## Reading Numbers

Suppose `numbers.txt` contains:

```text
10 20 30 40 50
```

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ifstream file("numbers.txt");

    int number;

    while (file >> number) {
        cout << number << " ";
    }

    file.close();

    return 0;
}
```

## Expected Output

```text
10 20 30 40 50
```

## Important Functions

| Function      | Purpose                         |
| ------------- | ------------------------------- |
| `getline()`   | Reads a complete line           |
| `operator >>` | Reads formatted data            |
| `close()`     | Closes the file                 |
| `is_open()`   | Checks whether the file is open |
