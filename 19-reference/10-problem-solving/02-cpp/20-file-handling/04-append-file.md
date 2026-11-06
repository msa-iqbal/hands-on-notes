# Append File

Appending means adding new data to the end of an existing file without removing its current contents.

Use `ios::app` when opening the file.

## Example

Suppose `data.txt` contains:

```text
First line.
Second line.
```

Program:

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ofstream file("data.txt", ios::app);

    if (!file) {
        cout << "Failed to open file." << endl;
        return 1;
    }

    file << "Third line." << endl;
    file << "Fourth line." << endl;

    file.close();

    return 0;
}
```

## Result

The existing content is preserved:

```text
First line.
Second line.
Third line.
Fourth line.
```

## Append User Input

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    ofstream file("notes.txt", ios::app);

    if (!file) {
        cout << "Failed to open file." << endl;
        return 1;
    }

    string note;

    cout << "Enter a note: ";
    getline(cin, note);

    file << note << endl;

    file.close();

    cout << "Note added." << endl;

    return 0;
}
```

## Expected Output

```text
Enter a note: Learn C++ file handling
Note added.
```

## `ios::app` vs Normal Writing

Normal:

```cpp
ofstream file("data.txt");
```

Opening an existing file this way generally truncates its contents.

Appending:

```cpp
ofstream file("data.txt", ios::app);
```

preserves the existing contents and writes new data at the end.

## Key Point

```cpp
ios::app
```

means:

> Write all new output at the end of the file.
