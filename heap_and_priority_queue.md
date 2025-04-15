# Heap / Priority Queue

## [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
import heapq

def findKthLargest(nums, k):
    # Use a min heap to keep track of the k largest elements
    min_heap = []

    for num in nums:
        # Push the current element onto the heap
        heapq.heappush(min_heap, num)

        # If the heap size exceeds k, remove the smallest element
        if len(min_heap) > k:
            heapq.heappop(min_heap)

    # The root of the heap is the kth largest element
    return min_heap[0]
```

### Computational Complexity
- **Time Complexity**: O(n log k), where n is the length of the input array and k is the given parameter
  - For each of the n elements, we perform a heap operation which costs log k
- **Space Complexity**: O(k) for storing k elements in the heap

### Keywords and Hints
- "kth largest element" suggests using a selection algorithm or a data structure that can efficiently maintain the largest elements
- "Can you solve it without sorting" is a key hint that a full O(n log n) sort is not the most efficient solution
- The problem is asking for the kth largest, not the kth distinct largest, which means duplicates count
- The constraint that k ≤ array length means we need to find a specific element, not just the maximum or minimum

### Key insights
- We only need to keep track of the k largest elements, not the entire sorted array
- A min-heap (priority queue) of size k is perfect for maintaining the k largest elements
- Once the heap has k elements, any new element larger than the smallest in the heap should replace it
- Alternatively, QuickSelect algorithm can solve this with an average O(n) time complexity, but a heap approach is more straightforward

### Solution Intuition
The intuition is to maintain a "top-k" container of the largest elements we've seen so far. As we process each number:
1. If our container has fewer than k elements, we add the number
2. If our container already has k elements, we compare the new number with the smallest in our container
3. If the new number is larger, it should be in our top-k, so we remove the smallest and add the new one
4. At the end, the smallest number in our container is the kth largest overall

### Solution explanation
The solution uses a min-heap data structure, which automatically keeps the smallest element at the top:

1. Initialize an empty min-heap
2. Iterate through each number in the input array:
   - Add the number to the heap
   - If the heap size exceeds k, remove the smallest element
3. After processing all numbers, the heap contains the k largest elements from the array
4. The root of the heap (the smallest among these k elements) is the kth largest element overall

This approach avoids sorting the entire array, which would be less efficient for large arrays when k is small.

### Analogy
Imagine you're a talent scout for a basketball team looking for the k tallest players from a large group. Instead of measuring and ranking everyone (full sorting), you use a more efficient approach:

1. Start by selecting the first k players you see, regardless of height
2. For each additional player you encounter, compare their height to the shortest player in your current selection
3. If the new player is taller, replace your shortest current pick with this new player
4. Continue this process until you've seen everyone
5. The shortest player in your final selection is the kth tallest overall

Your "selection group" works like our min-heap - it always keeps the shortest player (smallest element) easily accessible for comparison, and after seeing all players, contains exactly the k tallest ones.

## [Smallest Number in Infinite Set](https://leetcode.com/problems/smallest-number-in-infinite-set/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
import heapq

class SmallestInfiniteSet:
    def __init__(self):
        # Keep track of the current smallest number in the infinite set
        self.current_min = 1
        # Min heap to store numbers that were added back
        self.added_back = []
        # Set for O(1) lookup to check if a number is already added back
        self.added_back_set = set()

    def popSmallest(self):
        # If we have numbers that were added back
        if self.added_back:
            # Get the smallest from the heap
            smallest = heapq.heappop(self.added_back)
            # Remove from the set as well
            self.added_back_set.remove(smallest)
            return smallest
        else:
            # Otherwise, return and increment the current minimum
            smallest = self.current_min
            self.current_min += 1
            return smallest

    def addBack(self, num):
        # Only add back if the number is less than current_min and not already in the added_back set
        if num < self.current_min and num not in self.added_back_set:
            heapq.heappush(self.added_back, num)
            self.added_back_set.add(num)
```

### Computational Complexity
- Time Complexity:
  - `__init__()`: O(1)
  - `popSmallest()`: O(log n) where n is the number of items in the added_back heap
  - `addBack(num)`: O(log n) for the heap operations if the number is added
- Space Complexity: O(n) for storing the added back numbers in both the heap and set

### Keywords and Hints
- "Infinite set" suggests we need a way to represent a potentially unbounded collection
- "popSmallest" indicates we need efficient access to the minimum element
- "addBack" suggests we need a data structure that supports dynamic insertions
- Having to track what numbers have been removed and possibly added back hints at using a priority queue (heap) for efficient minimum element extraction

### Key insights
1. We don't need to explicitly store all positive integers, which would be impossible
2. We can track the smallest number in the infinite set using a counter (`current_min`)
3. We only need to explicitly store numbers that are "added back" after being removed
4. A min-heap is ideal for efficiently retrieving the smallest element when we have added-back numbers

