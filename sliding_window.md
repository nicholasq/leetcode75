# Sliding Window

## [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def findMaxAverage(nums, k):
    # Calculate the sum of the first k elements
    current_sum = sum(nums[:k])
    max_sum = current_sum

    # Slide the window through the array
    for i in range(k, len(nums)):
        # Add the new element and remove the oldest element from the window
        current_sum += nums[i] - nums[i - k]
        # Update max_sum if current_sum is larger
        max_sum = max(max_sum, current_sum)

    # Return the maximum average
    return max_sum / k
```

### Computational Complexity
- Time Complexity: O(n) - We iterate through the array once. The initial sum calculation takes O(k) time, but since k ≤ n, the overall time complexity is O(n).
- Space Complexity: O(1) - We only use a constant amount of extra space regardless of input size.

### Keywords and Hints
- "Contiguous subarray" - This indicates we need to consider a continuous sequence of elements.
- "Length equal to k" - We are looking for fixed-size subarrays, which hints at using a sliding window approach.
- "Maximum average value" - We need to find the maximum sum of k elements and then divide by k.

### Key insights
1. Since we're looking for subarrays of fixed length k, we don't need to try all possible subarray lengths.
2. As we move the window through the array, we can efficiently update the sum by adding the new element and removing the element that's no longer in the window.
3. Finding the maximum average is equivalent to finding the maximum sum (since dividing by k is just a constant operation).

### Solution Intuition
Instead of recalculating the sum for each k-length subarray (which would be inefficient), we can use a sliding window approach. We start by calculating the sum of the first k elements. Then, as we move through the array, we add the next element and subtract the element that just left the window. This way, we maintain a current sum for each window with just O(1) operations per step.

### Solution explanation
1. Calculate the sum of the first k elements in the array.
2. Initialize `max_sum` to this initial sum.
3. Iterate through the array starting from index k:
   - Update the current sum by adding the new element and subtracting the element that's now outside the window.
   - Update `max_sum` if the current sum is larger.
4. Return `max_sum` divided by k to get the maximum average.

### Analogy
Think of this algorithm like a moving camera with a fixed frame size (k) that's trying to capture the scene with the brightest overall lighting.

As you move the camera frame along, instead of measuring the total brightness in each frame from scratch (which would be time-consuming), you simply account for what's entering the frame on one side and what's leaving the frame on the other side. If the new frame has a higher total brightness than any previous frame, you make a note of it.

At the end, the frame position with the highest total brightness will give you the maximum average brightness when you divide by the frame size. This is much more efficient than repositioning your camera to each possible location and measuring the total brightness from zero each time.

## [Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def maxVowels(s: str, k: int) -> int:
    vowels = set(['a', 'e', 'i', 'o', 'u'])

    # Count vowels in the first window of size k
    current_count = 0
    for i in range(k):
        if s[i] in vowels:
            current_count += 1

    max_count = current_count

    # Slide the window
    for i in range(k, len(s)):
        # Remove the contribution of the character leaving the window
        if s[i-k] in vowels:
            current_count -= 1

        # Add the contribution of the character entering the window
        if s[i] in vowels:
            current_count += 1

        max_count = max(max_count, current_count)

    return max_count
```

### Computational Complexity
- Time Complexity: O(n), where n is the length of the string s. We iterate through the string once.
- Space Complexity: O(1), as we only use a fixed-size set to store the vowels and a few variables.

### Keywords and Hints
- "Maximum number of vowels in any substring of length k" suggests a sliding window approach, as we need to consider all substrings of a fixed length.
- "Return the maximum" indicates we need to track the best result as we process the string.
- The constraint on s.length being up to 10^5 suggests we need an efficient O(n) solution rather than checking all possible substrings, which would be O(n*k).

### Key insights
- Instead of recounting vowels in each potential substring (which would be inefficient), we can use a sliding window approach to maintain a running count.
- When we slide the window, we only need to consider:
  1. The character that leaves the window (potentially decrementing our count)
  2. The character that enters the window (potentially incrementing our count)
- This approach allows us to compute the vowel count for each substring in O(1) time after the initial count.

### Solution Intuition
The intuition is to maintain a "sliding window" of size k over the string. As we slide, we update our vowel count by removing the contribution of the character that left the window and adding the contribution of the character that entered. This way, we avoid recounting all k characters for each potential substring.

### Solution explanation
1. Create a set of vowels for efficient lookup.
2. Initialize the count of vowels in the first window of size k.
3. Slide the window from left to right:
   - If the character leaving the window is a vowel, decrement the count.
   - If the character entering the window is a vowel, increment the count.
   - Update the maximum count seen so far.
4. Return the maximum vowel count.

### Analogy
Think of this like counting people wearing red shirts in a moving train with k compartments. Initially, you count all people wearing red shirts in the first k compartments. Then, as the train moves, for each new compartment that comes into view, you add the number of people wearing red shirts in it, and for each compartment that goes out of view, you subtract the number of people wearing red shirts in it. This way, you always know how many people are wearing red shirts in the k compartments currently visible, without having to recount everyone every time the train moves.

The train represents the string, each compartment represents a character, and people wearing red shirts represent vowels. The goal is to find the maximum number of people wearing red shirts (vowels) that can be seen at once in k consecutive compartments (substring of length k).

## [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def longestOnes(nums, k):
    left = 0
    zeros_count = 0
    max_length = 0

    for right in range(len(nums)):
        # If the current element is 0, increment the zeros counter
        if nums[right] == 0:
            zeros_count += 1

        # If we have too many zeros, shrink the window from the left
        while zeros_count > k:
            if nums[left] == 0:
                zeros_count -= 1
            left += 1

        # Update the maximum length
        max_length = max(max_length, right - left + 1)

    return max_length
