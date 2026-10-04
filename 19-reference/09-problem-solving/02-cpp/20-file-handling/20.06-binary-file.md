# Binary File

A binary file stores data as bytes rather than human-readable text.

Use `ios::binary` when opening a binary file.

## Write Binary Data

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    int number = 12345;

    ofstream file(
        "number.bin",
        ios::binary
    );

    if (!file) {
        cout << "Failed to open file." << endl;
        return 1;
    }

    file.write(
        reinterpret_cast<char*>(&number),
        sizeof(number)
    );

    file.close();

    cout << "Binary data written." << endl;

    return 0;
}
```

## Expected Output

```text
Binary data written.
```

## Read Binary Data

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    int number;

    ifstream file(
        "number.bin",
        ios::binary
    );

    if (!file) {
        cout << "Failed to open file." << endl;
        return 1;
    }

    file.read(
        reinterpret_cast<char*>(&number),
        sizeof(number)
    );

    file.close();

    cout << "Number: " << number << endl;

    return 0;
}
```

## Expected Output

```text
Number: 12345
```

## Binary Struct Example

```cpp
#include <iostream>
#include <fstream>

using namespace std;

struct Student {
    int id;
    double marks;
};

int main() {
    Student student = {101, 95.5};

    ofstream output(
        "student.bin",
        ios::binary
    );

    output.write(
        reinterpret_cast<char*>(&student),
        sizeof(student)
    );

    output.close();

    Student result;

    ifstream input(
        "student.bin",
        ios::binary
    );

    input.read(
        reinterpret_cast<char*>(&result),
        sizeof(result)
    );

    input.close();

    cout << "ID: " << result.id << endl;
    cout << "Marks: " << result.marks << endl;

    return 0;
}
```

## Expected Output

```text
ID: 101
Marks: 95.5
```

## Text vs Binary

|Feature|Text File|Binary File|
|---|---|---|
|Data representation|Human-readable|Raw bytes|
|Typical extension|`.txt`|`.bin`|
|Readability|Easy|Not normally human-readable|
|`ios::binary`|Not required|Required|
|Common operations|`<<`, `>>`, `getline()`|`read()`, `write()`|

## Important Note

Writing an object directly with `write()` is appropriate only for suitable trivially copyable data. It is not a general serialization mechanism for objects containing pointers, virtual functions, dynamically allocated memory, or other non-portable state.

For portable long-term data exchange, use an explicit serialization format instead.
