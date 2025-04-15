# Monotonic Stack

## [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def dailyTemperatures(temperatures):
    n = len(temperatures)
    answer = [0] * n
    stack = []

    for i in range(n):
        # While stack is not empty and current temperature is warmer than the temperature at the stack's top index
        while stack and temperatures[i] > temperatures[stack[-1]]:
            prev_idx = stack.pop()
            answer[prev_idx] = i - prev_idx

        # Push current index to the stack
        stack.append(i)

    return answer
```

### Computational Complexity
- Time Complexity: O(n) where n is the length of the temperatures array. Each index is pushed and popped from the stack at most once.
- Space Complexity: O(n) in the worst case (for the stack) when temperatures are in decreasing order.

### Keywords and Hints
- "Wait after ith day" suggests a forward-looking approach, needing to track future temperatures
- "Number of days" implies we need to calculate differences between indices
- The pattern of comparing elements with future elements hints at using a stack data structure
- The problem involves finding the "next greater element" which is a classic monotonic stack problem

### Key insights
1. We need to find the next warmer day for each day.
2. A brute force approach would be O(n²), checking each future day for each current day.
3. Using a monotonic stack lets us process each temperature just once while keeping track of previous days that are waiting for a warmer temperature.
4. The indices stored in the stack form a decreasing temperature sequence.

### Solution Intuition
Instead of looking forward for each day to find a warmer temperature (which would be inefficient), we can process temperatures from left to right and use a stack to keep track of days for which we haven't found a warmer temperature yet. When we encounter a warmer temperature, we can update all the days in our stack that were waiting for this warmer day.

### Solution explanation
1. Initialize an array `answer` with all zeros and an empty stack.
2. Iterate through the temperatures array with index `i`:
   - While the stack is not empty and the current temperature is warmer than the temperature at the top of the stack:
     - Pop the index from the stack
     - Calculate the days difference between the current index and popped index
     - Update the answer for the popped index with this difference
   - Push the current index onto the stack
3. Return the answer array (indices remaining in the stack will have their default value of 0 since they never found a warmer day)

### Analogy
Imagine you're standing in a line of people, each holding a card with their body temperature. Everyone is facing forward, and they can only see the people in front of them. The goal is for each person to know how many people ahead they need to look until they find someone with a higher temperature.

A naive approach would be for each person to walk forward, counting steps until they find someone warmer. This is inefficient since many people will trace the same path.

Instead, we can use a more efficient approach: start from the back of the line and move forward. Each person looks at the temperature of the person directly in front. If that person is warmer, they know the answer is 1. If not, they can "skip ahead" by using the information the person in front has already calculated. This is similar to how the stack allows us to efficiently jump over days we've already processed, making the solution O(n) instead of O(n²).

## [Online Stock Span](https://leetcode.com/problems/online-stock-span/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
class StockSpanner:
    def __init__(self):
        # Stack of pairs: (price, span)
        self.stack = []

    def next(self, price: int) -> int:
        span = 1  # At minimum, span is 1 (today)

        # Pop all prices that are less than or equal to current price
        # and add their spans to the current span
        while self.stack and self.stack[-1][0] <= price:
            prev_price, prev_span = self.stack.pop()
            span += prev_span

        # Push the current price and its span to the stack
        self.stack.append((price, span))

        return span
```

### Computational Complexity
- **Time Complexity**:
  - Amortized O(1) per call to `next()`
  - While there's a while loop that could iterate through the stack, each element is pushed and popped at most once over all calls, making the amortized time complexity O(1)
- **Space Complexity**:
  - O(n) where n is the number of calls to `next()`
  - In the worst case, we might need to store all prices in the stack

### Keywords and Hints
- **"span"** - This indicates we need to count consecutive days with specific properties
- **"consecutive days (starting from that day and going backward)"** - Suggests we need to track history
- **"less than or equal to"** - Defines the comparison operator for determining the span
- **"daily price quotes"** - We receive prices sequentially, indicating an online algorithm
- The problem requires a data structure to efficiently track historical information while avoiding repeated calculations

### Key insights
1. For each price, we need to know how many consecutive previous prices were less than or equal to it
2. We don't need to store all previous prices, only those that might affect future calculations
3. Using a monotonic stack approach allows us to efficiently compute spans by skipping redundant comparisons
4. When we encounter a new price, we can "collapse" spans of smaller prices that came before it

### Solution Intuition
When a new stock price comes in, we need to count how many consecutive previous days had prices less than or equal to today's price. A naive approach would be to scan backward each time, but that's inefficient.

Instead, we can leverage a stack to maintain only the prices that might affect future spans. If today's price is higher than previous prices, those previous (lower) prices can never be the "stopping point" for future spans - they're essentially hidden behind today's price. So we can collapse them and just remember their combined span.

### Solution explanation
The solution uses a stack to store pairs of (price, span):
1. For each new price, we start with a span of 1 (just today)
2. We check if the current price is greater than or equal to the prices at the top of our stack
3. If it is, we pop that price from our stack and add its span to our current span
4. We repeat this process until we find a price greater than the current price
5. Finally, we push the current price and its calculated span onto the stack
6. The stack maintains only the necessary price history - prices that could potentially limit future spans

### Analogy
Think of a cityscape with buildings of different heights, where each building represents a day and its height represents the stock price that day. You're standing on the newest building (today) and looking backward.

The span is like counting how many consecutive buildings you can see that are shorter than or the same height as your current building. Once you encounter a taller building, your view is blocked, and the counting stops.

The stack approach is like taking snapshots of only the tallest buildings you've seen so far (from different viewpoints). When a new building is constructed, you only need to compare it with these tallest buildings, rather than all previous buildings. If the new building is taller than some previous "tallest" buildings, those buildings become irrelevant for future comparisons - they're permanently hidden behind the new, taller building.
