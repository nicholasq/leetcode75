# Graphs DFS

## [Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def canVisitAllRooms(rooms):
    n = len(rooms)
    visited = [False] * n
    visited[0] = True  # Start from room 0

    # Using a stack for DFS
    stack = [0]

    while stack:
        room = stack.pop()

        # Explore all keys in the current room
        for key in rooms[room]:
            if not visited[key]:
                visited[key] = True
                stack.append(key)

    # Check if all rooms were visited
    return all(visited)
```

### Computational Complexity
- **Time Complexity**: O(n + e), where n is the number of rooms and e is the total number of keys (edges in the graph)
- **Space Complexity**: O(n) for the visited array and stack in the worst case

### Keywords and Hints
- "Rooms labeled from 0 to n-1" suggests a graph structure, where rooms are nodes
- "Keys that you can obtain" indicates edges between nodes (rooms)
- "Visit all the rooms" indicates a graph traversal problem
- "Cannot enter a locked room without having its key" suggests a reachability problem
- These hints point to a graph traversal algorithm like DFS or BFS being the appropriate solution

### Key insights
1. The problem can be represented as a graph traversal problem where:
   - Rooms are nodes in a graph
   - Keys represent directed edges from one room to another
2. We need to determine if all nodes (rooms) are reachable from the starting node (room 0)
3. A simple DFS or BFS traversal starting from room 0 can solve this problem
4. If we can visit all rooms during traversal, return true; otherwise, return false

### Solution Intuition
The intuition is to explore rooms in a depth-first manner. We start from room 0 (which is unlocked) and collect all available keys. For each key we find, we unlock the corresponding room and explore it further to find more keys. If at the end of our exploration, we've visited all rooms, then the answer is true. Otherwise, some rooms remain locked and inaccessible, so the answer is false.

### Solution explanation
1. Initialize a visited array to keep track of rooms we have visited
2. Mark room 0 as visited (since we start there)
3. Use a stack for DFS traversal, starting from room 0
4. While the stack is not empty:
   - Pop a room from the stack
   - For each key in the room:
     - If the room the key unlocks hasn't been visited yet:
       - Mark it as visited
       - Add it to the stack for further exploration
5. After traversal, check if all rooms have been visited
   - If yes, return true
   - If no, return false

### Analogy
Think of this problem as exploring a dungeon with locked chambers. You start in the entrance hall (room 0) which is unlocked. Inside this hall, you find some keys labeled with room numbers. You can use each key to unlock the corresponding chamber.

When you enter a new chamber, you might find more keys that unlock other chambers. Your goal is to determine if you can access all chambers in the dungeon.

Some chambers might contain keys to chambers you've already visited, which isn't helpful. Some dungeons might have chambers with keys locked inside them (like in Example 2), making those chambers and potentially others inaccessible.

This is exactly like a graph traversal problem: you're exploring connected nodes and seeing if all nodes are reachable from your starting point.

## [Number of Provinces](https://leetcode.com/problems/number-of-provinces/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
class Solution:
    def findCircleNum(self, isConnected: list[list[int]]) -> int:
        n = len(isConnected)
        visited = [False] * n
        provinces = 0

        def dfs(city):
            visited[city] = True
            for neighbor in range(n):
                if isConnected[city][neighbor] == 1 and not visited[neighbor]:
                    dfs(neighbor)

        for city in range(n):
            if not visited[city]:
                provinces += 1
                dfs(city)

        return provinces
```

### Computational Complexity
- Time Complexity: O(n²) where n is the number of cities. We potentially visit each city once and check all of its connections, which is an n×n operation in the worst case.
- Space Complexity: O(n) for the visited array and the recursive call stack (in the worst case of a chain-like graph).

### Keywords and Hints
- "Directly or indirectly connected" suggests finding connected components in a graph
- "Province is a group of connected cities" indicates each province is a connected component
- The matrix representation is an adjacency matrix of a graph
- "Return the total number of provinces" means counting connected components

These keywords point to a graph traversal problem where we need to count connected components, making DFS or BFS a natural choice.

