# Prefix Sum

## [Find the Highest Altitude](https://leetcode.com/problems/find-the-highest-altitude/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def largestAltitude(gain):
    max_altitude = 0
    current_altitude = 0

    for g in gain:
        current_altitude += g
        max_altitude = max(max_altitude, current_altitude)

    return max_altitude
```

### Computational Complexity
- **Time Complexity**: O(n) where n is the length of the gain array. We iterate through the array once.
- **Space Complexity**: O(1) as we only use a constant amount of extra space regardless of input size.

### Keywords and Hints
- **"net gain in altitude"** indicates that we need to calculate the cumulative sum at each point.
- **"highest altitude"** suggests we need to track the maximum value encountered.
- The starting altitude is 0, which serves as our initial value.
- The problem mentions "n + 1 points" but gives an array of length n, indicating we need to consider the starting point separately.

### Key insights
- The altitudes can be derived by calculating the running sum of gain values.
- The highest altitude can be tracked while calculating these running sums.
- We need to compare each altitude with the current maximum to find the highest.
- The initial altitude is 0, so we must include it in our comparisons.

### Solution Intuition
The problem is asking us to find the maximum cumulative sum at any point in the array, given the initial value is 0. As we iterate through the array, we add each gain to our current altitude and keep track of the highest altitude we've seen so far.

### Solution explanation
1. Initialize `max_altitude` and `current_altitude` to 0 (starting altitude).
2. Iterate through the `gain` array:
   - Add the current gain to `current_altitude`.
   - Update `max_altitude` if the `current_altitude` is higher.
3. Return the `max_altitude`.

The solution works by calculating each altitude point by point. Since we only need to know the highest altitude, we just keep track of the maximum value as we go along, rather than storing all altitudes.

### Analogy
Think of this problem like hiking up and down a mountain trail with elevation changes. You start at sea level (altitude 0) and have a series of climbs and descents ahead of you.

- Each value in the `gain` array represents going uphill (positive value) or downhill (negative value).
- You want to know the highest point you reached during your entire journey.
- You carry an altimeter (the `current_altitude` variable) that tracks your current elevation.
- You also carry a record book (the `max_altitude` variable) where you note the highest elevation you've reached at any point.

As you walk the trail, your altimeter constantly updates. Whenever you see that your current altitude is higher than anything recorded in your book, you update the record. At the end of the trail, your record book shows the highest elevation you reached.

Just like in this hiking analogy, our algorithm continuously tracks both the current altitude and the maximum altitude seen during the journey, giving us the highest point reached.

## [Find Pivot Index](https://leetcode.com/problems/find-pivot-index/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def pivotIndex(nums):
    total_sum = sum(nums)
    left_sum = 0

    for i in range(len(nums)):
        # Right sum is the total sum minus left sum minus the current element
        right_sum = total_sum - left_sum - nums[i]

        if left_sum == right_sum:
            return i

        left_sum += nums[i]

    return -1
```

### Computational Complexity
- **Time Complexity**: O(n) where n is the length of the input array. We iterate through the array once after calculating the total sum.
- **Space Complexity**: O(1) as we only use a constant amount of extra space regardless of input size.

### Keywords and Hints
- **"pivot index"** - This suggests a balance point, which hints at comparing sums on either side
- **"left sum equals right sum"** - This explicitly tells us we need to find a point where sums on both sides are equal
- **"leftmost pivot index"** - Indicates that we should iterate from left to right and return the first valid index
- **"edge cases"** - The problem mentions that for elements at the edges, one of the sums would be 0, which is crucial for the solution

### Key insights
1. We need to find an index where the sum of elements to its left equals the sum of elements to its right.
2. Instead of recalculating the sums for each potential pivot, we can compute the total sum once and then derive the right sum for each index.
3. As we iterate through the array, we can incrementally update the left sum.

### Solution Intuition
The naive approach would be to check each index by calculating left and right sums separately for each potential pivot, resulting in O(n²) time complexity. However, we can be smarter:

1. Calculate the total sum of the array once.
2. For each index i, the left sum is the running sum we've seen so far.
3. The right sum is (total sum - left sum - current element).
4. If left sum equals right sum, we've found our pivot.

### Solution explanation
1. We calculate the total sum of all elements in the array.
2. We initialize a variable `left_sum` to 0.
3. We iterate through the array. For each element:
   - Calculate the right sum as `total_sum - left_sum - current_element`
   - Check if left sum equals right sum. If so, return the current index.
   - Update the left sum by adding the current element.
4. If no pivot index is found, return -1.

### Analogy
Think of the array as a see-saw (balance) with each element representing a weight positioned at different points. The pivot index is the point where if you place the fulcrum (the balance point of the see-saw), the weights on both sides would perfectly balance each other.

In our solution, instead of trying different positions for the fulcrum and recalculating the balance each time, we weigh the entire see-saw once. Then, as we move the fulcrum from left to right, we just need to know how much weight is on the left side so far, and can deduce the right side weight using the total. This saves us the effort of weighing both sides separately for each potential pivot position.
