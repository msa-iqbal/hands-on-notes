# emplace

`emplace` functions construct objects directly inside a container using constructor arguments.

Common examples:

```cpp
emplace_back()
emplace()
```

## Example 1: vector::emplace_back()

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class Person {
public:
    string name;
    int age;

    Person(string name, int age)
        : name(name), age(age) {}
};

int main() {
    vector<Person> people;

    people.emplace_back("Alice", 25);
    people.emplace_back("Bob", 30);

    for (const auto& person : people) {
        cout << person.name << " " << person.age << endl;
    }

    return 0;
}
```

### Output

```text
Alice 25
Bob 30
```

## Example 2: push_back()

With `push_back()`, you normally provide an already-created object.

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class Person {
public:
    string name;
    int age;

    Person(string name, int age)
        : name(name), age(age) {}
};

int main() {
    vector<Person> people;

    Person person("Alice", 25);

    people.push_back(person);

    cout << people[0].name << endl;

    return 0;
}
```

## Example 3: emplace_back()

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class Person {
public:
    string name;
    int age;

    Person(string name, int age)
        : name(name), age(age) {}
};

int main() {
    vector<Person> people;

    people.emplace_back("Alice", 25);

    cout << people[0].name << endl;

    return 0;
}
```

## Example 4: map::emplace()

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> ages;

    ages.emplace("Alice", 25);
    ages.emplace("Bob", 30);

    for (const auto& [name, age] : ages) {
        cout << name << " " << age << endl;
    }

    return 0;
}
```

## Example 5: set::emplace()

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> numbers;

    numbers.emplace(10);
    numbers.emplace(20);
    numbers.emplace(30);

    for (int number : numbers) {
        cout << number << " ";
    }

    return 0;
}
```

### Output

```text
10 20 30
```

## push_back vs emplace_back

|Function|Typical usage|
|---|---|
|`push_back(value)`|Insert an existing value|
|`emplace_back(args...)`|Construct an element from arguments|

Example:

```cpp
people.push_back(Person("Alice", 25));
```

versus:

```cpp
people.emplace_back("Alice", 25);
```

`emplace_back` can construct the object directly in the container, but it is not automatically better in every situation. Choose based on clarity and the types involved.

## Key Points

- `emplace` constructs an object using constructor arguments.
- `emplace_back` is common with `vector`, `deque`, and `list`.
- `emplace` is available for associative containers such as `map` and `set`.
- `push_back` is useful when you already have an object.
- `emplace_back` is useful when constructing an object from its arguments.
