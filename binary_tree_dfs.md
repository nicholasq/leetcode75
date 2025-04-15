# Binary Tree Depth First Search

## [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # Base case: if the root is None, the depth is 0
        if not root:
            return 0

        # Recursive case: the maximum depth is the maximum of the depths of the left and right subtrees, plus 1 for the current node
        left_depth = self.maxDepth(root.left)
        right_depth = self.maxDepth(root.right)

        return max(left_depth, right_depth) + 1
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is the maximum size of the call stack during recursion. In the worst case of a completely unbalanced tree, h can be O(n), but in a balanced tree, h would be O(log n).

### Keywords and Hints
- **"maximum depth"**: This indicates we need to find the longest path from root to leaf
- **"binary tree"**: Suggests a recursive approach since binary trees have a recursive structure
- **"number of nodes along the longest path"**: Clarifies that we're counting nodes, not edges
- **Base case of empty tree**: The problem states the number of nodes could be 0, so we need to handle the empty tree case

### Key insights
- The depth of a tree can be defined recursively: the maximum depth of a binary tree is 1 + the maximum depth of its deepest subtree
- We can solve this using either recursion or iteration (like DFS or BFS)
- Each recursive call computes the depth of a subtree
- Leaf nodes have a depth of 1, and null nodes have a depth of 0

### Solution Intuition
The intuition is to use the recursive nature of binary trees. If we know the maximum depth of the left subtree and the maximum depth of the right subtree, then the maximum depth of the entire tree is 1 plus the maximum of these two depths. The base case is when we reach a null node, which has a depth of 0.

### Solution explanation
1. If the root is null, return 0 as the depth
2. Otherwise, recursively find the maximum depth of the left subtree
3. Recursively find the maximum depth of the right subtree
4. Return 1 (counting the current node) plus the maximum of the depths of the left and right subtrees

This recursive approach naturally traverses the entire tree, computing the maximum depth bottom-up. When it reaches leaf nodes, it returns 1, and as it backtracks, it builds up the depth by taking the maximum of the left and right subtree depths at each level.

### Analogy
Think of a binary tree as a family genealogy tree, and you want to find how many generations there are from the oldest ancestor (the root) to the youngest descendant (the leaf farthest from the root).

To solve this, you could ask each of your two children: "How many generations are there in your family line?" Each child would then ask their children the same question, and so on. When you reach someone with no children (a leaf node), they would answer "just me, so one generation."

As the answers come back up the family tree, each person would take the maximum generation count from their two children and add 1 (to include themselves). This continues until the original ancestor gets answers from both of their children, takes the larger number, adds 1 for themselves, and that's the total number of generations.

This mirrors our recursive algorithm: we ask each subtree for its depth, take the maximum, and add 1 for the current node.

## [Leaf Similar Trees](https://leetcode.com/problems/leaf-similar-trees/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def leafSimilar(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:
        def get_leaf_sequence(root):
            leaves = []

            def dfs(node):
                if not node:
                    return

                # If node is a leaf (no children), add its value to the leaves list
                if not node.left and not node.right:
                    leaves.append(node.val)
                    return

                # Otherwise, traverse left and right subtrees
                dfs(node.left)
                dfs(node.right)

            dfs(root)
            return leaves

        # Get leaf sequences for both trees and compare them
        return get_leaf_sequence(root1) == get_leaf_sequence(root2)