### Key insights
1. The problem can be modeled as a graph where cities are nodes and connections are edges
2. The isConnected matrix is an adjacency matrix representation of this graph
3. A province is a connected component in the graph
4. To find the number of provinces, we need to count the number of connected components

### Solution Intuition
The intuition is to perform a graph traversal (like DFS) starting from an unvisited city. During this traversal, we mark all cities in the same province as visited. After completing the traversal, we know one province. We repeat this process starting from any remaining unvisited city until all cities are visited. The number of times we had to start a new traversal is the number of provinces.

### Solution explanation
1. Initialize a visited array to keep track of cities we've already processed
2. Initialize a counter for the number of provinces
3. For each unvisited city:
   - Increment the province counter
   - Perform DFS to visit all cities in the same province
   - Mark all these cities as visited
4. Return the province counter

The DFS traversal explores all cities connected (directly or indirectly) to the starting city, effectively identifying one complete province at a time.

### Analogy
Imagine a map with different islands (provinces) and each island has cities. The connections between cities are like bridges. Within an island, you can travel from any city to any other by using one or more bridges, but you can't travel between islands.

To count the number of islands, you start at any city you haven't visited, then explore the entire island by crossing bridges and marking each city as visited. Once you can't go any further, you've mapped one island. You then look for any unvisited city, which must be on another island, and repeat the process.

Each time you have to "boat" to a new unvisited city to start exploring, you've found a new island. The number of times you need to start a new exploration equals the number of islands (provinces) on the map.

This analogy fits because like islands are separated by water, provinces are separated by the absence of connections between their cities.

## [Reorder Routes to Make All Paths Lead to the City Zero](https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def minReorder(n, connections):
    # Build adjacency list representation of the graph
    # We'll store both original edges and their reverse directions
    # For original edges (a->b), we'll use positive values (b)
    # For reverse edges (b->a), we'll use negative values (-b-1)
    adj = [[] for _ in range(n)]

    for a, b in connections:
        adj[a].append(b)      # Original edge (a->b)
        adj[b].append(-a-1)   # Reverse edge (b->a) stored with negative value

    def dfs(node, parent):
        count = 0
        for neighbor in adj[node]:
            # Skip parent node to avoid cycles
            if abs(neighbor) != parent:
                # If neighbor is positive, it's an original edge pointing away from 0
                # so we need to reorient it
                if neighbor >= 0:
                    count += 1 + dfs(neighbor, node)
                else:
                    # Negative index means original edge points toward 0, so no change needed
                    count += dfs(-neighbor-1, node)
        return count

    # Start DFS from city 0
    return dfs(0, -1)
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the number of cities. We visit each city exactly once in our DFS.
- **Space Complexity**: O(n) for the adjacency list and the recursive call stack.

### Keywords and Hints
- "Network forms a tree" - This is crucial as it means there are no cycles and exactly n-1 edges.
- "One way to travel" - Roads are directed, which is the core of the problem.
- "Each city can reach city 0" - Our goal is to ensure all paths lead to the capital.
- "Minimum number of edges changed" - We need to find which edges are already correct and which need to be flipped.

### Key insights
1. Since the network forms a tree, there's exactly one path between any two cities.
2. We can think of this as starting from city 0 and determining the correct direction for each edge.
3. If an edge is already pointing from the parent to the child in our DFS, it's correctly oriented. Otherwise, it needs to be changed.
4. We can use DFS to traverse the tree from the capital outward.

### Solution Intuition
Imagine all cities need to have a path to city 0 (the capital). We can mentally redefine the problem: if we view the tree from city 0 outward, all edges should point away from city 0. If we find an edge that points toward city 0, it's already correctly oriented. If it points away from city 0, we need to change its direction.

To identify this, we'll perform a DFS starting from city 0 and check each edge's original direction.

### Solution explanation
1. Build an adjacency list representation of the graph, preserving information about the original direction of each edge.
2. Perform a DFS starting from city 0.
3. When exploring an edge, if the original direction was from parent to child in our DFS traversal, we need to flip it. Otherwise, it's already correctly oriented.
4. Count the number of edges that need to be flipped and return the result.

Our implementation encodes the original direction by using positive indices for original edges and negative indices for reverse edges. This lets us easily determine during the DFS which edges need to be flipped.

