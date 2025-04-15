# Dynamic Programming - Multidimensional

## [Unique Paths](https://leetcode.com/problems/unique-paths/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def uniquePaths(m, n):
    # Create a 2D DP table
    dp = [[0 for _ in range(n)] for _ in range(m)]

    # Base case: there's only one way to reach any cell in the first row or first column
    for i in range(m):
        dp[i][0] = 1
    for j in range(n):
        dp[0][j] = 1

    # Fill the DP table
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]

    # Return the number of paths to the bottom-right corner
    return dp[m-1][n-1]
```

### Computational Complexity
- Time Complexity: O(m*n) - We need to fill every cell in the m×n grid exactly once.
- Space Complexity: O(m*n) - We use a 2D array of size m×n to store the intermediate results.

### Keywords and Hints
- **"Robot can only move either down or right"** - This restriction creates a classic dynamic programming problem where each state depends on a limited number of previous states.
- **"Number of possible unique paths"** - This hints at counting problems, which are often solved efficiently using dynamic programming.
- **"m x n grid"** - The grid structure with limited movement options suggests a 2D dynamic programming approach.

### Key insights
- For any cell (i,j), the number of ways to reach it is the sum of ways to reach the cell above it (i-1,j) and the cell to its left (i,j-1).
- This is because the robot can only move down or right, so it can only reach the current cell from these two directions.
- The first row and first column are special cases where there's only one way to reach each cell (by moving only right or only down).

### Solution Intuition
The intuition is to break down the problem into smaller subproblems. To reach any cell, the robot must have come from either the cell above or the cell to the left. Therefore, the number of ways to reach the current cell is the sum of ways to reach those two previous cells. This naturally leads to a dynamic programming solution where we build our answer from simpler cases.

### Solution explanation
1. Create a 2D array `dp` where `dp[i][j]` represents the number of unique paths to reach the cell at position (i,j).
2. Initialize the first row and first column of the DP table to 1, since there's only one way to reach any cell in the first row (moving right) or first column (moving down).
3. For each remaining cell (i,j), calculate `dp[i][j] = dp[i-1][j] + dp[i][j-1]` - meaning the ways to reach the current cell equals the sum of ways to reach the cell above and the cell to the left.
4. The answer is at `dp[m-1][n-1]`, which gives the number of unique paths to reach the bottom-right corner.

### Analogy
Think of the grid as a city with streets laid out in a perfect grid pattern. The robot starts at the northwest corner and wants to reach the southeast corner. Due to one-way street rules, the robot can only travel south or east at any intersection.

Now imagine water flowing from the northwest corner. At each intersection, the water splits and flows both south and east if possible. The amount of water reaching any intersection is the sum of water flowing from the north and from the west. After the water flows through the entire grid, the amount of water reaching the southeast corner represents the number of unique paths.

This analogy fits because:
1. Water naturally finds all possible paths
2. The cumulative nature of water flowing matches how we sum up the paths in our DP approach
3. The constraints (only moving south or east) are represented by the one-way streets

## [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def longestCommonSubsequence(text1, text2):
    m, n = len(text1), len(text2)

    # Create a 2D DP table with (m+1) rows and (n+1) columns
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # Fill the dp table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            # If characters match, take diagonal value + 1
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            # If characters don't match, take max of left or top
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    # Return the value in the bottom-right cell
    return dp[m][n]
```

### Computational Complexity
- **Time Complexity**: O(m * n) where m and n are the lengths of text1 and text2 respectively. We fill a 2D table with m+1 rows and n+1 columns.
- **Space Complexity**: O(m * n) for the dynamic programming table.

### Keywords and Hints
- **"Subsequence"**: This is a key indicator that we're not looking for a contiguous substring. Characters can be skipped but relative order must be maintained.
- **"Common to both strings"**: We need to find elements that exist in both strings in the same order, which suggests comparing both strings systematically.
- **"Longest"**: We're looking for the optimal solution, which is a classic sign that dynamic programming might be appropriate.
- The constraints (up to 1000 characters) suggest an O(m*n) solution would be acceptable.

