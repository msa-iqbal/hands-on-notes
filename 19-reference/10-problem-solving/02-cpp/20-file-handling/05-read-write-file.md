# Read and Write File

The `fstream` class can be used for both reading and writing.

Include:

```cpp
#include <fstream>
```

## Basic Example

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    fstream file(
        "data.txt",
        ios::in | ios::out | ios::trunc
    );

    if (!file) {
        cout << "Failed to open file." << endl;
        return 1;
    }

    file << "Hello, C++!" << endl;
    file << "Read and write using fstream." << endl;

    file.seekg(0);

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
Read and write using fstream.
```

## Open Modes

```cpp
ios::in
```

Open for reading.

```cpp
ios::out
```

Open for writing.

```cpp
ios::app
```

Append data to the end.

```cpp
ios::trunc
```

Clear existing contents when opening for output.

Modes can be combined:

```cpp
ios::in | ios::out
```

## Reading and Writing Numbers

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    fstream file(
        "numbers.txt",
        ios::in | ios::out | ios::trunc
    );

    file << 10 << " "
         << 20 << " "
         << 30;

    file.seekg(0);

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
10 20 30
```

## File Position

When using `fstream`, input and output positions can be controlled with:

```cpp
seekg()
seekp()
tellg()
tellp()
```

For basic text-file operations, `ifstream` and `ofstream` are often clearer when only reading or only writing is required.
