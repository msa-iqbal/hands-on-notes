# Graph

A **graph** is a data structure consisting of:

- **Vertices** — nodes
- **Edges** — connections between nodes

Example:

```text
A ---- B
|      |
|      |
C ---- D
```

## 1. Undirected Graph

In an undirected graph:

```text
A ---- B
```

means:

```text
A -> B
B -> A
```

## 2. Directed Graph

In a directed graph:

```text
A ---> B
```

the edge has a direction.

## 3. Weighted Graph

Edges can contain weights:

```text
A --5-- B
```

The weight could represent:

- Distance
- Cost
- Time
- Network latency

## 4. Adjacency Matrix

An adjacency matrix uses a 2D array.

Example:

```text
A -- B
|    |
C -- D
```

```cpp
#include <iostream>
using namespace std;

int main() {
    int graph[4][4] = {
        {0, 1, 1, 0},
        {1, 0, 0, 1},
        {1, 0, 0, 1},
        {0, 1, 1, 0}
    };

    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            cout << graph[i][j] << " ";
        }

        cout << endl;
    }

    return 0;
}
```

### Output

```text
0 1 1 0
1 0 0 1
1 0 0 1
0 1 1 0
```

## 5. Adjacency List

An adjacency list stores the neighbors of each vertex.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<vector<int>> graph(4);

    graph[0].push_back(1);
    graph[0].push_back(2);

    graph[1].push_back(0);
    graph[1].push_back(3);

    graph[2].push_back(0);
    graph[2].push_back(3);

    graph[3].push_back(1);
    graph[3].push_back(2);

    for (int i = 0; i < 4; i++) {
        cout << i << ": ";

        for (int neighbor : graph[i]) {
            cout << neighbor << " ";
        }

        cout << endl;
    }

    return 0;
}
```

### Output

```text
0: 1 2
1: 0 3
2: 0 3
3: 1 2
```

## 6. Breadth-First Search

BFS uses a queue.

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

void bfs(const vector<vector<int>>& graph, int start) {
    vector<bool> visited(graph.size(), false);
    queue<int> q;

    visited[start] = true;
    q.push(start);

    while (!q.empty()) {
        int current = q.front();
        q.pop();

        cout << current << " ";

        for (int neighbor : graph[current]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}

int main() {
    vector<vector<int>> graph = {
        {1, 2},
        {0, 3},
        {0, 3},
        {1, 2}
    };

    bfs(graph, 0);

    return 0;
}
```

### Output

```text
0 1 2 3
```

## 7. Depth-First Search

DFS can be implemented recursively.

```cpp
#include <iostream>
#include <vector>
using namespace std;

void dfs(
    const vector<vector<int>>& graph,
    vector<bool>& visited,
    int current
) {
    visited[current] = true;

    cout << current << " ";

    for (int neighbor : graph[current]) {
        if (!visited[neighbor]) {
            dfs(graph, visited, neighbor);
        }
    }
}

int main() {
    vector<vector<int>> graph = {
        {1, 2},
        {0, 3},
        {0, 3},
        {1, 2}
    };

    vector<bool> visited(graph.size(), false);

    dfs(graph, visited, 0);

    return 0;
}
```

### Output

```text
0 1 3 2
```

The exact DFS traversal order depends on the order in which neighbors are stored.

## Graph Representations

|Representation|Space|Useful For|
|---|--:|---|
|Adjacency Matrix|O(V²)|Dense graphs|
|Adjacency List|O(V + E)|Sparse graphs|

Where:

- `V` = number of vertices
- `E` = number of edges

## Important Graph Terms

|Term|Meaning|
|---|---|
|Vertex|A graph node|
|Edge|Connection between vertices|
|Directed|Edges have direction|
|Undirected|Edges have no direction|
|Weighted|Edges have values|
|Degree|Number of connected edges|
|Path|Sequence of connected vertices|
|Cycle|Path returning to its starting vertex|
