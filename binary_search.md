# Binary Search

## [Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the pick
#          1 if num is lower than the pick
#          0 if num is equal to the pick
# def guess(num: int) -> int:

class Solution:
    def guessNumber(self, n: int) -> int:
        left = 1
        right = n

        while left <= right:
            mid = left + (right - left) // 2
            result = guess(mid)

            if result == 0:
                return mid
            elif result == -1:
                right = mid - 1
            else:
                left = mid + 1

        return -1  # This line should never be reached
```

### Computational Complexity
- **Time Complexity**: O(log n) - Binary search reduces the search space by half in each iteration
- **Space Complexity**: O(1) - We only use a constant amount of extra space regardless of the input size

### Keywords and Hints
- "I tell you whether the number is higher or lower" - This indicates we get feedback after each guess, making this perfect for a binary search approach
- "I pick a number from 1 to n" - We have a clear, sorted range to search within
- "guess API returns -1, 0, or 1" - These return values provide the exact information needed to narrow our search in a binary search

### Key insights
- This is a classic binary search problem where we're looking for a specific value in a sorted range
- The key insight is that the guess API provides the comparison info we need to determine which half of the current range to focus on next
- Since we can eliminate half the search space with each guess, binary search is optimal

### Solution Intuition
- Instead of guessing each number sequentially (which would be O(n)), we can use binary search to find the answer in O(log n) time
- We start by guessing the middle number in the range
- Based on the feedback from the guess API, we either found the answer or need to look in the left or right half of the current range
- We repeat this process, halving our search space each time, until we find the target number

### Solution explanation
1. Initialize two pointers: `left = 1` and `right = n` to represent the current search range
2. While `left <= right`, compute the middle value `mid` of the current range
3. Call the `guess(mid)` API to check if our guess is correct
4. If `guess(mid)` returns 0, we found the answer and return `mid`
5. If `guess(mid)` returns -1, our guess is too high, so search in the lower half by setting `right = mid - 1`
6. If `guess(mid)` returns 1, our guess is too low, so search in the upper half by setting `left = mid + 1`
7. Continue this process until we find the answer

### Analogy
This is like finding a specific page in a book where you only know the page number is between 1 and n.

Imagine you're looking for page 250 in a 500-page book:
1. You open to the middle (page 250) - and you get lucky and find it immediately.
2. But let's say you were looking for page 75:
   - You open to page 250
   - Someone tells you "go earlier in the book"
   - You open to page 125 (halfway between 1 and 250)
   - Someone says "go earlier"
   - You open to page 62 (halfway between 1 and 125)
   - Someone says "go later"
   - You open to page 93 (halfway between 62 and 125)
   - And so on...

This is much more efficient than checking each page one by one. With each "guess," you eliminate half of the remaining pages from consideration, which is why binary search is so powerful for this type of problem.

## [Successful Pairs of Spells and Potions](https://leetcode.com/problems/successful-pairs-of-spells-and-potions/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def successfulPairs(spells, potions, success):
    n = len(spells)
    m = len(potions)

    # Sort potions for binary search
    potions.sort()

    # Initialize result array
    pairs = [0] * n

    for i in range(n):
        spell = spells[i]

        # Calculate the minimum potion strength needed
        min_strength_needed = (success + spell - 1) // spell

        # Binary search to find the first potion with strength >= min_strength_needed
        left, right = 0, m - 1

        while left <= right:
            mid = left + (right - left) // 2
            if potions[mid] >= min_strength_needed:
                right = mid - 1
            else:
                left = mid + 1

        # The number of valid potions is m - left
        pairs[i] = m - left

    return pairs
```

### Computational Complexity
- Time Complexity: O(n log m + m log m), where n is the length of spells and m is the length of potions. Sorting potions takes O(m log m), and for each spell we perform a binary search which takes O(log m).
- Space Complexity: O(n) for the result array, excluding the input.

