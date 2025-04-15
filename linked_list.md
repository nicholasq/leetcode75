# Linked List

## [Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def deleteMiddle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Edge case: If there's only one node, return None
        if not head.next:
            return None

        # Initialize slow and fast pointers
        slow = head
        fast = head
        prev = None

        # Move fast pointer twice as fast as slow pointer
        while fast and fast.next:
            fast = fast.next.next
            prev = slow
            slow = slow.next

        # Now slow points to the middle node, and prev points to the node before middle
        # Delete the middle node
        prev.next = slow.next

        return head
```

### Computational Complexity
- **Time Complexity**: O(n) where n is the number of nodes in the linked list. We traverse the list once using the fast and slow pointers approach.
- **Space Complexity**: O(1) since we only use a constant amount of extra space regardless of input size.

### Keywords and Hints
- "Delete the middle node" - This suggests we need to find the middle node in a singly linked list.
- "⌊n / 2⌋th node from the start" - This tells us the exact definition of the middle node, which is important for edge cases.
- "using 0-based indexing" - This clarifies how we count positions in the list.
- "For n = 1, 2, 3, 4, and 5, the middle nodes are 0, 1, 1, 2, and 2, respectively" - This provides examples to clarify the middle node definition.

### Key insights
1. To delete a node in a singly linked list, we need a reference to the node before it (except if it's the head).
2. The fast and slow pointer technique is ideal for finding the middle of a linked list in one pass.
3. The slow pointer moves one node at a time, while the fast pointer moves two nodes at a time.
4. When the fast pointer reaches the end, the slow pointer is at the middle.
5. We need to keep track of the node preceding the slow pointer to delete the middle node.

### Solution Intuition
Imagine two runners on a track, one running twice as fast as the other. When the faster runner finishes the race (reaches the end of the list), the slower runner will be exactly at the halfway point (the middle of the list). By keeping track of where the slow runner is and the node right before it, we can remove the middle node in a single pass.

### Solution explanation
1. Handle the edge case: If there's only one node in the list, return `None`.
2. Initialize three pointers:
   - `slow` pointing to the head
   - `fast` pointing to the head
   - `prev` pointing to `None` initially
3. Traverse the list using the fast and slow pointer technique:
   - Move `fast` two nodes ahead at each step
   - Move `slow` one node ahead at each step
   - Update `prev` to keep track of the node before `slow`
4. When `fast` reaches the end or becomes `None`, `slow` is at the middle node.
5. Delete the middle node by setting `prev.next = slow.next`.
6. Return the head of the modified list.

### Analogy
Think of a train with numbered carriages. You want to remove the middle carriage, but you can only see one carriage at a time through a small window, and you can only move forward. You send two inspectors down the train:
1. Inspector A (slow pointer) walks at normal speed, checking one carriage per minute.
2. Inspector B (fast pointer) walks twice as fast, checking two carriages per minute.
3. You also have a helper (prev pointer) who follows Inspector A but stays one carriage behind.

When Inspector B reaches the end of the train, Inspector A will be at the middle carriage. At this point, the helper (who is one carriage behind) can disconnect Inspector A's carriage and reconnect the train, effectively removing the middle carriage. This works because in a linked list, you need to have a reference to the node before the one you want to delete.

## [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Handle edge cases
        if not head or not head.next:
            return head

        # Initialize pointers
        odd_head = odd = head
        even_head = even = head.next

        # Traverse the list and rearrange nodes
        while even and even.next:
            # Connect odd nodes
            odd.next = even.next
            odd = odd.next

            # Connect even nodes
            even.next = odd.next
            even = even.next

        # Connect the end of odd list to the start of even list
        odd.next = even_head

        return odd_head
```

### Computational Complexity
- Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list once.
- Space Complexity: O(1) as we only use a constant amount of extra space regardless of input size.

### Keywords and Hints
- "Group all the nodes with odd indices together followed by the nodes with even indices" - This indicates we need to separate nodes based on their position (not value), and maintain two separate chains.
- "Relative order inside both groups should remain" - We need to preserve the original order within each group.
- "O(1) extra space complexity" - We can't use additional data structures like arrays; we need to manipulate the pointers directly.
- "O(n) time complexity" - We should aim for a single pass solution.

