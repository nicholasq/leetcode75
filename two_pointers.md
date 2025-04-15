# Two pointers

## [Move Zeroes](https://leetcode.com/problems/move-zeroes/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def moveZeroes(nums):
    """
    :type nums: List[int]
    :rtype: None Do not return anything, modify nums in-place instead.
    """
    # Position to place the next non-zero element
    pos = 0

    # Move all non-zero elements to the front
    for i in range(len(nums)):
        if nums[i] != 0:
            nums[pos] = nums[i]
            pos += 1

    # Fill the remaining positions with zeros
    for i in range(pos, len(nums)):
        nums[i] = 0
```

### Computational Complexity
- Time Complexity: O(n) where n is the length of the array. We traverse the array twice.
- Space Complexity: O(1) as we modify the array in-place without using extra space proportional to the input size.

### Keywords and Hints
- "in-place" - This indicates we need to modify the original array without creating a new one, suggesting a two-pointer approach.
- "maintaining the relative order" - We need to preserve the sequence of non-zero elements, which rules out simply swapping zeros with non-zeros from the end.
- "minimize the total number of operations" - This hints at avoiding unnecessary operations, like swapping elements when not needed.

### Key insights
- We can use a two-pass approach:
  1. First, move all non-zero elements to the front of the array, maintaining their relative order.
  2. Then, fill the remaining positions with zeros.
- Alternatively, we could use a single pass with a snowball approach where zeros accumulate while non-zeros move ahead.

### Solution Intuition
The intuition is to keep track of where the next non-zero element should be placed. We scan through the array once, and whenever we find a non-zero element, we place it at the tracked position and increment that position. After this scan, all non-zero elements are placed in the correct order at the beginning of the array. We then fill the remaining positions with zeros.

### Solution explanation
1. Initialize a variable `pos` to 0, which represents the position where the next non-zero element should be placed.
2. Iterate through the array. For each non-zero element found:
   - Place it at position `pos` in the array.
   - Increment `pos` to prepare for the next non-zero element.
3. After placing all non-zero elements, fill the remaining positions (from `pos` to the end) with zeros.

This approach ensures that all non-zero elements maintain their relative order while all zeros are moved to the end.

### Analogy
Think of this algorithm as organizing a bookshelf where zeros are empty spaces and non-zero elements are books. You want all the books to be together at the beginning of the shelf, maintaining their original order, with all empty spaces at the end.

1. Start by having a marker at the beginning of the shelf (your `pos` variable).
2. Go through each position on the shelf from left to right.
3. If you find a book (non-zero), place it at your marker position and move your marker to the right.
4. Once you've checked every position, you'll have all books neatly arranged at the beginning of the shelf.
5. Fill the remaining spaces (from your marker to the end) with empty slots (zeros).

This analogy captures the essence of the algorithm: we're reorganizing elements while maintaining their order, with a specific placement rule for non-zero elements and zeros.

## [Is Subsequence](https://leetcode.com/problems/is-subsequence/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def isSubsequence(s: str, t: str) -> bool:
    if not s:
        return True

    s_pointer = 0
    for char in t:
        if char == s[s_pointer]:
            s_pointer += 1
            if s_pointer == len(s):
                return True

    return False
```

### Computational Complexity
- Time Complexity: O(n) where n is the length of string t. We need to iterate through each character in t exactly once.
- Space Complexity: O(1) as we only use a constant amount of extra space regardless of input size (just a single pointer variable).

### Keywords and Hints
- "Subsequence" is the key term here - it means characters need to appear in the same order but not necessarily contiguously. This hints at using a simple iterative approach rather than a more complex algorithm.
- "Deleting some (can be none) of the characters without disturbing the relative positions" - this confirms that we need to maintain the order of characters, suggesting a sequential processing approach.
- The constraints indicate small to moderate input sizes, which suggests that a simple linear algorithm will be efficient enough.

### Key insights
- We need to check if characters in string s appear in string t in the same relative order.
- We don't need to find all possible subsequences - we just need to verify if s is one of them.
- Once we match a character from s in t, we can never use it again, and we should move on to the next character in s.
- If we can match all characters in s, then s is a subsequence of t.

### Solution Intuition
The simplest approach is to maintain a pointer for string s and iterate through string t. Whenever we find a matching character, we advance the pointer for s. If we can advance the pointer beyond the length of s, it means we've found all characters of s in the correct order in t.

### Solution explanation
1. If s is empty, it's always a subsequence (return True).
2. Initialize a pointer for string s at position 0.
3. Iterate through each character in string t:
   - If the current character in t matches the character in s at the pointer position, advance the pointer.
   - If the pointer reaches the end of s, we've found the entire subsequence and can return True.
4. If we finish iterating through t without finding the entire subsequence, return False.

### Analogy
Think of this as a scavenger hunt where you have a list of items (string s) that you need to find in order. You're walking along a path (string t) where various items are placed. As you walk, you check off each item on your list when you find it, but you can only move forward on the path - never backward. If you can check off all items on your list by the end of the path, you've completed the scavenger hunt successfully (s is a subsequence of t). If you reach the end of the path without finding all items on your list, you've failed (s is not a subsequence of t).

This analogy fits because, like the algorithm, you only care about finding items in the correct order, not about the distance between them or items that aren't on your list.