### Keywords and Hints
- "Product of their strengths" indicates multiplication between two values
- "At least success" implies a minimum threshold to meet
- Large array sizes (up to 10^5) suggest an efficient O(n log m) solution rather than O(n*m)
- "Number of potions" suggests counting elements that meet a condition, which is often amenable to binary search when the array is sorted
- The problem is asking for counts rather than the specific pairs, which reinforces the binary search approach

### Key insights
1. We need to count how many potions will form successful pairs with each spell
2. For a fixed spell strength s, we need to find potions with strength >= success/s
3. This becomes a counting problem that can benefit from binary search
4. Sorting the potions array allows us to efficiently find the number of potions above a threshold
5. We can compute the minimum required potion strength for each spell and find how many potions meet this requirement

### Solution Intuition
For each spell, we need to find out how many potions will create a product that meets or exceeds the success threshold. Rather than checking every spell-potion combination (which would be O(n*m)), we can sort the potions array and use binary search to find the dividing point where potions become strong enough to create successful pairs with a given spell.

If spell strength is s, then we need potions with strength p where s * p ≥ success, or p ≥ success/s. The binary search allows us to quickly find the index of the first potion that meets this requirement, and then we can calculate how many potions satisfy the condition.

### Solution explanation
1. Sort the potions array in ascending order
2. For each spell, calculate the minimum potion strength needed: min_strength = ceiling(success/spell)
3. Use binary search to find the index of the first potion with strength ≥ min_strength
4. Count how many potions have at least this strength (total number of potions minus the index of the first valid potion)
5. Return the array of counts

The ceiling division is implemented using (success + spell - 1) // spell to avoid floating-point division and ensure we round up.

### Analogy
This algorithm is like a sports team recruiting players who need to meet a minimum physical requirement.

Imagine you're a basketball coach (spells) evaluating potential players (potions) for your team. You have a minimum combined score (success) that's the product of your coaching ability and the player's natural talent.

Instead of interviewing each player individually (the O(n*m) approach), you first have all players line up in order of increasing talent (sorting potions). Then for each coach, you calculate the minimum player talent needed and use a binary search to quickly find where in the line that talent level begins. All players from that point onward are suitable matches for that coach.

This is much more efficient than having each coach evaluate every single player, especially when you have many coaches and many players. The binary search allows you to "cut the line in half" repeatedly until you find exactly where the talent threshold is met.

## [Find Peak Element](https://leetcode.com/problems/find-peak-element/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def findPeakElement(nums):
    left, right = 0, len(nums) - 1

    while left < right:
        mid = left + (right - left) // 2

        # If mid element is smaller than its right neighbor,
        # then a peak exists on the right side
        if nums[mid] < nums[mid + 1]:
            left = mid + 1
        # Otherwise, a peak exists on the left side or at mid
        else:
            right = mid

    # When left == right, we've found a peak
    return left
```

### Computational Complexity
- **Time Complexity**: O(log n) - Binary search halves the search space at each step
- **Space Complexity**: O(1) - Only using a constant amount of extra space regardless of input size

### Keywords and Hints
- "**peak element**" - This indicates we're looking for local maxima, not the global maximum
- "**must write an algorithm that runs in O(log n) time**" - This is a direct hint to use binary search
- "**nums[-1] = nums[n] = -∞**" - This boundary condition simplifies the problem by ensuring the array always has at least one peak
- "**nums[i] != nums[i + 1]**" - This constraint ensures there are no plateaus, making binary search viable

### Key insights
1. Any array with the given constraints (no adjacent equal elements) must have at least one peak
2. When looking at the middle element, we can determine which half of the array contains a peak by checking if it's increasing or decreasing
3. If nums[mid] < nums[mid+1], we know a peak must exist on the right side
4. If nums[mid] > nums[mid+1], a peak must exist on the left side or at mid itself

### Solution Intuition
The intuition is to use the "hill climbing" strategy. Imagine you're standing at the middle of a mountain range. If the ground is sloping upward to your right, you should move right to find a peak. If it's sloping upward to your left, move left. By continually following the upward slope, you're guaranteed to find a peak.

### Solution explanation
1. We apply binary search to find any peak element
2. Initialize left and right pointers to the start and end of the array
3. For each middle element, compare it with its right neighbor:
   - If mid is smaller than its right neighbor, move left pointer to mid+1
   - Otherwise, move right pointer to mid
4. When left and right pointers meet, we've found a peak element

The reason this works is that when nums[mid] < nums[mid+1], the mid+1 element is either:
- A peak itself
- Part of an increasing sequence that leads to a peak

Similarly, when nums[mid] > nums[mid+1], either mid is a peak or there's a peak to its left.

### Analogy
Think of this problem as hiking in a mountain range. You're trying to find any summit, not necessarily the highest one.

You start in the middle of the range. From your position, you look to your right:
- If the terrain is going uphill to your right, then you know there must be at least one peak in that direction (even if it's far away), so you continue your search to the right.
- If the terrain is going downhill to your right, you know there must be a peak either where you are standing or somewhere to your left, so you search in that direction.

By following this strategy and halving your search area each time, you're guaranteed to find a peak efficiently, just as binary search guarantees we find our target in logarithmic time. This works because the problem only asks for any peak, not a specific one.

## [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code Solution
```python
import math

