# Multiple Template Parameters

Write a C++ program to create a class template with multiple template parameters.

## Program

```
#include <iostream>
using namespace std;

template <typename T, typename U>
class Pair {
private:
    T first;
    U second;

public:
    Pair(T a, U b) {
        first = a;
        second = b;
    }

    void display() const {
        cout << "First: " << first << endl;
        cout << "Second: " << second << endl;
    }
};

int main() {
    Pair<int, double> pair1(10, 25.5);
    Pair<string, int> pair2("Age", 25);

    cout << "Pair 1:" << endl;
    pair1.display();

    cout << endl;

    cout << "Pair 2:" << endl;
    pair2.display();

    return 0;
}
```

## Sample Output

```
Pair 1:
First: 10
Second: 25.5

Pair 2:
First: Age
Second: 25
```