## [Container With Most Water](https://leetcode.com/problems/container-with-most-water/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def maxArea(height):
    left, right = 0, len(height) - 1
    max_area = 0

    while left < right:
        # Calculate the width between the two lines
        width = right - left

        # Calculate the height (which is the minimum of the two lines)
        h = min(height[left], height[right])

        # Update the maximum area if current area is larger
        max_area = max(max_area, width * h)

        # Move the pointer that points to the shorter line
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1

    return max_area
```

### Computational Complexity
- **Time Complexity**: O(n) where n is the length of the height array. We traverse the array once, with each element being visited at most once.
- **Space Complexity**: O(1) as we only use a constant amount of extra space regardless of the input size.

### Keywords and Hints
- "Two endpoints" - suggests we need to consider pairs of elements
- "Container contains the most water" - indicates a maximization problem
- "Amount of water" - determined by the width and the minimum height
- "May not slant the container" - the water level is determined by the shorter of the two sides

These keywords hint at using a two-pointer approach to find the optimal pair of lines.

### Key insights
1. The amount of water that can be stored between two lines is determined by:
   - The width (distance between the lines)
   - The height (which is the shorter of the two lines)
2. A key insight is that the optimal solution requires a trade-off between width and height.
3. Using a greedy approach with two pointers starting from the widest possible container and moving inward works because:
   - When we move a pointer, we always decrease the width
   - To compensate for this, we need to find a taller line

### Solution Intuition
The intuition is to start with the widest possible container (pointers at both ends) and then try to improve the height by moving pointers inward. We always move the pointer pointing to the shorter line because:
1. The amount of water is limited by the shorter line
2. Moving the taller pointer would definitely reduce the area (reduced width with same or lesser height)
3. Moving the shorter pointer gives us a chance to find a taller line that might increase the area despite the reduced width

### Solution explanation
1. Initialize two pointers: left at the beginning and right at the end of the array.
2. Calculate the initial area using the width (right - left) and height (minimum of the two heights).
3. In each iteration, move the pointer that points to the shorter line inward.
4. Keep track of the maximum area encountered.
5. Continue until the pointers meet.
6. Return the maximum area found.

The solution works because we systematically examine potential containers by starting with the widest one and then looking for containers with potentially greater heights to compensate for their narrower widths.

### Analogy
Think of this as a problem of finding the best rectangular swimming pool in a hilly landscape:

1. Imagine you're in a hilly terrain where the height of each point corresponds to the height in the array.
2. You want to build a rectangular swimming pool by choosing two hills and filling the area between them with water up to the height of the shorter hill.
3. You start by considering the two farthest hills (maximum width).
4. Then, you make a decision: "Should I move closer from the left side or the right side?"
5. Since the water level is determined by the shorter hill, there's no point in moving away from the taller hill (you'd lose width without gaining height).
6. Instead, you move away from the shorter hill, hoping to find a taller one that would allow more water despite the reduced width.
7. You continue this process, always updating your record of the maximum capacity pool you've found.

This analogy reflects the two-pointer approach where we start with maximum width and strategically reduce it to potentially find greater heights, always keeping track of the best solution.

## [Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def maxOperations(nums, k):
    counter = {}
    operations = 0

    for num in nums:
        complement = k - num

        if counter.get(complement, 0) > 0:
            counter[complement] -= 1
            operations += 1
        else:
            counter[num] = counter.get(num, 0) + 1

    return operations
```

### Computational Complexity
- Time Complexity: O(n) where n is the length of the array. We iterate through the array once.
- Space Complexity: O(n) in the worst case, where we might need to store all elements in the counter.

### Keywords and Hints
- "two numbers from the array whose sum equals k" suggests a **pair sum problem**
- "maximum number of operations" indicates we need to find as many valid pairs as possible
- The need to "remove them from the array" implies we need to track which elements have been used

These keywords point to the two-sum pattern which is efficiently solved using a hash map to track elements we've seen.

### Key insights
1. For each number `num`, we need to find another number `k - num` to form a valid pair.
2. We can use a hash map to keep track of numbers we've seen and their frequencies.
3. When processing a number, we check if its complement (`k - num`) exists in our map.
4. If it does, we form a pair and increase our operation count.

### Solution Intuition
The intuition is to use the "complement finding" approach. For each number we encounter, we check if we've already seen its complement (the number that would sum to k when added to the current number). If we have, we can form a pair. Otherwise, we store this number for potential future pairing.

This approach is elegant because we only need to pass through the array once while maintaining a record of seen numbers.

### Solution explanation
1. Create a hash map (counter) to track elements we've seen.
2. Initialize a counter for successful operations.
3. Iterate through each number in the array:
   - Calculate the complement (k - current number).
   - If the complement exists in our counter (with count > 0):
     - Decrement its count (marking it as "used").
     - Increment our operations counter.
   - Otherwise, add the current number to our counter.
4. Return the total number of operations.

This solution efficiently finds all valid pairs by looking for each number's complement as we traverse the array.

### Analogy
Imagine you're at a dance where everyone needs to find a partner whose height, when combined with theirs, equals a specific value (k inches).

As each person arrives, you check if there's someone already waiting whose height complements theirs to reach k inches. If there is, you pair them up and they leave the dance floor (a successful operation). If not, this new person joins the waiting area, hoping a future arrival will complement their height.

The hash map is like the waiting area, tracking everyone who hasn't found a partner yet. Each time we match a pair and they leave, it's like one successful operation in our algorithm. The goal is to match as many pairs as possible before everyone has arrived.

This analogy captures the essence of our algorithm: for each element, we either find its complement in our "waiting area" or add it to the waiting area for potential future matches.