def minEatingSpeed(piles, h):
    left = 1
    right = max(piles)

    while left < right:
        mid = left + (right - left) // 2

        hours_needed = 0
        for pile in piles:
            hours_needed += math.ceil(pile / mid)

        if hours_needed <= h:
            right = mid
        else:
            left = mid + 1

    return left
```

### Computational Complexity
- **Time Complexity**: O(n log m), where n is the number of piles and m is the maximum pile size. The binary search takes O(log m) iterations, and in each iteration we iterate through all piles (O(n)).
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of input size.

### Keywords and Hints
- "Minimum integer k" - This suggests we need to optimize for the smallest value of k that meets the criteria, hinting at a binary search approach.
- "Each hour, she chooses a pile" - This means we need to calculate how many hours are needed for a given eating speed.
- "h hours" - The constraint that h can be very large (up to 10^9) suggests that a brute force approach checking every possible speed would time out.
- "Eating speed of k" - The fact that we're searching for a specific value k within a range suggests binary search.

### Key Insights
1. The key insight is recognizing that the problem is actually asking us to find the minimum eating speed that allows Koko to finish all bananas within h hours.
2. For any eating speed k, we can calculate exactly how many hours it would take to eat all piles.
3. If we increase k, the time needed decreases (or stays the same), making this a monotonic function.
4. The monotonic nature of the relationship between k and hours needed enables us to use binary search.

### Solution Intuition
The intuition is to use binary search to find the minimum eating speed. We know Koko can eat at a minimum speed of 1 banana per hour and at a maximum speed equal to the largest pile (since eating faster than that wouldn't help for the largest pile). So we do a binary search between these values to find the smallest k where she can finish in time.

### Solution Explanation
1. Initialize a binary search range where:
   - Left bound (minimum possible speed) = 1
   - Right bound (maximum needed speed) = max(piles)
2. For each middle value in the binary search:
   - Calculate how many hours Koko needs to eat all piles at that speed
   - If she can finish within h hours, this speed might work but we may find a slower one, so we search in the lower half
   - If she can't finish in time, we need a faster speed, so we search in the upper half
3. The binary search will eventually converge to the minimum speed where Koko can finish in h hours

### Analogy
Think of Koko's situation like a delivery truck driver who needs to make n deliveries within h hours. The driver is looking for the minimum driving speed needed:

- Each pile represents a delivery route of different lengths
- The eating speed k represents the driving speed (mph)
- For each route, the driver must complete the entire route, and time is rounded up to the nearest hour (you can't finish 1.5 hours of driving in 1 hour)
- If a route would take less than an hour at a given speed, the driver still spends a full hour on it (just like Koko won't eat from another pile in the same hour)

Just as the driver would test different speeds (not literally driving at every possible speed, but calculating), we use binary search to efficiently find the minimum speed that allows all deliveries to be completed within the time limit. Starting too slow means running out of time, while driving too fast wastes fuel (analogous to Koko wanting to eat slowly).