### Solution Intuition
The problem asks us to maintain an infinite set starting from 1, with operations to remove the smallest element and add elements back. Instead of explicitly storing all numbers, we can:
1. Track the smallest available number in the infinite sequence (`current_min`)
2. Separately track the numbers that were removed and added back
3. When popping the smallest, check both our added-back collection and the `current_min` to determine the true smallest

### Solution explanation
The solution uses a combination of a counter, a min-heap, and a set:
1. `current_min` tracks the smallest number in the original sequence that hasn't been popped yet
2. `added_back` is a min-heap that stores numbers that have been popped and then added back
3. `added_back_set` is a set for O(1) lookups to check if a number is already in the added_back heap

When `popSmallest()` is called:
- If there are numbers in the added_back heap, return the smallest one (which is always at the top of the min-heap)
- Otherwise, return and increment `current_min`

When `addBack(num)` is called:
- Only add the number if it's less than `current_min` (meaning it was previously removed) and not already in the added_back set
- This maintains the property that all numbers < `current_min` that aren't in `added_back` have been removed from the set

### Analogy
Think of this as managing a number ticket system for a bakery:
- Initially, ticket numbers start at 1 and go up sequentially
- The `current_min` is like the "Now Serving" display, showing the next customer to be served if no previously-called customers return
- The `added_back` heap is like a special priority queue for customers who were called but stepped away temporarily and have now returned
- When calling the next customer (popSmallest), you first check if any previously-called customers have returned (check the heap), and if not, you increment the "Now Serving" display
- The `added_back_set` is like a quick lookup system to check if a returning customer is already in the priority queue without having to search through it

This system ensures that customers (numbers) are always served (returned) in the correct order, with the lowest number going first.

## [Maximum Subsequence Score](https://leetcode.com/problems/maximum-subsequence-score/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution

```python
import heapq

def maxScore(nums1, nums2, k):
    # Create pairs of (nums2[i], nums1[i]) and sort by nums2[i] in descending order
    pairs = sorted(zip(nums2, nums1), reverse=True)

    # Initialize a min heap to track k largest elements from nums1
    heap = []
    total_sum = 0
    max_score = 0

    for min_val, num1_val in pairs:
        # Add current nums1 value to our sum and heap
        total_sum += num1_val
        heapq.heappush(heap, num1_val)

        # If we have more than k elements, remove the smallest
        if len(heap) > k:
            total_sum -= heapq.heappop(heap)

        # Once we have exactly k elements, calculate the score
        if len(heap) == k:
            max_score = max(max_score, total_sum * min_val)

    return max_score
```

### Computational Complexity

- **Time Complexity**: O(n log n), where n is the length of the arrays. This is dominated by the sorting operation. The heap operations (push and pop) are O(log k), but they're performed n times, resulting in O(n log k), which is less than O(n log n).
- **Space Complexity**: O(n) for storing the pairs and O(k) for the heap, so overall O(n).

### Keywords and Hints

- "subsequence of indices" - suggests we need to select a specific subset of positions
- "maximum possible score" - indicates an optimization problem
- "sum of selected elements multiplied by minimum" - this is the key insight: the minimum value in nums2 acts as a bottleneck for our score
- The combination of summing one array and taking the minimum of another suggests a greedy approach with sorting

### Key insights

1. The score is the product of the sum of selected nums1 elements and the minimum of selected nums2 elements.
2. For any set of indices we choose, the minimum of nums2 will be our bottleneck.
3. We can sort the pairs (nums2[i], nums1[i]) by nums2 in descending order to process them from potentially highest minimum to lowest.
4. A min-heap helps us maintain the k largest nums1 values we've seen so far.

### Solution Intuition

The key insight is to prioritize elements with higher values in nums2 (since they might be the minimum), while maximizing the sum of the corresponding nums1 elements.

We sort the (nums2, nums1) pairs by nums2 in descending order. Then, as we iterate through them, we know the current nums2 value is the smallest we've seen so far and would be our minimum if we include it. We use a min-heap to keep track of the k largest nums1 values we've seen up to this point, ensuring we maximize the sum component of our score.

### Solution explanation

1. Sort the pairs (nums2[i], nums1[i]) by nums2[i] in descending order.
2. Initialize a min heap to keep the k largest elements from nums1.
3. Iterate through the sorted pairs:
   - Add the nums1 value to our running sum and to the heap.
   - If the heap size exceeds k, remove the smallest element.
   - When we have exactly k elements, calculate the score: current sum * current nums2 value.
   - Track the maximum score seen.
4. Return the maximum score.

