# Intervals

## [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def eraseOverlapIntervals(intervals):
    if not intervals:
        return 0

    # Sort intervals by end time
    intervals.sort(key=lambda x: x[1])

    count = 0
    end = intervals[0][1]

    # Greedy approach to keep intervals with earliest end time
    for i in range(1, len(intervals)):
        if intervals[i][0] < end:
            # Overlapping interval found, must remove it
            count += 1
        else:
            # Non-overlapping interval, update the end time
            end = intervals[i][1]

    return count
```

### Computational Complexity
- **Time Complexity**: O(n log n) - dominated by the sorting operation.
- **Space Complexity**: O(1) - we only use a constant amount of extra space (ignoring the space required by the sorting algorithm).

### Keywords and Hints
- "Minimum number of intervals to remove" suggests an optimization problem
- "Non-overlapping" indicates we need to identify and handle overlaps
- "Intervals" points to a classic interval scheduling problem
- The statement that intervals "which only touch at a point are non-overlapping" suggests we need to be careful about interval boundaries
- These keywords hint at a greedy approach, which is often efficient for interval scheduling problems

### Key insights
1. This is the classic "Activity Selection" or "Interval Scheduling" problem
2. Sorting by end time is crucial - it guarantees we can make locally optimal choices
3. After sorting, a greedy strategy works because keeping the interval with the earliest end time maximizes the room for future intervals
4. The solution counts removals rather than keeping a list of intervals to keep

### Solution Intuition
The intuition is to be greedy by preferring intervals that end earliest. Why? Because an interval that ends early gives us more room to accommodate future intervals. By sorting the intervals by end time and then iteratively selecting non-overlapping intervals, we minimize the number of intervals that need to be removed.

### Solution explanation
1. Sort all intervals by their end times.
2. Keep track of the end time of the last selected interval.
3. Iterate through the sorted intervals:
   - If the current interval starts before the end of the last selected interval, they overlap, so increment the removal counter.
   - If there's no overlap, select this interval and update the end time.
4. Return the count of removed intervals.

This greedy approach works because by always choosing the interval that ends earliest, we maximize our options for future intervals, thus minimizing the number we need to remove.

### Analogy
Imagine you're managing meeting room bookings for a single conference room. You have various meeting requests with different start and end times, but some meetings overlap. To accommodate as many meetings as possible, you'd prioritize shorter meetings or those that end earlier.

For example, if you have a meeting ending at 11:00 AM and another ending at 2:00 PM (both starting at 9:00 AM), you'd choose the 11:00 AM meeting because after it ends, you can potentially fit more meetings in the remaining time.

This is exactly what our algorithm does - by sorting by end times and greedily selecting intervals that don't overlap with previously selected ones, we maximize the number of intervals we can keep, thus minimizing the number we need to remove.

## [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def findMinArrowShots(points):
    if not points:
        return 0

    # Sort balloons by their end positions
    points.sort(key=lambda x: x[1])

    arrows = 1
    end_pos = points[0][1]

    for i in range(1, len(points)):
        # If the current balloon starts after the previous end position
        # we need a new arrow
        if points[i][0] > end_pos:
            arrows += 1
            end_pos = points[i][1]

    return arrows
```

### Computational Complexity
- Time Complexity: O(n log n) where n is the number of balloons. This is due to the sorting operation.
- Space Complexity: O(1) if we don't count the input or the sorting space, as we only use a few variables.

### Keywords and Hints
- "Balloons burst by an arrow if xstart <= x <= xend" - This suggests overlapping intervals.
- "Minimum number of arrows" - This hints at a greedy approach where we want to maximize the number of balloons burst with each arrow.
- The balloons are defined by their horizontal diameter only - We can sort and process them based on x-coordinates.
- "Shot vertically" - An arrow at position x will burst all balloons whose interval contains x.

### Key insights
1. If we sort the balloons by their end position, we can shoot an arrow at the end position of the first balloon.
2. This arrow will burst all overlapping balloons whose start position is less than or equal to the end position.
3. For any balloon not hit by the arrow, we repeat the process.
4. Sorting by end positions allows us to maximize the number of balloons hit by each arrow.

### Solution Intuition
The intuitive approach is to find groups of overlapping balloons and shoot one arrow per group. If we sort by end positions, we can place an arrow at the end of the first balloon. This will hit all balloons that overlap with it. Then we move to the next balloon not hit by this arrow and repeat.

### Solution explanation
1. Sort the balloons by their end positions.
2. Initialize the number of arrows to 1 and mark the end position of the first balloon as our current arrow position.
3. Iterate through the remaining balloons:
   - If a balloon's start position is after our current arrow position, it means the balloon won't be burst by the current arrow.
   - In this case, increment the arrow count and update the arrow position to the end of this new balloon.
4. Return the final arrow count.

### Analogy
Think of this problem as scheduling meetings in a meeting room with a twist. Each balloon is like a meeting with a start and end time. However, instead of finding how many rooms we need (which would be the maximum overlapping meetings), we're finding the minimum number of time slots to schedule "inspections," where each inspection time must fall within at least one meeting time.

For example, if you need to inspect several construction sites, and each site is only accessible during specific hours, you'd want to minimize the number of trips. You'd try to visit as many sites as possible during each trip. By sorting the sites by their closing times and visiting at the last possible moment, you can maximize the number of sites visited per trip, thus minimizing the total number of trips needed.