```

### Computational Complexity
- Time Complexity: O(n), where n is the length of the input array. Each element is processed at most twice (once by the right pointer and at most once by the left pointer).
- Space Complexity: O(1), as we only use a constant amount of extra space regardless of the input size.

### Keywords and Hints
- "consecutive" suggests a contiguous subarray problem
- "flip at most k 0's" indicates a constraint that can be managed with a sliding window approach
- The constraint of allowing k flips (not requiring exactly k) suggests flexibility in our solution
- The binary nature of the array simplifies the problem since we only need to track zeros

### Key insights
1. We're looking for the longest sequence of 1's we can create by flipping at most k 0's
2. This is a perfect sliding window problem because we're looking for a contiguous subarray with a specific property
3. We need to maintain a window where the number of 0's is ≤ k
4. We can grow the window from the right and shrink from the left when needed
5. The window length at any point represents a valid sequence of consecutive 1's after flipping

### Solution Intuition
The core intuition is to maintain a sliding window that contains at most k zeros. As we iterate through the array, we keep track of the number of zeros in our current window. If including a new element would exceed our zero limit, we shrink the window from the left until we're back within our constraint. At each step, we update our maximum length. This approach allows us to find the longest valid subarray without having to actually perform any flips.

### Solution explanation
1. Initialize two pointers: `left` starts at 0, and `right` will iterate through the array
2. Use a counter `zeros_count` to keep track of how many 0's are in our current window
3. For each position of `right`:
   - If the current element is 0, increment `zeros_count`
   - If `zeros_count` exceeds k, shrink the window by moving `left` pointer forward and decreasing `zeros_count` if necessary
   - Calculate the current window size (`right - left + 1`) and update the maximum if needed
4. Return the maximum window size found

The sliding window approach ensures that we only need to scan the array once, making the solution efficient.

### Analogy
Imagine you're a city planner trying to build the longest possible pedestrian path along an existing road. The road has good segments (represented by 1's) and bad segments (represented by 0's). You have a budget to repair at most k bad segments of the road.

As you survey the road (from left to right):
- You keep track of how many bad segments you've marked for repair within your current consideration area
- When you hit a bad segment, you add it to your repair list
- If your repair list exceeds your budget (k), you have to remove the leftmost bad segment from your consideration
- Throughout this process, you keep track of the longest continuous area of road that could become a pedestrian path

The size of the largest consideration area you found represents the maximum consecutive "good" segments you can create with your limited repair budget, just like the maximum consecutive 1's we can have after flipping at most k 0's.

## [Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def longestSubarray(nums):
    left = 0
    zeros = 0
    max_length = 0

    for right in range(len(nums)):
        if nums[right] == 0:
            zeros += 1

        # If we have more than one zero, shrink the window from the left
        while zeros > 1:
            if nums[left] == 0:
                zeros -= 1
            left += 1

        # Current window size is (right - left + 1)
        # We need to subtract 1 because we must delete one element
        max_length = max(max_length, right - left)

    # If max_length is 0, it means all elements are 0s
    return max_length
```

### Computational Complexity
- **Time Complexity**: O(n) - Each element is processed at most twice (once when right pointer passes over it and once when left pointer passes over it)
- **Space Complexity**: O(1) - We only use a few variables regardless of input size

### Keywords and Hints
- "Longest subarray": This typically hints at a sliding window approach
- "After deleting one element": This is the key constraint - it means we can have at most 1 zero in our window
- "Return the size": We're tracking a maximum length, not the actual subarray
- "Binary array": Makes it easy to count specific elements (0s) within our window

### Key insights
1. We can rephrase the problem: find the longest subarray with at most one 0
2. After finding this subarray, we must delete one element (which will be the 0 if there is one)
3. A sliding window approach allows us to maintain and update the longest valid window
4. We need to shrink the window whenever we have more than one zero

### Solution Intuition
The problem asks for the longest subarray of 1's after deleting exactly one element. This means we're looking for the longest sequence where we have at most one 0 (which we'll delete). Using a sliding window, we can expand our window when we encounter 1's or the first 0, but must shrink it when we encounter a second 0. The final length of our answer is the window size minus 1 (for the element we must delete).

### Solution explanation
1. Initialize a sliding window with left and right pointers both at 0
2. Track the number of zeros in the current window and the maximum valid window length
3. Expand the window by moving the right pointer
4. When we encounter a 0, increment our zero counter
5. If we have more than 1 zero, shrink the window from the left until we have only 1 zero
6. Track the maximum valid window size at each step, but subtract 1 (for the element we must delete)
7. Return the maximum length found

### Analogy
Imagine you're a movie editor tasked with creating the longest possible sequence of action scenes by cutting exactly one scene from a movie. The action scenes are represented by 1's, and non-action scenes by 0's.

You watch the movie with two markers: one at the start of a potential sequence (left) and one at the current scene (right). As you move the right marker forward:
- If you encounter an action scene (1), great! Keep going.
- If you encounter one non-action scene (0), you can plan to cut it out, so you mark it but keep going.
- If you encounter a second non-action scene, you know you can only cut one, so you must move your left marker forward until you've passed one of the non-action scenes.

At each step, you note the length of your potential sequence (considering you'll cut one scene). The longest such sequence is your answer.

This is exactly how our sliding window algorithm works - constantly adjusting the window to maintain a valid state (at most one zero) while tracking the maximum valid length.