This approach ensures that for each potential minimum value from nums2, we're using the k largest available values from nums1 to maximize the score.

### Analogy

Imagine you're forming a team of k workers for a project. Each worker has two attributes:
1. Productivity (nums1): How much work they can do individually.
2. Collaboration Score (nums2): How well they work in a team.

The overall team performance is calculated as (sum of individual productivities) * (lowest collaboration score in the team).

To maximize team performance, you first rank candidates by their collaboration score (highest to lowest). Then, for each potential "weakest collaborator", you select the k most productive workers who have at least that collaboration level.

It's like saying: "If we set a minimum collaboration standard, what's the best team we can form?" By trying different minimum standards (starting from the highest), we can find the optimal team composition.

This approach mirrors our algorithm: we prioritize high nums2 values (collaboration scores) while maximizing the sum of corresponding nums1 values (productivity).

## [Total Cost to Hire K Workers](https://leetcode.com/problems/total-cost-to-hire-k-workers/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
import heapq

def totalCost(costs, k, candidates):
    n = len(costs)

    # If we need to consider all workers or more than available, we can simplify
    if candidates * 2 >= n:
        # Sort and take k lowest costs
        costs.sort()
        return sum(costs[:k])

    # Initialize two heaps for the first and last 'candidates' workers
    head_workers = costs[:candidates]
    tail_workers = costs[max(candidates, n - candidates):]

    heapq.heapify(head_workers)  # Min heap for first group
    heapq.heapify(tail_workers)  # Min heap for last group

    # Initialize pointers for the next workers to consider
    next_head = candidates
    next_tail = n - candidates - 1

    total_cost = 0

    for _ in range(k):
        # If head heap has the minimum cost (or both are equal, we prefer head)
        if not tail_workers or (head_workers and head_workers[0] <= tail_workers[0]):
            total_cost += heapq.heappop(head_workers)
            # If there are more workers to consider for the head section
            if next_head <= next_tail:
                heapq.heappush(head_workers, costs[next_head])
                next_head += 1
        else:
            total_cost += heapq.heappop(tail_workers)
            # If there are more workers to consider for the tail section
            if next_head <= next_tail:
                heapq.heappush(tail_workers, costs[next_tail])
                next_tail -= 1

    return total_cost
```

### Computational Complexity
- **Time Complexity**: O(k log(candidates))
  - Building the initial heaps takes O(candidates)
  - Each of the k hiring sessions requires O(log(candidates)) for heap operations
- **Space Complexity**: O(candidates)
  - We maintain two heaps, each with at most 'candidates' elements

### Keywords and Hints
- "Choose the worker with the lowest cost from either the first candidates workers or the last candidates workers"
- "Break the tie by the smallest index"
- "In each hiring session, choose..."

These phrases suggest a repeated selection process where we always need the minimum cost worker from two specific groups. This points to using a min-heap (priority queue) to efficiently track the minimum cost worker from each group.

### Key insights
1. We need to make k selections, always choosing the minimum cost worker from the first or last 'candidates' workers
2. After selecting a worker, we need to bring in a new worker to maintain our candidate pools
3. Two separate min-heaps allow us to efficiently find the minimum cost in each group
4. We need to track the boundaries between selected and unselected workers using pointers

### Solution Intuition
The problem is asking us to repeatedly select the worker with the lowest cost from either the first or last 'candidates' workers. After each selection, the pool of available workers changes. This suggests using a data structure that can efficiently give us the minimum element - a min-heap.

By maintaining two separate min-heaps (one for the first 'candidates' workers and one for the last 'candidates' workers), we can quickly identify the overall minimum cost worker at each step.

### Solution explanation
1. Create two min-heaps: one for the first 'candidates' workers and one for the last 'candidates' workers
2. Initialize pointers to track the next workers to consider from the middle section
3. For each of the k hiring sessions:
   - Compare the minimum cost worker from each heap
   - Select the worker with the lower cost (or from the first heap in case of a tie)
   - Add the selected worker's cost to the total
   - Replace the selected worker with a new worker from the middle section if available
4. Return the total cost after k selections

### Analogy
Think of this process like hosting a talent competition with two separate audition rooms - one on the east side of town and one on the west side.

- In each room, there are 'candidates' number of contestants lined up by their ID numbers
- You have a talent scout in each room who quickly identifies the contestant with the lowest salary demand
- For each of k rounds, you compare the best offers from both rooms and hire the person asking for less money (or the east room person in case of a tie)
- After someone is hired from a room, the next person in line (from the middle area of town) comes in to replace them
- The process continues until you've hired exactly k people

This analogy works because the heaps act like the talent scouts, quickly identifying the minimum cost person from each group without having to check everyone. The pointers tracking the middle section represent the line of people waiting to enter the audition rooms.
