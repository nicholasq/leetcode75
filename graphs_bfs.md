# Graphs BFS

## [Nearest Exit from Entrance in Maze](https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
from collections import deque

def nearestExit(maze, entrance):
    rows, cols = len(maze), len(maze[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # right, down, left, up

    # Mark the entrance as visited
    start_row, start_col = entrance
    queue = deque([(start_row, start_col, 0)])  # (row, col, steps)
    maze[start_row][start_col] = '+'  # Mark as visited

    while queue:
        row, col, steps = queue.popleft()

        # Check all four directions
        for dr, dc in directions:
            new_row, new_col = row + dr, col + dc

            # Check if the new position is valid
            if 0 <= new_row < rows and 0 <= new_col < cols and maze[new_row][new_col] == '.':
                # Check if it's an exit (on the border) and not the entrance
                if new_row == 0 or new_row == rows - 1 or new_col == 0 or new_col == cols - 1:
                    return steps + 1

                # Mark as visited and add to queue
                maze[new_row][new_col] = '+'
                queue.append((new_row, new_col, steps + 1))

    return -1  # No exit found
```

### Computational Complexity
- **Time Complexity**: O(m * n), where m is the number of rows and n is the number of columns in the maze. In the worst case, we may need to visit every cell in the maze.
- **Space Complexity**: O(m * n) for the queue in the worst case, where we might need to store all cells at once.

### Keywords and Hints
- **Maze navigation**: This suggests we need to explore the maze in some systematic way.
- **Nearest exit**: "Nearest" indicates we need to find the minimum distance, which suggests a breadth-first search (BFS).
- **Steps in shortest path**: This confirms that BFS is appropriate, as it finds the shortest path in an unweighted graph.
- **Border of the maze**: We need to identify cells that are on the edge of the maze as potential exits.

### Key insights
1. This is a shortest path problem where each cell transition has a cost of 1, making BFS the ideal algorithm.
2. We need to start from the entrance and explore outward, level by level.
3. The first border cell we encounter (that isn't the entrance) will be the nearest exit.
4. We can modify the maze directly to mark visited cells, avoiding a separate visited set.

### Solution Intuition
The problem asks for the shortest path from the entrance to any exit. In graph theory, when all edges have equal weight (in this case, each step has a weight of 1), BFS provides the shortest path from a starting node to all other nodes.

By using BFS, we explore cells in layers - first all cells 1 step away, then all cells 2 steps away, and so on. This guarantees that when we first encounter an exit, it will be the one with the minimum number of steps from the entrance.

### Solution explanation
1. We use a queue to implement BFS, starting with the entrance cell and a step count of 0.
2. We mark the entrance as visited by changing its value in the maze.
3. For each cell we pop from the queue, we explore its four adjacent cells.
4. If an adjacent cell is valid (within bounds and not a wall) and not yet visited:
   - If it's on the border of the maze, we've found an exit and return the current step count + 1.
   - Otherwise, we mark it as visited and add it to the queue with an incremented step count.
5. If we exhaust the queue without finding an exit, we return -1.

### Analogy
Think of the maze as a city with city blocks (cells), where some blocks have buildings (walls) and others are empty streets. You start at a specific intersection (the entrance) and want to find the quickest way to leave the city (reach an exit).

You send out scouts in all four directions (north, south, east, west). Each scout can move at the same speed, and they all leave at the same time. After one minute, all scouts have checked intersections one block away from the start. After two minutes, they've checked intersections two blocks away, and so on.

The scouts communicate with each other to avoid checking the same intersection twice (marking cells as visited). The first scout to reach the city limits (the border of the maze) reports back the number of minutes (steps) it took, which is guaranteed to be the fastest route out of the city.

This is exactly how BFS works - exploring in concentric waves from the starting point, guaranteeing that the first time we encounter a destination, we've found the shortest path to it.

## [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
from collections import deque

def orangesRotting(grid):
    rows, cols = len(grid), len(grid[0])
    fresh_oranges = 0
    queue = deque()

    # Count fresh oranges and add rotten oranges to queue
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 1:
                fresh_oranges += 1
            elif grid[r][c] == 2:
                queue.append((r, c, 0))  # (row, col, time)

    # If no fresh oranges, return 0
    if fresh_oranges == 0:
        return 0

    # BFS to simulate rotting process
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # right, down, left, up
    max_time = 0

    while queue:
        r, c, time = queue.popleft()
        max_time = max(max_time, time)

        for dr, dc in directions:
            nr, nc = r + dr, c + dc

            # Check boundaries and if it's a fresh orange
            if (0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1):
                grid[nr][nc] = 2  # Mark as rotten
                fresh_oranges -= 1
                queue.append((nr, nc, time + 1))

    # If there are still fresh oranges, return -1
    if fresh_oranges > 0:
        return -1

    return max_time
```

### Computational Complexity
- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns in the grid. We visit each cell at most once.
- **Space Complexity**: O(m × n) for the queue in the worst case where all cells contain rotten oranges.

### Keywords and Hints
- "4-directionally adjacent" indicates a grid traversal problem where we need to consider the four adjacent cells (up, down, left, right).
- "Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten" suggests a wave-like propagation which is a strong hint for using Breadth-First Search (BFS).
- "Return the minimum number of minutes" suggests we need to find the shortest time, which aligns with BFS giving us the shortest path in an unweighted graph.

### Key insights
1. This is a multi-source BFS problem where we start from all rotten oranges simultaneously.
2. We need to keep track of time (minutes) during the BFS traversal.
3. We must count all fresh oranges initially and decrement this count as oranges rot.
4. If any fresh oranges remain after BFS completes, it means some oranges can never rot, so we return -1.

### Solution Intuition
Think of the rotting process as a "wave" of infection spreading from multiple sources (initial rotten oranges) at the same time. BFS naturally models this simultaneous spreading process where each "wave" represents one minute of time passing. By using BFS starting from all rotten oranges at once, we can track how many minutes it takes for the rotting to reach the farthest fresh orange.

### Solution explanation
1. Count the number of fresh oranges and collect positions of all rotten oranges in a queue.
2. Perform BFS starting from all initially rotten oranges:
   - For each rotten orange, look at its 4 adjacent cells.
   - If an adjacent cell has a fresh orange, mark it as rotten, decrement fresh orange count, and add it to the queue with time incremented by 1.
3. Keep track of the maximum time seen during BFS.
4. After BFS completes:
   - If there are still fresh oranges left, return -1 (some oranges can never rot).
   - Otherwise, return the maximum time, which is the minimum minutes needed.

### Analogy
Imagine a disease outbreak in multiple cities (initial rotten oranges) in a country where cities are connected by highways (4-directional connections). The disease spreads from infected cities to adjacent cities every day (minute). We want to know how many days it will take for the entire country to be infected.

- Cities are the cells in our grid
- Infected cities are rotten oranges
- Healthy cities are fresh oranges
- Empty land represents empty cells
- The disease spreading each day represents the rotting process each minute

Using BFS is like tracking the disease spread day by day, level by level, from all initially infected cities simultaneously. We advance one day at a time, marking newly infected cities, until either all cities are infected or we realize some cities are isolated and can never be infected.