### Key insights
1. The problem can be broken down into smaller, overlapping subproblems, making it ideal for dynamic programming.
2. When we encounter a matching character, we add 1 to the length of the LCS found so far (excluding the current characters).
3. When characters don't match, we need to decide whether to advance in string text1 or text2, choosing the option that gives us the longer subsequence.
4. The solution builds up from smaller subproblems to solve the complete problem.

### Solution Intuition
For each position (i, j) in our dynamic programming table, dp[i][j] represents the length of the longest common subsequence of text1[0...i-1] and text2[0...j-1]. When we find a matching character, we add 1 to the LCS that existed before these characters. When characters don't match, we take the maximum LCS that can be formed by either skipping the current character in text1 or text2.

### Solution explanation
1. Create a 2D array `dp` where dp[i][j] represents the length of the LCS of text1[0...i-1] and text2[0...j-1].
2. Initialize the first row and first column of the dp table to 0 (automatically done in Python).
3. Iterate through each character of both strings:
   - If the characters match (text1[i-1] == text2[j-1]), then dp[i][j] = dp[i-1][j-1] + 1 (extend the LCS by 1).
   - If the characters don't match, dp[i][j] = max(dp[i-1][j], dp[i][j-1]) (choose the best of skipping either the current character in text1 or text2).
4. The value in dp[m][n] gives the length of the LCS of the entire strings.

### Analogy
Imagine you're a detective trying to find similarities between two different stories (the two strings). You're reading both stories character by character, looking for common plot points (characters) that appear in the same order in both stories.

When you find a matching plot point, you add it to your "common storyline" and move forward in both stories. When elements don't match, you have two choices: either skip ahead in the first story to see if later elements match, or skip ahead in the second story. You always choose the path that ultimately gives you the longest common storyline.

Your detective notebook (the DP table) keeps track of the longest common storyline found so far for each combination of positions in the two stories. By the time you reach the end of both stories, your notebook contains the length of the longest common subsequence.

## [Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def maxProfit(prices, fee):
    n = len(prices)
    if n <= 1:
        return 0

    # Initialize states
    # hold: maximum profit when holding a stock
    # cash: maximum profit when not holding any stock
    hold = -prices[0]  # Buy the stock on day 0
    cash = 0           # Don't do anything on day 0

    for i in range(1, n):
        # Maximum of keeping the stock or buying a new one
        old_hold = hold
        hold = max(hold, cash - prices[i])

        # Maximum of keeping cash or selling the stock (minus fee)
        cash = max(cash, old_hold + prices[i] - fee)

    # We want to end with no stock
    return cash
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the length of the prices array. We only iterate through the array once.
- **Space Complexity**: O(1), as we only use two variables regardless of the input size.

### Keywords and Hints
- "**You may complete as many transactions as you like**" suggests we need to consider multiple buy/sell operations.
- "**You must sell the stock before you buy again**" indicates we can't hold multiple stocks at once.
- "**Transaction fee is only charged once for each stock purchase and sale**" hints that we need to incorporate the fee into our profit calculation when selling.
- These constraints point to a dynamic programming approach with two states (holding and not holding stock).

### Key insights
1. At any given day, we can be in one of two states: holding a stock or not holding any stock.
2. For each state, we need to track the maximum profit possible.
3. When transitioning between states, we need to account for the transaction fee when selling a stock.
4. The final answer is the maximum profit when not holding any stock at the end.

### Solution Intuition
The key intuition is to model this as a state machine with two states:
1. **Hold state**: We have a stock in our possession.
2. **Cash state**: We don't have any stock.

For each day, we decide whether to:
- Transition from cash to hold (buy a stock)
- Transition from hold to cash (sell a stock and pay the fee)
- Remain in our current state (do nothing)

Our goal is to maximize the profit at each state, and the final answer is the maximum profit in the cash state.

### Solution explanation
1. We use two variables to track our maximum profit in each state:
   - `hold`: Maximum profit when holding a stock
   - `cash`: Maximum profit when not holding any stock

2. For each day:
   - To update `hold`, we take the maximum of:
     - Keeping our current stock (staying in hold state)
     - Buying a new stock (transitioning from cash state)

   - To update `cash`, we take the maximum of:
     - Keeping our cash (staying in cash state)
     - Selling our stock and paying the fee (transitioning from hold state)

3. We initialize `hold` to `-prices[0]` (buying the stock on day 0) and `cash` to 0 (not doing anything on day 0).