```

### Computational Complexity
- **Time Complexity**: O(n1 + n2), where n1 and n2 are the number of nodes in the two trees. We need to traverse each node in both trees once.
- **Space Complexity**: O(h1 + h2 + l1 + l2), where h1 and h2 are the heights of the trees (for recursion stack), and l1 and l2 are the number of leaves in each tree (for storing the leaf values).

### Keywords and Hints
- "Leaf value sequence" - This indicates we need to focus specifically on the leaf nodes, not all nodes
- "From left to right order" - This suggests a depth-first traversal that prioritizes going left before right
- "Leaf-similar" - We need to compare sequences, so we'll need to collect leaf values in order
- Binary tree traversal - This is a classic tree problem where DFS would be appropriate

### Key insights
1. We need to extract only the leaf nodes from each tree
2. Order matters - we need to collect the leaves in a left-to-right order
3. Both trees can have different structures, but as long as their leaf sequences are the same, they are "leaf-similar"
4. A simple depth-first search (DFS) can be used to collect leaves in the correct order

### Solution Intuition
To determine if two trees are leaf-similar, we need to collect all leaf nodes from each tree in the correct order and then compare these sequences. A leaf node is a node with no children (no left and no right child). We can use depth-first search (DFS) to traverse each tree, and whenever we encounter a leaf node, we add its value to our collection. After collecting leaf values from both trees, we simply compare the two sequences.

### Solution explanation
The solution follows these steps:
1. Define a helper function `get_leaf_sequence` that collects leaf values from a tree:
   - Use a nested DFS function to traverse the tree
   - At each node, check if it's a leaf (no children)
   - If it's a leaf, add its value to a list
   - If not, recursively traverse its left and right children
2. Call `get_leaf_sequence` on both trees to get their leaf sequences
3. Compare the two sequences and return true if they are identical, false otherwise

The DFS implementation ensures that leaves are visited from left to right because we always visit the left subtree before the right subtree.

### Analogy
Think of this problem like comparing two different hiking trails through a forest. Each tree is a different trail, and the trees and vegetation along the way represent the nodes in our binary tree. The leaf nodes are special viewpoints or landmarks on the trail.

Two hikers start at different trailheads (roots of the trees) and follow their respective trails. They both take pictures only at the special viewpoints (leaf nodes), and they visit these viewpoints in the order they encounter them (left to right).

At the end of the day, they compare their photo albums. If the sequence of viewpoints they photographed is exactly the same, then we say the trails are "viewpoint-similar" or in our problem's terms, "leaf-similar." The actual paths they took to reach these viewpoints might be different, but what matters is that they saw the same sequence of viewpoints.

This analogy captures the essence of the algorithm - we're not concerned with the entire structure of the trees, just the sequence of special nodes (leaves) when traversed in a specific order.
## [Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/?envType=study-plan-v2&envId=leetcode-75)

## Count Good Nodes in Binary Tree Solution

*Claude 3.7 Sonnet:*

### Code solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        def dfs(node, max_so_far):
            if not node:
                return 0

            # Check if current node is good
            count = 1 if node.val >= max_so_far else 0

            # Update the maximum value seen so far
            max_so_far = max(max_so_far, node.val)

            # Recursively count good nodes in left and right subtrees
            count += dfs(node.left, max_so_far)
            count += dfs(node.right, max_so_far)

            return count

        return dfs(root, float('-inf'))
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the binary tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This represents the maximum depth of the recursion stack. In the worst case (skewed tree), h can be O(n), but in a balanced tree, h would be O(log n).

### Keywords and Hints
- **"Path from root to X"**: This indicates we need to track information along a path through the tree, suggesting a depth-first search approach.
- **"No nodes with a value greater than X"**: This implies we need to keep track of the maximum value seen so far during our traversal.
- **Binary tree traversal**: The problem involves traversing all nodes in a binary tree, which can be done using DFS or BFS, but since we need to track information along specific paths, DFS is more natural.

### Key insights
1. A node is "good" if it's greater than or equal to all nodes in the path from the root to itself.
2. This is equivalent to saying a node is good if it's greater than or equal to the maximum value seen in the path from root to the node.
3. We can use a recursive DFS traversal that keeps track of the maximum value seen so far.
4. When we encounter a new node, we compare its value with the maximum value in the path so far to determine if it's a good node.

### Solution Intuition
Imagine walking down a path from the root of the tree to a leaf. Along the way, you keep track of the highest mountain you've seen. When you reach a new point, if it's at least as high as the highest mountain you've seen, it's considered a "good" point. The solution applies this same concept - tracking the maximum value seen so far and counting nodes that are at least as large as that maximum.

### Solution explanation
The solution uses depth-first search (DFS) to traverse the tree. For each node, we:

1. Pass down the maximum value seen so far in the path from the root to the current node.
2. If the current node's value is greater than or equal to the maximum value, it's a "good" node, so we add 1 to our count.
3. Update the maximum value to include the current node.
4. Recursively apply the same logic to the left and right children.
5. Return the total count of good nodes.

For the initial call, we set the maximum value to negative infinity, ensuring the root is always counted as a good node.

### Analogy
Think of this problem as hiking through a mountain range following a specific trail. You're looking for "impressive viewpoints" - places where you can see at least as far as any previous point on your journey so far.

As you hike, you keep track of the highest elevation you've reached. When you arrive at a new spot, you check if it's at least as high as the highest point you've visited. If it is, it's an "impressive viewpoint" (a good node).

This analogy fits because:
1. The path from the root to a node represents your hiking trail.
2. The node values represent the elevation at each point.
3. A "good node" is like an impressive viewpoint - it's at least as high as all previous points on the trail.
4. Just as we track the maximum value in our algorithm, a hiker would track the highest elevation they've reached.

## [Path Sum III](https://leetcode.com/problems/path-sum-iii/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        # Dictionary to store prefix sums and their frequency
        prefix_sum = {0: 1}

        # Result counter
        count = 0

        def dfs(node, curr_sum):
            nonlocal count

            if not node:
                return

            # Update current sum
            curr_sum += node.val

            # Check if curr_sum - targetSum exists in prefix_sum
            # If it does, it means there's a path ending at current node with sum = targetSum
            count += prefix_sum.get(curr_sum - targetSum, 0)

            # Update prefix_sum dictionary
            prefix_sum[curr_sum] = prefix_sum.get(curr_sum, 0) + 1

            # Traverse left and right children
            dfs(node.left, curr_sum)
            dfs(node.right, curr_sum)

            # Backtrack: remove current sum from the prefix_sum
            # This is necessary because we're moving back up the tree
            prefix_sum[curr_sum] -= 1

        dfs(root, 0)
        return count
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(n) in the worst case, as we might need to store up to n prefix sums in the hash map, and the recursive call stack can go as deep as the height of the tree, which is O(n) in the worst case (skewed tree).

### Keywords and Hints
- "Path does not need to start or end at the root or a leaf" - This indicates we need to consider all possible sub-paths in the tree, suggesting a prefix sum approach.
- "Must go downwards" - This restricts our paths to go only from parent to child, which means we can use a DFS traversal.
- "Return the number of paths" - We're counting, not finding specific paths, which suggests using a counter.
- "Sum of values along the path" - This is a classic prefix sum problem, where we track running sums.

### Key insights
1. A prefix sum approach can efficiently find subarrays that sum to a target value in linear time.
2. We can adapt this approach to trees by storing running sums along each path from root to the current node.
3. At each node, we check if the current prefix sum minus the target sum exists in our history, indicating a valid path.
4. Backtracking is necessary to remove the current path's contribution to the prefix sum when moving to a different branch.

### Solution Intuition
The intuition is to adapt the prefix sum technique from array problems to a tree structure. For each node, we want to know if there's a segment of the path ending at that node with a sum of `targetSum`. This can be determined by checking if there's a previous point in our traversal where the prefix sum was `current_sum - targetSum`.

### Solution explanation
1. We perform a DFS traversal of the tree, keeping track of the running sum from the root to the current node.
2. At each node, we check if `current_sum - targetSum` exists in our prefix sum dictionary. If it does, it means there's a subpath ending at the current node with a sum of `targetSum`.
3. We add the number of such previous sums to our result counter.
4. We then update our prefix sum dictionary with the current sum.
5. After exploring the current node's children, we need to backtrack by decrementing the count of the current sum in our prefix sum dictionary.
6. This process ensures that we count all valid paths in the tree that sum to `targetSum`.

### Analogy
Think of this problem as finding specific hiking trails on a mountain. The mountain is our tree, each node is a resting point, and the values are changes in elevation.

- You start at the top of the mountain (root) and want to find all hiking segments with a certain net elevation change (`targetSum`).
- As you hike down (can only go downward), you keep track of your total elevation change so far (prefix sum).
- At each resting point, you check your hiking log to see if there was a previous point where the elevation difference between then and now matches your target change.
- If you find such points, you mark those trail segments on your map (count them).
- After checking a resting point, you update your hiking log with your current elevation change.
- When you backtrack to try a different path, you remove the current trail from your consideration (decrement the prefix sum count).

This analogy fits because, like hiking down a mountain where you can only go from higher to lower points, our tree traversal only goes from parent to child. The recording of elevation changes is similar to our prefix sum tracking, and checking for specific elevation differences mirrors our look-up of `current_sum - targetSum` in the prefix sum dictionary.

## [Longest ZigZag Path in a Binary Tree](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/?envType=study-plan-v2&envId=leetcode-75)

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
    def longestZigZag(self, root: Optional[TreeNode]) -> int:
        self.max_length = 0

        def dfs(node, is_left, length):
            if not node:
                return

            # Update maximum zigzag length seen so far
            self.max_length = max(self.max_length, length)

            # If we came from left, we can only go right to continue zigzag
            if is_left:
                # Continue zigzag by going right
                dfs(node.right, False, length + 1)
                # Start new zigzag path by going left
                dfs(node.left, True, 1)
            else:
                # Continue zigzag by going left
                dfs(node.left, True, length + 1)
                # Start new zigzag path by going right
                dfs(node.right, False, 1)

        # Start zigzag paths from the root in both directions
        dfs(root.left, True, 1)  # Starts with left direction
        dfs(root.right, False, 1)  # Starts with right direction

        return self.max_length
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is the maximum depth of the recursion stack. In the worst case (skewed tree), this could be O(n).

### Keywords and Hints
- **"ZigZag path"**: This indicates we need to track alternating directions in a tree traversal.
- **"Direction (right or left)"**: We need to remember the current direction as we traverse.
- **"Change the direction"**: This means we need to alternate between going left and right.
- **"Number of nodes visited - 1"**: This defines how we calculate our zigzag length.
- The problem is asking for the longest such path, suggesting a DFS approach with some state tracking.

### Key Insights
1. A zigzag path always alternates between left and right moves.
2. At each node, we have two options:
   - Continue the current zigzag path by moving in the opposite direction from what we just did
   - Start a new zigzag path
3. We need to track the current direction and path length during traversal.
4. DFS is suitable because we need to explore all possible paths from each node.
5. The zigzag length is defined as the number of edges (or nodes - 1) in the path.

### Solution Intuition
The intuition is to traverse the tree using DFS while keeping track of:
1. The direction we just came from (left or right)
2. The current length of the zigzag path

At each node, we can either continue the zigzag pattern by moving in the opposite direction, or we can start a new zigzag path. We continuously update our maximum zigzag length as we traverse.

The key is understanding that a zigzag path has a "memory" of just one step - it only cares about making the next step in the opposite direction of the previous one.

### Solution Explanation
The solution uses a DFS traversal with three parameters:
1. `node`: The current node we're visiting
2. `is_left`: Boolean indicating if we arrived at this node from a left move
3. `length`: The current zigzag path length

For each node:
1. Update the maximum zigzag length seen so far.
2. If we arrived from the left:
   - Continue the zigzag by going right with an incremented length
   - Start a new zigzag by going left with a length of 1
3. If we arrived from the right:
   - Continue the zigzag by going left with an incremented length
   - Start a new zigzag by going right with a length of 1

We initiate the traversal from both the left and right children of the root to consider all possible starting points.

### Analogy
Imagine you're a mountain climber on a hiking trail with special rules:
- You're standing at the top of a mountain (the root).
- From any point, you can follow either a left or right trail to continue your journey.
- However, the trail is designed as a "switchback" path - if you take a left trail, your next move must be a right trail to continue on the same path (and vice versa).
- You can always decide to start a new switchback path by going in the same direction twice, but this resets your progress.
- Your goal is to find the longest possible switchback path in the mountain.

The zig-zag algorithm works like exploring all possible hiking paths while following the switchback rules, keeping track of the longest compliant path you've found. Just as a real switchback trail must alternate between left and right turns to navigate a steep slope efficiently, our zigzag path must alternate between left and right child nodes to be valid.

## [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # Base case: if we reach null or find one of the nodes
        if root is None or root == p or root == q:
            return root

        # Recursively search in left and right subtrees
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        # If both left and right have results, root is the LCA
        if left and right:
            return root

        # If only one subtree returns a non-null value, propagate it up
        return left if left else right
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree. In the worst case, we might need to visit all nodes once.
- **Space Complexity**: O(h), where h is the height of the tree. This is due to the recursion stack. In the worst case (skewed tree), this could be O(n).

### Keywords and Hints
- **"Lowest common ancestor (LCA)"** - This immediately suggests a tree traversal problem where we need to find a specific node.
- **"Descendant of itself"** - This special condition hints that we need a solution that can handle a node being an ancestor of itself.
- **"Binary tree"** (not binary search tree) - We can't use ordering properties, so we need to traverse the entire tree.
- **"Two given nodes p and q"** - We need to find a relationship between two specific nodes, suggesting a post-order traversal approach where we gather information from both subtrees.

### Key Insights
1. The LCA must be either one of the nodes (if one is an ancestor of the other) or a node that has p and q in different subtrees.
2. A post-order traversal (left, right, root) is ideal because we need information from both subtrees to make a decision at each node.
3. If both p and q are in the tree, the algorithm will always find an LCA.
4. If a node is found in a subtree, that information needs to be propagated upward in the recursion.

### Solution Intuition
We approach this problem using a recursive depth-first search. At each node, we ask:
1. Is this node p or q? If yes, return this node.
2. Is p or q in the left subtree? If yes, get that result.
3. Is p or q in the right subtree? If yes, get that result.
4. If both results from left and right are non-null, we've found our LCA (the current node).
5. Otherwise, propagate up the non-null result (if any).

### Solution Explanation
The algorithm works by recursively traversing the tree in a post-order manner:
1. Base case: If the current node is null or equals p or q, return the current node.
2. Recursively search for p and q in the left and right subtrees.
3. If both left and right recursive calls return non-null values, it means p and q are in different subtrees, so the current node is the LCA.
4. If only one recursive call returns a non-null value, it means either:
   - Both p and q are in that subtree, and the non-null return value is their LCA.
   - Only one of p or q is in the tree, and the non-null return value is that node.
5. Return the non-null result from either left or right, or null if both are null.

### Analogy
Think of a family tree search for the most recent common ancestor of two family members:

Imagine you're at a family reunion and want to find the closest common relative between two people (p and q). You start at the family's oldest ancestor (the root) and ask each person about their family branches:

1. If you're talking to either p or q directly, you've found one of the targets.
2. For each person, you ask their children (left and right subtrees) if p or q are in their line.
3. If you find that p is in one person's line and q is in another person's line, then the current person you're talking to is the closest common relative.
4. If both p and q are in the same person's family line, you continue your search down that line.

This is just like our algorithm - we're working our way through the tree, identifying where our targets are, and finding the point where their paths diverge (or where one is an ancestor of the other).
