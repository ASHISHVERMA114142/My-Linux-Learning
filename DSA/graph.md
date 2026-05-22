# Graph Data Structure - Complete Interview Guide

## Table of Contents
1. [What is a Graph?](#what-is-a-graph)
2. [Graph Terminology](#graph-terminology)
3. [Graph Representations](#graph-representations)
4. [Graph Traversal Algorithms](#graph-traversal-algorithms)
5. [Important Graph Algorithms](#important-graph-algorithms)
6. [Common Graph Patterns](#common-graph-patterns)
7. [Practice Problems by Category](#practice-problems-by-category)
8. [Interview Tips](#interview-tips)

---

## What is a Graph?

A graph is a non-linear data structure consisting of **vertices (nodes)** and **edges** that connect these vertices. Graphs are used to represent networks, relationships, dependencies, and many real-world scenarios like social networks, maps, web pages, etc.

**Key Components:**
- **Vertex (Node):** A fundamental unit representing an entity
- **Edge:** A connection between two vertices
- **Weight:** Optional value associated with an edge (in weighted graphs)

```
Example Graph:
    A --- B
    |     |
    |     |
    C --- D

Vertices: {A, B, C, D}
Edges: {(A,B), (A,C), (B,D), (C,D)}
```

---

## Graph Terminology

### Types of Graphs

**1. Directed Graph (Digraph)**
- Edges have a direction (A → B is different from B → A)
- Example: Twitter followers, web page links

**2. Undirected Graph**
- Edges have no direction (A — B means both ways)
- Example: Facebook friends, road networks

**3. Weighted Graph**
- Edges have associated weights/costs
- Example: Road networks with distances, flight routes with costs

**4. Unweighted Graph**
- All edges are equal (no weights)
- Example: Social connections, maze paths

**5. Cyclic Graph**
- Contains at least one cycle (path that starts and ends at the same vertex)

**6. Acyclic Graph**
- Contains no cycles
- **DAG (Directed Acyclic Graph):** Special case used in scheduling, dependency resolution

**7. Connected Graph**
- Path exists between every pair of vertices

**8. Disconnected Graph**
- Some vertices are not reachable from others

### Important Terms

- **Degree:** Number of edges connected to a vertex
  - **In-degree:** Number of incoming edges (directed graphs)
  - **Out-degree:** Number of outgoing edges (directed graphs)
- **Path:** Sequence of vertices connected by edges
- **Cycle:** Path that starts and ends at the same vertex
- **Connected Component:** Maximal set of connected vertices
- **Strongly Connected Component (SCC):** In directed graphs, maximal set where every vertex is reachable from every other

---

## Graph Representations

### 1. Adjacency Matrix

A 2D array where `matrix[i][j] = 1` if there's an edge from vertex i to vertex j.

**Pros:**
- O(1) edge lookup
- Simple implementation
- Good for dense graphs

**Cons:**
- O(V²) space complexity
- Inefficient for sparse graphs

```java
// Adjacency Matrix Implementation
class GraphMatrix {
    private int[][] adjMatrix;
    private int vertices;
    
    public GraphMatrix(int vertices) {
        this.vertices = vertices;
        adjMatrix = new int[vertices][vertices];
    }
    
    // Add edge (undirected)
    public void addEdge(int src, int dest) {
        adjMatrix[src][dest] = 1;
        adjMatrix[dest][src] = 1; // Remove for directed graph
    }
    
    // Add weighted edge
    public void addWeightedEdge(int src, int dest, int weight) {
        adjMatrix[src][dest] = weight;
        adjMatrix[dest][src] = weight; // Remove for directed graph
    }
    
    // Check if edge exists
    public boolean hasEdge(int src, int dest) {
        return adjMatrix[src][dest] != 0;
    }
}
```

### 2. Adjacency List

Array of lists where each index represents a vertex and stores its neighbors.

**Pros:**
- O(V + E) space complexity
- Efficient for sparse graphs
- Fast neighbor iteration

**Cons:**
- O(V) edge lookup in worst case

```java
// Adjacency List Implementation
class GraphList {
    private List<List<Integer>> adjList;
    private int vertices;
    
    public GraphList(int vertices) {
        this.vertices = vertices;
        adjList = new ArrayList<>();
        for (int i = 0; i < vertices; i++) {
            adjList.add(new ArrayList<>());
        }
    }
    
    // Add edge (undirected)
    public void addEdge(int src, int dest) {
        adjList.get(src).add(dest);
        adjList.get(dest).add(src); // Remove for directed graph
    }
    
    // Get neighbors
    public List<Integer> getNeighbors(int vertex) {
        return adjList.get(vertex);
    }
}

// Weighted Graph with Adjacency List
class WeightedGraph {
    static class Edge {
        int dest;
        int weight;
        
        Edge(int dest, int weight) {
            this.dest = dest;
            this.weight = weight;
        }
    }
    
    private List<List<Edge>> adjList;
    
    public WeightedGraph(int vertices) {
        adjList = new ArrayList<>();
        for (int i = 0; i < vertices; i++) {
            adjList.add(new ArrayList<>());
        }
    }
    
    public void addEdge(int src, int dest, int weight) {
        adjList.get(src).add(new Edge(dest, weight));
        adjList.get(dest).add(new Edge(src, weight)); // Remove for directed
    }
}
```

### 3. Edge List

Simple list of all edges in the graph.

```java
class Edge {
    int src, dest, weight;
    
    Edge(int src, int dest, int weight) {
        this.src = src;
        this.dest = dest;
        this.weight = weight;
    }
}

List<Edge> edgeList = new ArrayList<>();
```

---

## Graph Traversal Algorithms

### 1. Breadth-First Search (BFS)

Explores graph level by level using a queue. Used for shortest path in unweighted graphs.

**Time Complexity:** O(V + E)  
**Space Complexity:** O(V)

**Use Cases:**
- Shortest path in unweighted graphs
- Level-order traversal
- Finding connected components
- Bipartite graph checking

```java
// BFS Implementation
public void bfs(int start, List<List<Integer>> graph) {
    boolean[] visited = new boolean[graph.size()];
    Queue<Integer> queue = new LinkedList<>();
    
    queue.offer(start);
    visited[start] = true;
    
    while (!queue.isEmpty()) {
        int node = queue.poll();
        System.out.print(node + " ");
        
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                queue.offer(neighbor);
            }
        }
    }
}

// BFS with distance tracking
public int[] bfsDistance(int start, List<List<Integer>> graph) {
    int n = graph.size();
    int[] distance = new int[n];
    Arrays.fill(distance, -1);
    
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    distance[start] = 0;
    
    while (!queue.isEmpty()) {
        int node = queue.poll();
        
        for (int neighbor : graph.get(node)) {
            if (distance[neighbor] == -1) {
                distance[neighbor] = distance[node] + 1;
                queue.offer(neighbor);
            }
        }
    }
    
    return distance;
}
```

### 2. Depth-First Search (DFS)

Explores graph by going as deep as possible before backtracking. Uses recursion or stack.

**Time Complexity:** O(V + E)  
**Space Complexity:** O(V)

**Use Cases:**
- Cycle detection
- Topological sorting
- Finding strongly connected components
- Path finding
- Maze solving

```java
// DFS Recursive Implementation
public void dfs(int node, List<List<Integer>> graph, boolean[] visited) {
    visited[node] = true;
    System.out.print(node + " ");
    
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            dfs(neighbor, graph, visited);
        }
    }
}

// DFS Iterative Implementation
public void dfsIterative(int start, List<List<Integer>> graph) {
    boolean[] visited = new boolean[graph.size()];
    Stack<Integer> stack = new Stack<>();
    
    stack.push(start);
    
    while (!stack.isEmpty()) {
        int node = stack.pop();
        
        if (!visited[node]) {
            visited[node] = true;
            System.out.print(node + " ");
            
            // Add neighbors in reverse order to maintain left-to-right order
            List<Integer> neighbors = graph.get(node);
            for (int i = neighbors.size() - 1; i >= 0; i--) {
                if (!visited[neighbors.get(i)]) {
                    stack.push(neighbors.get(i));
                }
            }
        }
    }
}

// DFS with path tracking
public boolean dfsPath(int node, int target, List<List<Integer>> graph, 
                       boolean[] visited, List<Integer> path) {
    visited[node] = true;
    path.add(node);
    
    if (node == target) {
        return true;
    }
    
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            if (dfsPath(neighbor, target, graph, visited, path)) {
                return true;
            }
        }
    }
    
    path.remove(path.size() - 1); // Backtrack
    return false;
}
```

---

## Important Graph Algorithms

### 1. Dijkstra's Algorithm (Shortest Path)

Finds shortest path from source to all vertices in weighted graph with non-negative weights.

**Time Complexity:** O((V + E) log V) with priority queue  
**Space Complexity:** O(V)

```java
public int[] dijkstra(List<List<Edge>> graph, int start) {
    int n = graph.size();
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;
    
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
    pq.offer(new int[]{start, 0}); // {node, distance}
    
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int node = curr[0];
        int distance = curr[1];
        
        if (distance > dist[node]) continue;
        
        for (Edge edge : graph.get(node)) {
            int newDist = dist[node] + edge.weight;
            if (newDist < dist[edge.dest]) {
                dist[edge.dest] = newDist;
                pq.offer(new int[]{edge.dest, newDist});
            }
        }
    }
    
    return dist;
}
```

### 2. Bellman-Ford Algorithm

Finds shortest path with negative weights, detects negative cycles.

**Time Complexity:** O(V × E)  
**Space Complexity:** O(V)

```java
public int[] bellmanFord(int vertices, List<Edge> edges, int start) {
    int[] dist = new int[vertices];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;
    
    // Relax edges V-1 times
    for (int i = 0; i < vertices - 1; i++) {
        for (Edge edge : edges) {
            if (dist[edge.src] != Integer.MAX_VALUE &&
                dist[edge.src] + edge.weight < dist[edge.dest]) {
                dist[edge.dest] = dist[edge.src] + edge.weight;
            }
        }
    }
    
    // Check for negative cycles
    for (Edge edge : edges) {
        if (dist[edge.src] != Integer.MAX_VALUE &&
            dist[edge.src] + edge.weight < dist[edge.dest]) {
            System.out.println("Negative cycle detected!");
            return null;
        }
    }
    
    return dist;
}
```

### 3. Floyd-Warshall Algorithm (All Pairs Shortest Path)

Finds shortest paths between all pairs of vertices.

**Time Complexity:** O(V³)  
**Space Complexity:** O(V²)

```java
public int[][] floydWarshall(int[][] graph) {
    int n = graph.length;
    int[][] dist = new int[n][n];
    
    // Initialize distances
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            dist[i][j] = graph[i][j];
        }
    }
    
    // Try all intermediate vertices
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (dist[i][k] != Integer.MAX_VALUE && 
                    dist[k][j] != Integer.MAX_VALUE &&
                    dist[i][k] + dist[k][j] < dist[i][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }
    
    return dist;
}
```

### 4. Topological Sort

Linear ordering of vertices in DAG where for every edge u → v, u comes before v.

**Time Complexity:** O(V + E)  
**Space Complexity:** O(V)

**Use Cases:**
- Task scheduling
- Build systems
- Course prerequisites

```java
// Topological Sort using DFS
public List<Integer> topologicalSort(List<List<Integer>> graph) {
    int n = graph.size();
    boolean[] visited = new boolean[n];
    Stack<Integer> stack = new Stack<>();
    
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            topologicalSortDFS(i, graph, visited, stack);
        }
    }
    
    List<Integer> result = new ArrayList<>();
    while (!stack.isEmpty()) {
        result.add(stack.pop());
    }
    return result;
}

private void topologicalSortDFS(int node, List<List<Integer>> graph,
                                boolean[] visited, Stack<Integer> stack) {
    visited[node] = true;
    
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            topologicalSortDFS(neighbor, graph, visited, stack);
        }
    }
    
    stack.push(node);
}

// Topological Sort using Kahn's Algorithm (BFS)
public List<Integer> topologicalSortKahn(List<List<Integer>> graph) {
    int n = graph.size();
    int[] indegree = new int[n];
    
    // Calculate indegrees
    for (int i = 0; i < n; i++) {
        for (int neighbor : graph.get(i)) {
            indegree[neighbor]++;
        }
    }
    
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0) {
            queue.offer(i);
        }
    }
    
    List<Integer> result = new ArrayList<>();
    while (!queue.isEmpty()) {
        int node = queue.poll();
        result.add(node);
        
        for (int neighbor : graph.get(node)) {
            indegree[neighbor]--;
            if (indegree[neighbor] == 0) {
                queue.offer(neighbor);
            }
        }
    }
    
    // Check for cycle
    if (result.size() != n) {
        return new ArrayList<>(); // Cycle detected
    }
    
    return result;
}
```

### 5. Union-Find (Disjoint Set Union)

Efficiently tracks connected components and detects cycles.

**Time Complexity:** O(α(n)) ≈ O(1) per operation (with path compression and union by rank)  
**Space Complexity:** O(V)

```java
class UnionFind {
    private int[] parent;
    private int[] rank;
    
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            rank[i] = 0;
        }
    }
    
    // Find with path compression
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    // Union by rank
    public boolean union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) {
            return false; // Already in same set
        }
        
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        
        return true;
    }
    
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```

### 6. Minimum Spanning Tree (MST)

Tree that connects all vertices with minimum total edge weight.

#### Kruskal's Algorithm

**Time Complexity:** O(E log E)  
**Space Complexity:** O(V)

```java
public List<Edge> kruskalMST(int vertices, List<Edge> edges) {
    // Sort edges by weight
    Collections.sort(edges, (a, b) -> a.weight - b.weight);
    
    UnionFind uf = new UnionFind(vertices);
    List<Edge> mst = new ArrayList<>();
    
    for (Edge edge : edges) {
        if (uf.union(edge.src, edge.dest)) {
            mst.add(edge);
            if (mst.size() == vertices - 1) {
                break;
            }
        }
    }
    
    return mst;
}
```

#### Prim's Algorithm

**Time Complexity:** O((V + E) log V)  
**Space Complexity:** O(V)

```java
public int primMST(List<List<Edge>> graph) {
    int n = graph.size();
    boolean[] inMST = new boolean[n];
    PriorityQueue<Edge> pq = new PriorityQueue<>((a, b) -> a.weight - b.weight);
    
    int totalWeight = 0;
    pq.offer(new Edge(0, 0)); // Start from vertex 0
    
    while (!pq.isEmpty()) {
        Edge edge = pq.poll();
        int node = edge.dest;
        
        if (inMST[node]) continue;
        
        inMST[node] = true;
        totalWeight += edge.weight;
        
        for (Edge nextEdge : graph.get(node)) {
            if (!inMST[nextEdge.dest]) {
                pq.offer(nextEdge);
            }
        }
    }
    
    return totalWeight;
}
```

### 7. Cycle Detection

#### Undirected Graph (DFS)

```java
public boolean hasCycleUndirected(List<List<Integer>> graph) {
    int n = graph.size();
    boolean[] visited = new boolean[n];
    
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            if (dfsCycleUndirected(i, -1, graph, visited)) {
                return true;
            }
        }
    }
    return false;
}

private boolean dfsCycleUndirected(int node, int parent, 
                                   List<List<Integer>> graph, boolean[] visited) {
    visited[node] = true;
    
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            if (dfsCycleUndirected(neighbor, node, graph, visited)) {
                return true;
            }
        } else if (neighbor != parent) {
            return true; // Cycle found
        }
    }
    return false;
}
```

#### Directed Graph (DFS with recursion stack)

```java
public boolean hasCycleDirected(List<List<Integer>> graph) {
    int n = graph.size();
    boolean[] visited = new boolean[n];
    boolean[] recStack = new boolean[n];
    
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            if (dfsCycleDirected(i, graph, visited, recStack)) {
                return true;
            }
        }
    }
    return false;
}

private boolean dfsCycleDirected(int node, List<List<Integer>> graph,
                                 boolean[] visited, boolean[] recStack) {
    visited[node] = true;
    recStack[node] = true;
    
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            if (dfsCycleDirected(neighbor, graph, visited, recStack)) {
                return true;
            }
        } else if (recStack[neighbor]) {
            return true; // Cycle found
        }
    }
    
    recStack[node] = false;
    return false;
}
```

### 8. Bipartite Graph Check

Check if graph can be colored with 2 colors such that no adjacent vertices have same color.

```java
public boolean isBipartite(List<List<Integer>> graph) {
    int n = graph.size();
    int[] color = new int[n];
    Arrays.fill(color, -1);
    
    for (int i = 0; i < n; i++) {
        if (color[i] == -1) {
            if (!bfsBipartite(i, graph, color)) {
                return false;
            }
        }
    }
    return true;
}

private boolean bfsBipartite(int start, List<List<Integer>> graph, int[] color) {
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    color[start] = 0;
    
    while (!queue.isEmpty()) {
        int node = queue.poll();
        
        for (int neighbor : graph.get(node)) {
            if (color[neighbor] == -1) {
                color[neighbor] = 1 - color[node];
                queue.offer(neighbor);
            } else if (color[neighbor] == color[node]) {
                return false;
            }
        }
    }
    return true;
}
```

---

## Common Graph Patterns

### Pattern 1: Matrix as Graph (Grid Problems)

Treat 2D matrix as graph where each cell is a node.

```java
// 4-directional movement
int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

// 8-directional movement
int[][] directions8 = {{-1, 0}, {1, 0}, {0, -1}, {0, 1},
                       {-1, -1}, {-1, 1}, {1, -1}, {1, 1}};

public void dfsGrid(int[][] grid, int row, int col, boolean[][] visited) {
    int m = grid.length, n = grid[0].length;
    
    if (row < 0 || row >= m || col < 0 || col >= n || 
        visited[row][col] || grid[row][col] == 0) {
        return;
    }
    
    visited[row][col] = true;
    
    for (int[] dir : directions) {
        dfsGrid(grid, row + dir[0], col + dir[1], visited);
    }
}
```

### Pattern 2: Implicit Graph (State Space)

Graph exists implicitly through state transitions.

```java
// Example: Word Ladder - each word is a node, edge exists if words differ by 1 char
public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    if (!wordSet.contains(endWord)) return 0;
    
    Queue<String> queue = new LinkedList<>();
    queue.offer(beginWord);
    int level = 1;
    
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            String word = queue.poll();
            if (word.equals(endWord)) return level;
            
            // Try all possible one-letter changes
            char[] chars = word.toCharArray();
            for (int j = 0; j < chars.length; j++) {
                char original = chars[j];
                for (char c = 'a'; c <= 'z'; c++) {
                    if (c == original) continue;
                    chars[j] = c;
                    String newWord = new String(chars);
                    if (wordSet.contains(newWord)) {
                        queue.offer(newWord);
                        wordSet.remove(newWord);
                    }
                }
                chars[j] = original;
            }
        }
        level++;
    }
    return 0;
}
```

### Pattern 3: Multi-Source BFS

Start BFS from multiple sources simultaneously.

```java
// Example: Rotten Oranges
public int orangesRotting(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    Queue<int[]> queue = new LinkedList<>();
    int fresh = 0;
    
    // Add all rotten oranges to queue
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) {
                queue.offer(new int[]{i, j});
            } else if (grid[i][j] == 1) {
                fresh++;
            }
        }
    }
    
    if (fresh == 0) return 0;
    
    int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    int minutes = 0;
    
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int[] curr = queue.poll();
            for (int[] dir : dirs) {
                int x = curr[0] + dir[0];
                int y = curr[1] + dir[1];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                    grid[x][y] = 2;
                    fresh--;
                    queue.offer(new int[]{x, y});
                }
            }
        }
        minutes++;
    }
    
    return fresh == 0 ? minutes - 1 : -1;
}
```

### Pattern 4: Backtracking in Graphs

Explore all paths with backtracking.

```java
// Find all paths from source to target
public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    path.add(0);
    dfsAllPaths(0, graph.length - 1, graph, path, result);
    return result;
}

private void dfsAllPaths(int node, int target, int[][] graph,
                         List<Integer> path, List<List<Integer>> result) {
    if (node == target) {
        result.add(new ArrayList<>(path));
        return;
    }
    
    for (int neighbor : graph[node]) {
        path.add(neighbor);
        dfsAllPaths(neighbor, target, graph, path, result);
        path.remove(path.size() - 1); // Backtrack
    }
}
```

---

## Practice Problems by Category

### 1. Graph Basics & Traversal

#### Easy
1. **[LeetCode 997] Find the Town Judge**
   - Concept: Indegree/Outdegree
   - Hint: Judge has indegree = n-1, outdegree = 0

2. **[LeetCode 1971] Find if Path Exists in Graph**
   - Concept: BFS/DFS, Union-Find
   - Hint: Simple connectivity check

3. **[LeetCode 1791] Find Center of Star Graph**
   - Concept: Graph properties
   - Hint: Center node appears in all edges

#### Medium
4. **[LeetCode 133] Clone Graph**
   - Concept: DFS/BFS with HashMap
   - Hint: Use HashMap to track original → clone mapping

5. **[LeetCode 797] All Paths From Source to Target**
   - Concept: DFS, Backtracking
   - Hint: DAG guarantees no cycles

6. **[LeetCode 1466] Reorder Routes to Make All Paths Lead to the City Zero**
   - Concept: DFS/BFS, Direction tracking
   - Hint: Count edges that need reversal

---

### 2. BFS Problems

#### Easy
7. **[LeetCode 1791] Find Center of Star Graph**
   - Concept: Graph structure
   - Hint: Center connects to all nodes

#### Medium
8. **[LeetCode 200] Number of Islands**
   - Concept: BFS/DFS on grid, Connected components
   - Hint: Each island is a connected component

9. **[LeetCode 994] Rotting Oranges**
   - Concept: Multi-source BFS
   - Hint: Start BFS from all rotten oranges simultaneously

10. **[LeetCode 1091] Shortest Path in Binary Matrix**
    - Concept: BFS shortest path
    - Hint: BFS guarantees shortest path in unweighted graph

11. **[LeetCode 127] Word Ladder**
    - Concept: BFS, Implicit graph
    - Hint: Each word is a node, edges connect words differing by 1 char

12. **[LeetCode 752] Open the Lock**
    - Concept: BFS, State space search
    - Hint: Each lock state is a node

13. **[LeetCode 1926] Nearest Exit from Entrance in Maze**
    - Concept: BFS shortest path
    - Hint: BFS from entrance, stop at first exit

14. **[LeetCode 542] 01 Matrix**
    - Concept: Multi-source BFS
    - Hint: Start from all 0s simultaneously

#### Hard
15. **[LeetCode 1293] Shortest Path in a Grid with Obstacles Elimination**
    - Concept: BFS with state (row, col, obstacles_remaining)
    - Hint: 3D visited array

16. **[LeetCode 847] Shortest Path Visiting All Nodes**
    - Concept: BFS with bitmask
    - Hint: State = (node, visited_mask)

---

### 3. DFS Problems

#### Medium
17. **[LeetCode 695] Max Area of Island**
    - Concept: DFS on grid
    - Hint: DFS to count connected 1s

18. **[LeetCode 417] Pacific Atlantic Water Flow**
    - Concept: DFS from boundaries
    - Hint: Run DFS from both oceans, find intersection

19. **[LeetCode 130] Surrounded Regions**
    - Concept: DFS from boundaries
    - Hint: Mark 'O's connected to boundary, flip others

20. **[LeetCode 547] Number of Provinces**
    - Concept: Connected components
    - Hint: Count connected components in graph

21. **[LeetCode 684] Redundant Connection**
    - Concept: Cycle detection, Union-Find
    - Hint: Find edge that creates cycle

22. **[LeetCode 399] Evaluate Division**
    - Concept: DFS with weights, Graph construction
    - Hint: Build weighted graph, DFS to find path product

23. **[LeetCode 1376] Time Needed to Inform All Employees**
    - Concept: DFS/BFS on tree
    - Hint: Find longest path from root

#### Hard
24. **[LeetCode 329] Longest Increasing Path in a Matrix**
    - Concept: DFS with memoization
    - Hint: Cache results to avoid recomputation

25. **[LeetCode 1192] Critical Connections in a Network**
    - Concept: Tarjan's algorithm, Bridges
    - Hint: Find bridges in graph

---

### 4. Topological Sort

#### Medium
26. **[LeetCode 207] Course Schedule**
    - Concept: Cycle detection in directed graph
    - Hint: Use Kahn's algorithm or DFS

27. **[LeetCode 210] Course Schedule II**
    - Concept: Topological sort
    - Hint: Return topological order if no cycle

28. **[LeetCode 802] Find Eventual Safe States**
    - Concept: Reverse topological sort
    - Hint: Nodes with no outgoing edges or leading to safe nodes

29. **[LeetCode 1136] Parallel Courses**
    - Concept: Topological sort with levels
    - Hint: Count levels in topological sort

#### Hard
30. **[LeetCode 269] Alien Dictionary** (Premium)
    - Concept: Topological sort, Graph construction
    - Hint: Build graph from character order, topological sort

31. **[LeetCode 310] Minimum Height Trees**
    - Concept: Topological sort from leaves
    - Hint: Remove leaves iteratively, centers remain

32. **[LeetCode 1203] Sort Items by Groups Respecting Dependencies**
    - Concept: Double topological sort
    - Hint: Sort groups, then items within groups

---

### 5. Shortest Path Algorithms

#### Medium
33. **[LeetCode 743] Network Delay Time**
    - Concept: Dijkstra's algorithm
    - Hint: Find max distance from source

34. **[LeetCode 787] Cheapest Flights Within K Stops**
    - Concept: Modified Dijkstra/Bellman-Ford
    - Hint: Track (node, cost, stops)

35. **[LeetCode 1514] Path with Maximum Probability**
    - Concept: Modified Dijkstra (maximize instead of minimize)
    - Hint: Use max-heap

36. **[LeetCode 1631] Path With Minimum Effort**
    - Concept: Binary search + BFS or Dijkstra
    - Hint: Minimize maximum edge weight in path

37. **[LeetCode 1368] Minimum Cost to Make at Least One Valid Path in a Grid**
    - Concept: 0-1 BFS or Dijkstra
    - Hint: Cost 0 for following arrow, cost 1 for changing

#### Hard
38. **[LeetCode 778] Swim in Rising Water**
    - Concept: Binary search + BFS or Dijkstra
    - Hint: Minimize maximum elevation in path

39. **[LeetCode 882] Reachable Nodes In Subdivided Graph**
    - Concept: Dijkstra with edge subdivision
    - Hint: Track distance and remaining capacity

40. **[LeetCode 2203] Minimum Weighted Subgraph With the Required Paths**
    - Concept: Three Dijkstra runs
    - Hint: Find paths from src1, src2, and reverse from dest

---

### 6. Union-Find (Disjoint Set)

#### Medium
41. **[LeetCode 547] Number of Provinces**
    - Concept: Connected components
    - Hint: Union friends, count components

42. **[LeetCode 684] Redundant Connection**
    - Concept: Cycle detection
    - Hint: Union edges, detect when both nodes already connected

43. **[LeetCode 721] Accounts Merge**
    - Concept: Union-Find with strings
    - Hint: Union emails, group by parent

44. **[LeetCode 990] Satisfiability of Equality Equations**
    - Concept: Union-Find
    - Hint: Union equal variables, check inequalities

45. **[LeetCode 1319] Number of Operations to Make Network Connected**
    - Concept: Connected components, Union-Find
    - Hint: Need n-1 edges to connect n components

46. **[LeetCode 1584] Min Cost to Connect All Points**
    - Concept: MST (Kruskal's with Union-Find)
    - Hint: Manhattan distance as edge weight

#### Hard
47. **[LeetCode 685] Redundant Connection II**
    - Concept: Union-Find, Directed graph
    - Hint: Handle two cases: node with 2 parents, or cycle

48. **[LeetCode 1697] Checking Existence of Edge Length Limited Paths**
    - Concept: Union-Find with sorting
    - Hint: Process queries and edges by weight

49. **[LeetCode 1998] GCD Sort of an Array**
    - Concept: Union-Find with GCD
    - Hint: Union numbers sharing GCD > 1

---

### 7. Minimum Spanning Tree

#### Medium
50. **[LeetCode 1584] Min Cost to Connect All Points**
    - Concept: Prim's or Kruskal's algorithm
    - Hint: Manhattan distance between points

51. **[LeetCode 1135] Connecting Cities With Minimum Cost**
    - Concept: MST (Kruskal's or Prim's)
    - Hint: Standard MST problem

#### Hard
52. **[LeetCode 1168] Optimize Water Distribution in a Village**
    - Concept: MST with virtual node
    - Hint: Add virtual node for wells

---

### 8. Strongly Connected Components

#### Hard
53. **[LeetCode 1192] Critical Connections in a Network**
    - Concept: Tarjan's algorithm
    - Hint: Find bridges

54. **[LeetCode 1568] Minimum Number of Days to Disconnect Island**
    - Concept: Articulation points
    - Hint: Check if removing any cell disconnects island

---

### 9. Bipartite Graphs

#### Medium
55. **[LeetCode 785] Is Graph Bipartite?**
    - Concept: 2-coloring with BFS/DFS
    - Hint: Color nodes alternately, check conflicts

56. **[LeetCode 886] Possible Bipartition**
    - Concept: Bipartite check
    - Hint: Build graph of dislikes, check if bipartite

---

### 10. Advanced Graph Problems

#### Hard
57. **[LeetCode 815] Bus Routes**
    - Concept: BFS on meta-graph
    - Hint: Nodes are bus routes, not stops

58. **[LeetCode 864] Shortest Path to Get All Keys**
    - Concept: BFS with state (row, col, keys_bitmask)
    - Hint: 3D state space

59. **[LeetCode 1345] Jump Game IV**
    - Concept: BFS with value grouping
    - Hint: Group indices by value

60. **[LeetCode 127] Word Ladder**
    - Concept: BFS, Bidirectional search
    - Hint: Meet in the middle for optimization

61. **[LeetCode 126] Word Ladder II**
    - Concept: BFS + DFS, Backtracking
    - Hint: BFS to find shortest distance, DFS to construct paths

62. **[LeetCode 332] Reconstruct Itinerary**
    - Concept: Eulerian path, Hierholzer's algorithm
    - Hint: Find path visiting all edges exactly once

63. **[LeetCode 1579] Remove Max Number of Edges to Keep Graph Fully Traversable**
    - Concept: Union-Find with two sets
    - Hint: Process type 3 edges first

---

### 11. Grid-Based Graph Problems

#### Medium
64. **[LeetCode 200] Number of Islands**
    - Concept: Connected components in grid
    - Hint: DFS/BFS for each unvisited land cell

65. **[LeetCode 286] Walls and Gates**
    - Concept: Multi-source BFS
    - Hint: Start from all gates simultaneously

66. **[LeetCode 1254] Number of Closed Islands**
    - Concept: DFS/BFS with boundary check
    - Hint: Ignore islands touching boundary

67. **[LeetCode 1020] Number of Enclaves**
    - Concept: DFS from boundaries
    - Hint: Mark boundary-connected cells, count rest

68. **[LeetCode 1905] Count Sub Islands**
    - Concept: DFS comparison
    - Hint: Check if island in grid2 is subset of grid1

#### Hard
69. **[LeetCode 827] Making A Large Island**
    - Concept: Connected components with merging
    - Hint: Label islands, try flipping each 0

70. **[LeetCode 1263] Minimum Moves to Move a Box to Their Target Location**
    - Concept: BFS with complex state
    - Hint: State = (box_pos, player_pos)

---

### 12. Tree as Graph Problems

#### Medium
71. **[LeetCode 1443] Minimum Time to Collect All Apples in a Tree**
    - Concept: DFS on tree
    - Hint: Count edges to/from nodes with apples

72. **[LeetCode 1557] Minimum Number of Vertices to Reach All Nodes**
    - Concept: Graph properties
    - Hint: Nodes with indegree 0

73. **[LeetCode 2359] Find Closest Node to Given Two Nodes**
    - Concept: BFS/DFS from two sources
    - Hint: Find distances from both nodes, minimize max

#### Hard
74. **[LeetCode 2246] Longest Path With Different Adjacent Characters**
    - Concept: DFS on tree, DP
    - Hint: Track two longest paths from each node

---

### 13. Additional Important Problems

#### Medium
75. **[LeetCode 1129] Shortest Path with Alternating Colors**
    - Concept: BFS with color state
    - Hint: State = (node, last_edge_color)

76. **[LeetCode 1306] Jump Game III**
    - Concept: BFS/DFS on implicit graph
    - Hint: Each index is a node

77. **[LeetCode 1462] Course Schedule IV**
    - Concept: Transitive closure, Floyd-Warshall
    - Hint: Check if path exists between pairs

78. **[LeetCode 1615] Maximal Network Rank**
    - Concept: Graph properties
    - Hint: Sum degrees, subtract 1 if edge exists

79. **[LeetCode 2092] Find All People With Secret**
    - Concept: Union-Find with time ordering
    - Hint: Process meetings by time, reset unions

#### Hard
80. **[LeetCode 2097] Valid Arrangement of Pairs**
    - Concept: Eulerian path
    - Hint: Find path using all edges exactly once

---

## Interview Tips

### 1. Problem Identification

**Ask yourself:**
- Is this a graph problem? (relationships, connections, dependencies)
- Is the graph directed or undirected?
- Is it weighted or unweighted?
- Is it a grid/matrix problem? (can be treated as graph)
- Are there cycles?

### 2. Choose the Right Algorithm

| Problem Type | Algorithm | Time Complexity |
|-------------|-----------|-----------------|
| Shortest path (unweighted) | BFS | O(V + E) |
| Shortest path (weighted, non-negative) | Dijkstra | O((V+E) log V) |
| Shortest path (negative weights) | Bellman-Ford | O(V × E) |
| All pairs shortest path | Floyd-Warshall | O(V³) |
| Cycle detection | DFS or Union-Find | O(V + E) |
| Connected components | DFS/BFS or Union-Find | O(V + E) |
| Topological sort | DFS or Kahn's | O(V + E) |
| Minimum spanning tree | Kruskal's or Prim's | O(E log E) |
| Bipartite check | BFS/DFS with coloring | O(V + E) |

### 3. Common Mistakes to Avoid

1. **Not handling disconnected graphs** - Always iterate through all vertices
2. **Forgetting to mark visited** - Leads to infinite loops
3. **Wrong base cases in recursion** - Check boundaries carefully
4. **Not considering edge cases** - Empty graph, single node, no edges
5. **Incorrect graph representation** - Choose based on density
6. **Not handling directed vs undirected** - Be clear about edge directions
7. **Integer overflow** - Use appropriate data types for distances

### 4. Optimization Techniques

1. **Early termination** - Stop when target found (BFS shortest path)
2. **Bidirectional search** - Meet in the middle
3. **Memoization** - Cache results (DFS with DP)
4. **Pruning** - Skip impossible paths
5. **Choose right data structure** - Priority queue for Dijkstra, deque for 0-1 BFS

### 5. Interview Communication

1. **Clarify the problem**
   - Ask about graph properties (directed/undirected, weighted/unweighted)
   - Confirm input format
   - Discuss edge cases

2. **Explain your approach**
   - State which algorithm you'll use and why
   - Discuss time and space complexity
   - Walk through a small example

3. **Code incrementally**
   - Start with helper functions
   - Test as you go
   - Handle edge cases

4. **Optimize if needed**
   - Discuss trade-offs
   - Mention alternative approaches

### 6. Time Complexity Cheat Sheet

| Operation | Adjacency Matrix | Adjacency List |
|-----------|------------------|----------------|
| Add vertex | O(V²) | O(1) |
| Add edge | O(1) | O(1) |
| Remove vertex | O(V²) | O(V + E) |
| Remove edge | O(1) | O(E) |
| Check if edge exists | O(1) | O(V) |
| Find all neighbors | O(V) | O(degree) |
| Space | O(V²) | O(V + E) |

### 7. Key Insights

1. **BFS finds shortest path in unweighted graphs** - Always use BFS for this
2. **DFS is good for path finding and cycle detection** - Simpler to implement recursively
3. **Union-Find excels at dynamic connectivity** - Near constant time operations
4. **Topological sort only works on DAGs** - Check for cycles first
5. **Grid problems are graph problems** - Each cell is a node
6. **Multi-source BFS** - Start from all sources simultaneously
7. **State space as graph** - Each state is a node, transitions are edges

### 8. Practice Strategy

1. **Start with basics** - Master BFS and DFS first
2. **Solve by category** - Focus on one pattern at a time
3. **Understand, don't memorize** - Know why algorithms work
4. **Draw it out** - Visualize graphs on paper
5. **Code without looking** - Practice implementation from scratch
6. **Time yourself** - Simulate interview conditions
7. **Review mistakes** - Understand what went wrong

### 9. Common Interview Questions

**Conceptual:**
- Explain BFS vs DFS
- When to use Dijkstra vs Bellman-Ford?
- How does Union-Find work?
- What is a topological sort?
- Difference between Prim's and Kruskal's?

**Coding:**
- Implement BFS/DFS
- Detect cycle in directed/undirected graph
- Find shortest path
- Clone a graph
- Number of islands

### 10. Resources for Further Practice

- **LeetCode:** Graph tag (300+ problems)
- **GeeksforGeeks:** Graph algorithms tutorials
- **Visualgo:** Algorithm visualizations
- **Graph Theory Playlist:** YouTube tutorials
- **Competitive Programming:** Codeforces, AtCoder

---

## Summary

Graphs are fundamental to computer science and appear frequently in interviews. Master these key concepts:

1. **Graph representations** (adjacency list vs matrix)
2. **Traversal algorithms** (BFS and DFS)
3. **Shortest path algorithms** (Dijkstra, Bellman-Ford)
4. **Topological sort** (for DAGs)
5. **Union-Find** (for connectivity)
6. **MST algorithms** (Kruskal's, Prim's)
7. **Cycle detection** (directed and undirected)
8. **Common patterns** (grid problems, multi-source BFS, backtracking)

**Practice consistently**, understand the underlying concepts, and you'll be well-prepared for any graph problem in interviews!

---