4. After processing all days, our final answer is the `cash` state, as we want to end up not holding any stock.

### Analogy
Think of this problem as running a pawn shop where you buy and sell a single item repeatedly:

- You start with some cash and an empty shop.
- Each day, you have two options:
  1. Buy an item to put in your shop (if you don't already have one)
  2. Sell the item in your shop (if you have one)
  3. Do nothing

- Every time you sell an item, you have to pay a fixed fee to the marketplace.
- Your goal is to maximize your cash at the end of the trading period.

Just like in the algorithm, you're constantly deciding whether to keep your current status (cash or item) or change it based on today's market price. The fee represents a "friction" in the system that makes you consider whether a transaction is worth it - you won't sell if the profit doesn't exceed the fee, and you won't buy if you don't see enough potential future profit to justify the expense.

## [Edit Distance](https://leetcode.com/problems/edit-distance/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def minDistance(word1, word2):
    m, n = len(word1), len(word2)

    # Create a 2D DP table
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # Initialize the first row and column
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j

    # Fill the DP table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]  # No operation needed
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],      # Delete
                    dp[i][j-1],      # Insert
                    dp[i-1][j-1]     # Replace
                )

    return dp[m][n]
```

### Computational Complexity
- **Time Complexity**: O(m * n), where m and n are the lengths of word1 and word2 respectively. We need to fill a 2D table of size (m+1) x (n+1).
- **Space Complexity**: O(m * n) for the DP table.

### Keywords and Hints
- "Minimum number of operations" suggests an optimization problem, which often points to dynamic programming.
- The presence of three distinct operations (insert, delete, replace) that can be applied in any order implies a complex state space that's suitable for DP.
- "Convert word1 to word2" indicates a string transformation problem where we're looking for the shortest path from one state to another.
- The term "edit distance" itself is a well-known DP problem (also known as Levenshtein distance).

### Key insights
1. At each step, we have three possible operations: insert, delete, or replace a character.
2. The problem exhibits optimal substructure - the minimum edit distance for the entire strings depends on the minimum edit distances of their substrings.
3. We can build a solution incrementally by considering prefixes of both strings and determining the minimum operations needed to transform them.
4. When characters match, no operation is needed for that position.
5. When characters don't match, we choose the operation that results in the minimum total edit distance.

### Solution Intuition
The solution uses dynamic programming where dp[i][j] represents the minimum number of operations needed to convert the first i characters of word1 to the first j characters of word2.

When deciding dp[i][j]:
- If the current characters match (word1[i-1] == word2[j-1]), no operation is needed for these characters, so dp[i][j] = dp[i-1][j-1].
- If they don't match, we take the minimum of three operations:
  - Delete: dp[i-1][j] + 1 (delete from word1)
  - Insert: dp[i][j-1] + 1 (insert into word1)
  - Replace: dp[i-1][j-1] + 1 (replace in word1)

### Solution explanation
1. Create a 2D DP table of size (m+1) × (n+1), where m and n are the lengths of word1 and word2.
2. Initialize the first row to represent transforming an empty string to word2's prefixes (which requires j insertions).
3. Initialize the first column to represent transforming word1's prefixes to an empty string (which requires i deletions).
4. For each cell dp[i][j], calculate the minimum operations needed based on:
   - If the current characters match, no operation is needed for them.
   - If they don't match, choose the minimum cost operation from delete, insert, or replace.
5. The value at dp[m][n] gives the minimum edit distance between the entire strings word1 and word2.

### Analogy
Think of the edit distance problem as finding the shortest path in a city grid from one corner to the opposite corner.

- The city grid has m×n blocks (for strings of length m and n).
- At each intersection, you have three possible moves: go right (insert), go down (delete), or go diagonally (replace or match).
- Going diagonally costs nothing if the corresponding characters match (like a free diagonal shortcut), but costs 1 otherwise.
- Going right or down always costs 1 (like paying a toll).
- You want to find the cheapest path from the top-left corner (empty string to empty string) to the bottom-right corner (complete word1 to complete word2).

This analogy fits because in both cases, we're looking for the minimum cost path through a grid where each step has different costs based on certain conditions, and we need to make optimal choices at each step to find the global minimum.