### Key insights
1. We can maintain two separate chains: one for odd-indexed nodes and one for even-indexed nodes.
2. As we traverse the list, we can alternate between adding to the odd chain and the even chain.
3. After processing all nodes, we can connect the end of the odd chain to the beginning of the even chain.
4. This can be done in a single pass through the list.

### Solution Intuition
Imagine you're sorting cards into two piles - one for odd positions and one for even positions. You look at each card exactly once, and immediately decide which pile it goes in based on its position. After going through all the cards, you place the even pile after the odd pile. This is essentially what we're doing with the linked list nodes.

### Solution explanation
1. We maintain two separate chains: one for odd-indexed nodes and one for even-indexed nodes.
2. We use two pointers (`odd` and `even`) to track the current node in each chain.
3. We start with the first node (index 1, odd) and the second node (index 2, even).
4. As we traverse the list, we:
   - Connect the current odd node to the next odd node (which is after the next even node)
   - Connect the current even node to the next even node (which is after the next odd node)
   - Move our pointers forward
5. After processing all nodes, we connect the end of the odd chain to the beginning of the even chain.
6. Return the head of the odd chain (which is the original head).

### Analogy
Imagine a line of people standing alternating between males and females. You want to rearrange them so all males are at the front of the line, followed by all females, while maintaining their relative order.

Instead of physically moving people (which would be inefficient), you create two separate lines: one for males and one for females. As you walk down the original line, you tell each person which new line to join. Males join the male line, and females join the female line. After everyone has been assigned, you connect the end of the male line to the beginning of the female line to form a single line again.

This is exactly what our algorithm does: we maintain two separate chains (odd and even), populate them by traversing the original list, and then connect them. The key benefit is that we do this by only changing the pointers rather than creating new nodes or using extra space.

## [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

# Iterative Solution
def reverseList(head):
    prev = None
    current = head

    while current:
        next_temp = current.next  # Store next node
        current.next = prev       # Reverse the link
        prev = current            # Move prev one step forward
        current = next_temp       # Move current one step forward

    return prev  # New head of the reversed list

# Recursive Solution
def reverseListRecursive(head):
    # Base case: empty list or list with only one node
    if not head or not head.next:
        return head

    # Recursively reverse the rest of the list
    reversed_list = reverseListRecursive(head.next)

    # Reverse the link for current node
    head.next.next = head
    head.next = None

    return reversed_list
```

### Computational Complexity

**Iterative Solution:**
- Time Complexity: O(n) where n is the number of nodes in the linked list. We visit each node exactly once.
- Space Complexity: O(1) as we only use a constant amount of extra space.

**Recursive Solution:**
- Time Complexity: O(n) where n is the number of nodes in the linked list. We visit each node exactly once.
- Space Complexity: O(n) due to the recursive call stack.

### Keywords and Hints

Key terms in the problem statement that guide us toward the solution:
- "singly linked list" - Indicates we're working with a data structure where each node points to the next one
- "reverse the list" - We need to change the direction of all the links
- "return the reversed list" - We need to find the new head (which will be the old tail)
- "iteratively or recursively" - The problem explicitly mentions that both approaches are valid

The problem is classified as "Easy" which suggests that the core algorithm isn't complex, and there's likely a straightforward approach using linked list manipulation.

### Key insights

1. To reverse a linked list, we need to change the direction of each pointer.
2. We must be careful not to lose track of nodes during the reversal process.
3. The original head node will become the tail of the reversed list.
4. The original tail node will become the head of the reversed list.
5. A temporary variable is needed to store the next node before we change the pointer.

### Solution Intuition

**Iterative approach:** Think of it as walking through the list while flipping the direction of each arrow. You need three pointers:
- One to keep track of where you've been (prev)
- One for where you are now (current)
- One for where you're going next (next_temp)

**Recursive approach:** Think of it as saying, "First reverse the rest of the list after the current node, then attach the current node to the end."

### Solution explanation

**Iterative solution:**
1. Initialize two pointers: `prev` as NULL and `current` as the head of the list.
2. Iterate through the list with the `current` pointer:
   - Store the next node in a temporary variable (`next_temp`)
   - Change the next pointer of the current node to point to the previous node
   - Move the previous pointer to the current node
   - Move the current pointer to the next node (stored in the temporary variable)
3. After the loop, the `prev` pointer will be pointing to the new head of the reversed list.

**Recursive solution:**
1. Define the base case: if the list is empty or has only one node, return the head.
2. Recursively call the function on the sublist starting from the next node, which returns the reversed sublist.
3. Set the next pointer of the next node to point to the current node.
4. Set the next pointer of the current node to NULL to avoid cycles.
5. Return the head of the reversed sublist, which becomes the head of the entire reversed list.

### Analogy

**Iterative approach:** Imagine a line of people facing forward (representing the linked list). To reverse their direction, you would start from one end and have each person turn around to face the person behind them.

As you move through the line, at each step:
1. You remember who's next in line (next_temp)
2. You tell the current person to face backward (current.next = prev)
3. You move to the next person (current = next_temp)

Eventually, the person who was originally at the back of the line becomes the front.

**Recursive approach:** Imagine a chain of dominoes already set up. Instead of pushing the first domino to knock them all down in sequence, you could go to the last domino and work your way backward.

You tell each domino: "Wait until I've repositioned all the dominoes after you, then turn around to face the other way." Since you start at the end, the last domino repositioned will be the first one in the original chain, effectively reversing the entire chain.

## [Maximum Twin Sum of a Linked List](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def pairSum(self, head: Optional[ListNode]) -> int:
        # Find the middle of the linked list using slow and fast pointers
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        # Reverse the second half of the linked list
        prev = None
        curr = slow
        while curr:
            next_temp = curr.next
            curr.next = prev
            prev = curr
            curr = next_temp

        # prev now points to the head of the reversed second half

        # Calculate the maximum twin sum
        max_sum = 0
        first_half = head
        second_half = prev

        while second_half:
            max_sum = max(max_sum, first_half.val + second_half.val)
            first_half = first_half.next
            second_half = second_half.next

        return max_sum
```

