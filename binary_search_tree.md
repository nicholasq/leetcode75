# Binary Search Tree

## [Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/?envType=study-plan-v2&envId=leetcode-75)

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
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # Base case: if root is None or we found the value
        if not root or root.val == val:
            return root

        # If target value is less than current node, search in left subtree
        if val < root.val:
            return self.searchBST(root.left, val)

        # If target value is greater than current node, search in right subtree
        return self.searchBST(root.right, val)
```

### Computational Complexity
- **Time Complexity**: O(h) where h is the height of the tree. In the worst case (skewed tree), this would be O(n) where n is the number of nodes. In a balanced BST, the height is log(n), so the complexity would be O(log n).
- **Space Complexity**: O(h) for the recursive call stack where h is the height of the tree. Same as time complexity, this can be O(n) in the worst case or O(log n) in a balanced tree.

### Keywords and Hints
- **Binary Search Tree (BST)**: The problem explicitly states that the input is a BST. This is critical because a BST has the property that all nodes in the left subtree have values less than the node's value, and all nodes in the right subtree have values greater than the node's value.
- **Subtree rooted with that node**: We need to return the entire subtree starting from the found node, not just the node value.
- **Return null if not found**: Clear instruction on what to return if the value isn't found.

### Key insights
- The BST property allows us to eliminate half of the remaining tree at each step, making the search efficient.
- We can use the BST property to decide whether to search in the left or right subtree based on the comparison of the current node's value with the target value.
- The recursive structure of a BST naturally lends itself to a recursive solution.

### Solution Intuition
The intuition for searching in a BST is simple: at each node, you compare the target value with the current node's value. If they match, you've found the node. If the target is smaller, you only need to search in the left subtree. If the target is larger, you only need to search in the right subtree. This way, you eliminate half of the remaining search space at each step.

### Solution explanation
The solution leverages the properties of a BST:
1. If the current node is null or its value equals the target value, return the current node.
2. If the target value is less than the current node's value, the target must be in the left subtree (if it exists), so recursively search the left subtree.
3. If the target value is greater than the current node's value, the target must be in the right subtree (if it exists), so recursively search the right subtree.

This approach guarantees that we'll find the node if it exists in the tree, and we'll do so in an efficient manner by eliminating half of the potential nodes at each step.

### Analogy
Think of searching in a BST like playing a high-low game where someone thinks of a number and you have to guess it. After each guess, they tell you if your guess is too high, too low, or correct.

In a BST, each node is like a guess. If the target value is less than the node's value, it's like being told "too high" - you need to go lower (to the left subtree). If the target value is greater than the node's value, it's like being told "too low" - you need to go higher (to the right subtree). And if the values match, you've found the correct number!

The efficiency comes from the fact that at each step, you eliminate half of the range of possible values, just like in the high-low game where each guess cuts the range of possible answers in half.

## [Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/?envType=study-plan-v2&envId=leetcode-75)

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
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None

        # Search for the node to delete
        if key < root.val:
            root.left = self.deleteNode(root.left, key)
        elif key > root.val:
            root.right = self.deleteNode(root.right, key)
        else:
            # Node with only one child or no child
            if not root.left:
                return root.right
            elif not root.right:
                return root.left

            # Node with two children
            # Find the inorder successor (smallest node in right subtree)
            successor = self.findMin(root.right)
            root.val = successor.val

            # Delete the inorder successor
            root.right = self.deleteNode(root.right, successor.val)

        return root

    def findMin(self, node):
        # Find the leftmost node (minimum value)
        current = node
        while current.left:
            current = current.left
        return current
```

### Computational Complexity
- Time Complexity: O(h), where h is the height of the tree. In the worst case, we may need to traverse from the root to the deepest leaf node (h = log N for a balanced BST, h = N for a skewed tree).
- Space Complexity: O(h) for the recursion stack.

### Keywords and Hints
- **BST (Binary Search Tree)**: The structure guarantees that for any node, all nodes in its left subtree have smaller values and all nodes in its right subtree have larger values.
- **Deletion cases**: The problem mentions "deletion can be divided into two stages," which hints at the need to handle different cases (no children, one child, two children).
- **Follow-up requirement**: "Time complexity O(height of tree)" suggests we should leverage the BST property to avoid scanning the entire tree.
- **Tree traversal**: We need to efficiently find the node to delete by exploiting the BST property.

### Key insights
1. The deletion in a BST involves three possible scenarios:
   - Node to delete has no children: Simply remove the node.
   - Node to delete has one child: Replace the node with its child.
   - Node to delete has two children: Replace the node with its inorder successor (smallest value in the right subtree) and then delete the successor.
2. The BST property allows us to efficiently locate the node to delete by comparing values at each step.
3. When deleting a node with two children, we can maintain the BST property by replacing it with its inorder successor or predecessor.

### Solution Intuition
The solution follows a recursive approach to navigate through the tree:
1. If the key is less than the current node's value, search in the left subtree.
2. If the key is greater than the current node's value, search in the right subtree.
3. If the key matches the current node's value, apply one of the deletion strategies based on the node's children.
4. For a node with two children, replace it with its inorder successor to maintain the BST property.

### Solution explanation
Our solution implements a recursive approach to delete a node in a BST:

1. First, we traverse the tree to find the node to delete using the BST property (smaller values to the left, larger to the right).
2. Once we find the node, we handle three cases:
   - If the node has no left child, we replace it with its right child.
   - If the node has no right child, we replace it with its left child.
   - If the node has both children, we find the minimum value in the right subtree (inorder successor), replace the current node's value with this successor's value, and then recursively delete the successor from the right subtree.
3. The findMin helper function finds the smallest node in a subtree by repeatedly going to the left.
4. This approach maintains the BST property throughout the deletion process.

### Analogy
Think of a BST as a file cabinet organized alphabetically. If you need to remove a file (node):

1. **No children case**: If the file has no references to other files, simply remove it from the cabinet.
2. **One child case**: If the file points to another file, replace it with the file it's pointing to, preserving the chain.
3. **Two children case**: If the file connects to files both before and after it alphabetically, find the file that comes just after it in alphabetical order (successor), copy its content to replace the file you're removing, and then remove the successor from its original position. This maintains the alphabetical ordering of the cabinet.

The alphabetical organization of the file cabinet represents the BST property, where items to the left come before the current item alphabetically, and items to the right come after. When removing files, we ensure this organization remains intact.