### Analogy
Think of this problem as rerouting a river system to flow from a central lake (city 0) outward to all other lakes.

If you start at the central lake and explore each connecting river:
- Rivers that naturally flow outward from the central lake (away from city 0) are already correctly oriented.
- Rivers that flow inward toward the central lake (toward city 0) need to be dammed and redirected to flow in the opposite direction.

The goal is to create a system where water flows from the central lake to every other lake, requiring the minimum number of river redirections. Just as we'd traverse the river system to check each connection, our DFS explores the city network to count how many roads need to be reoriented.

## [Evaluate Division](https://leetcode.com/problems/evaluate-division/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
from collections import defaultdict, deque

def calcEquation(equations, values, queries):
    # Build graph
    graph = defaultdict(dict)
    for (numerator, denominator), value in zip(equations, values):
        graph[numerator][denominator] = value
        graph[denominator][numerator] = 1.0 / value

    # Process queries using BFS
    results = []
    for numerator, denominator in queries:
        if numerator not in graph or denominator not in graph:
            results.append(-1.0)
            continue

        if numerator == denominator:
            results.append(1.0)
            continue

        visited = set()
        queue = deque([(numerator, 1.0)])
        found = False

        while queue and not found:
            node, curr_product = queue.popleft()

            if node == denominator:
                results.append(curr_product)
                found = True
                break

            visited.add(node)

            for neighbor, value in graph[node].items():
                if neighbor not in visited:
                    queue.append((neighbor, curr_product * value))

        if not found:
            results.append(-1.0)

    return results
```

### Computational Complexity
- Time Complexity: O(M * N), where M is the number of queries and N is the number of equations. In the worst case, each query might need to traverse the entire graph.
- Space Complexity: O(N), for storing the graph and the BFS queue. The size of the graph is proportional to the number of equations.

### Keywords and Hints
- "Variable pairs equations" and "equation Ai / Bi = values[i]" suggest a relationship between variables
- Need to find "Cj / Dj" based on given equations - this hints at transitivity
- "Evaluating the queries" suggests we need to find paths through existing relationships
- These keywords point to a graph problem where variables are nodes and division relationships are edges

### Key insights
1. This problem can be modeled as a graph where:
   - Each variable is a node
   - If a/b = 2, there's a directed edge from a to b with weight 2, and from b to a with weight 1/2
2. Finding the value of a query c/d is equivalent to finding a path from c to d in the graph
3. The product of weights along the path gives the answer to the query
4. BFS or DFS can be used to find such paths

### Solution Intuition
Imagine variables as cities and division values as exchange rates between currencies in those cities. If you know the exchange rate from USD to EUR, and from EUR to GBP, you can calculate the exchange rate from USD to GBP by multiplying the intermediate rates. Similarly, we're finding "paths" of known division relationships to solve for unknown ones.

### Solution explanation
1. Build a graph where each variable is a node, and edges represent known division relationships
2. For each equation a/b = v, add two directed edges:
   - From a to b with weight v
   - From b to a with weight 1/v
3. For each query c/d:
   - If either variable doesn't exist in our graph, return -1.0
   - If c equals d, return 1.0 (any number divided by itself is 1)
   - Use BFS to find a path from c to d, multiplying the weights along the path
   - If no path exists, return -1.0

### Analogy
Think of this problem as a currency exchange network. Each variable is a currency, and the values represent exchange rates between currencies.

For example, if 1 USD = 2 EUR and 1 EUR = 3 GBP, then:
- To convert from USD to GBP, you'd go USD → EUR → GBP, with a combined rate of 2 × 3 = 6
- To convert from GBP to USD, you'd go GBP → EUR → USD, with a combined rate of (1/3) × (1/2) = 1/6

In this analogy:
- Finding a query value is like finding the exchange rate between two currencies
- Multiplying values along a path corresponds to converting through multiple currencies
- Returning -1.0 is like saying "there's no known way to exchange between these currencies"

This analogy fits perfectly because division has the same transitive property as currency exchange rates: if you know a/b and b/c, you can calculate a/c by multiplication.