### Computational Complexity
- Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list once to find the middle, once to reverse the second half, and once to calculate the twin sums.
- Space Complexity: O(1) as we only use a constant amount of extra space regardless of the input size.

### Keywords and Hints
- "Twin of the (n-1-i)th node": This indicates we need to match nodes from the beginning with nodes from the end.
- "Linked list with even length": We know we can split the list exactly in half.
- "Return the maximum twin sum": We need to compare each twin sum and find the maximum.
- The fact that it's a singly linked list means we can't directly access nodes from the end, suggesting we need to use a technique to access pairs efficiently.

### Key insights
1. To find twin nodes (i and n-1-i), we need a way to match nodes from opposite ends of the list.
2. Since this is a singly linked list, we can't directly traverse backwards.
3. We can split the list in half, reverse the second half, and then walk through both halves simultaneously to compute twin sums.
4. Using fast and slow pointers allows us to find the middle in a single pass.

### Solution Intuition
The problem asks us to find the maximum sum of pairs of nodes that are symmetrically positioned from the start and end of the list. In a singly linked list, we can't easily access nodes from the end. However, we can transform the problem by:
1. Finding the middle point
2. Reversing the second half of the list
3. Walking through both halves simultaneously to compute the twin sums

This approach allows us to align the twins and compute their sums in a single pass.

### Solution explanation
1. Use the fast and slow pointer technique to find the middle of the linked list. The slow pointer moves one step at a time, while the fast pointer moves two steps. When the fast pointer reaches the end, the slow pointer is at the middle.
2. Reverse the second half of the linked list (starting from where the slow pointer ended).
3. Now we have two pointers: one at the start of the original list, and one at the start of the reversed second half.
4. Move both pointers forward simultaneously, calculating the sum of their values and keeping track of the maximum sum seen.
5. Return the maximum twin sum.

### Analogy
Think of this problem like folding a strip of paper with numbers written on it. When you fold the paper in half, the numbers that align with each other are twins.

Initially, our linked list is like a long strip of paper with numbers. We need to:
1. Find the middle point (like finding where to fold)
2. Reverse the second half (like flipping the paper before folding)
3. Then compare the aligned pairs (like looking at the pairs of numbers that touch after folding)

Just as when you fold a paper, the first and last numbers touch, the second and second-to-last touch, and so on, our algorithm pairs up these "twin" nodes and finds the maximum sum among them.
