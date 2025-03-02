# Binary Tree Breadth First Search

## [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []

        result = []
        queue = deque([root])

        while queue:
            level_size = len(queue)

            for i in range(level_size):
                node = queue.popleft()

                # If it's the last node in the current level, add it to the result
                if i == level_size - 1:
                    result.append(node.val)

                # Add child nodes to the queue (left first, then right)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

        return result
```

### Computational Complexity
- Time complexity: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- Space complexity: O(w), where w is the maximum width of the tree. In the worst case, this could be O(n/2) which simplifies to O(n).

### Keywords and Hints
- "Right side view" - This suggests that we need to find nodes that would be visible from the right side, which are the rightmost nodes at each level of the tree.
- "Ordered from top to bottom" - This indicates that we need to maintain the level order of nodes, suggesting a level-order traversal.
- The problem is fundamentally about capturing the last node at each level of the tree, which is exactly what level-order traversal (BFS) is designed to handle efficiently.

### Key Insights
- The right side view consists of the rightmost node at each level of the tree.
- A level-order traversal (BFS) naturally processes the tree level by level.
- By keeping track of the last node processed at each level, we can build the right side view.
- Using a queue data structure for BFS ensures we process nodes in the correct order.

### Solution Intuition
- Imagine taking a snapshot of the tree from the right side - you'd only see the rightmost node at each level.
- We can use a level-order traversal to visit the tree level by level.
- For each level, we only add the last node we process to our result.
- This ensures we capture exactly what would be visible from the right side.

### Solution Explanation
1. If the root is null, return an empty array.
2. Initialize a queue with the root node and an empty result array.
3. While the queue is not empty:
   a. Determine the number of nodes at the current level (size of queue).
   b. Process each node at the current level.
   c. For each node, add its left and right children to the queue (if they exist).
   d. Add the value of the last node at each level to the result array.
4. Return the result array containing the right side view.

### Analogy
Imagine you're standing at the edge of a forest looking at rows of trees. Each row has trees of varying heights, but you can only see the tallest tree in each row (or the rightmost tree if all are the same height).

In our algorithm, we're like a drone flying over this forest row by row from top to bottom. For each row:
1. We count how many trees are in the row (level_size).
2. We examine each tree from left to right.
3. We take note of the last tree in each row, as this is the one visible from the right side.
4. Then we move to the next row and repeat.

This systematic approach ensures we capture exactly what an observer standing to the right of the forest would see - the rightmost tree at each level.

## [Maximum Level Sum of a Binary Tree](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0

        queue = deque([root])
        max_sum = float('-inf')
        max_level = 1
        current_level = 1

        while queue:
            level_size = len(queue)
            level_sum = 0

            for _ in range(level_size):
                node = queue.popleft()
                level_sum += node.val

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            if level_sum > max_sum:
                max_sum = level_sum
                max_level = current_level

            current_level += 1

        return max_level
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the binary tree. We visit each node exactly once.
- **Space Complexity**: O(w), where w is the maximum width of the tree. In the worst case, the queue can hold all nodes at the widest level of the tree.

### Keywords and Hints
- **"level of a binary tree"** suggests that we need to process the tree level by level, indicating a breadth-first search approach.
- **"sum of all the values of nodes at level x"** tells us we need to track the sum for each level.
- **"smallest level x such that the sum is maximal"** indicates we need to keep track of both the maximum sum and its corresponding level.
- The phrase **"level of its root is 1"** tells us to use 1-indexed level numbering, not 0-indexed.

### Key insights
- This is a classic breadth-first search (BFS) problem where we need to process the tree level by level.
- We need to keep track of the sum of node values at each level as we traverse the tree.
- When we find a level with a sum greater than our current maximum, we update both the maximum sum and the level number.
- If multiple levels have the same maximum sum, we need to return the smallest level number.

### Solution Intuition
The intuition is to traverse the tree level by level using BFS, calculating the sum at each level. As we go, we keep track of the level with the maximum sum. If we find a new level with a sum greater than our current maximum, we update our answer to that level. Since we're traversing in level order (from top to bottom), if we encounter a level with the same maximum sum as a previous level, we'll keep the smaller level number.

### Solution explanation
1. Initialize a queue with the root node and set the current level to 1.
2. Use variables to track the maximum sum found and its corresponding level.
3. Process the tree level by level:
   - For each level, compute the sum of all node values at that level.
   - If the sum is greater than the current maximum sum, update the maximum sum and its level.
   - Add all child nodes to the queue for the next level's processing.
4. Continue until all levels are processed.
5. Return the level with the maximum sum.

### Analogy
Imagine you're organizing a charity collection in a multi-story building where each floor represents a level of the tree. Each resident (node) donates a certain amount (node value), which can be positive or negative (some might take money instead of giving!).

You start from the top floor and work your way down, collecting all donations on each floor and keeping a record of the total amount collected on each floor. Your task is to determine which floor had the highest total donation amount. If two floors have the same total, you choose the higher floor (lower level number) because it's closer to the entrance and easier for reporting purposes.

The BFS approach ensures that you fully complete collection on one floor before moving to the next, allowing you to accurately track the total donations by floor.
