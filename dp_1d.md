# Dynamic Programming - 1 Dimensional

## [N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code Solution
```python
def tribonacci(n: int) -> int:
    if n == 0:
        return 0
    if n == 1 or n == 2:
        return 1

    t0, t1, t2 = 0, 1, 1

    for _ in range(3, n + 1):
        t0, t1, t2 = t1, t2, t0 + t1 + t2

    return t2
```

### Computational Complexity
- Time Complexity: O(n) - we iterate from 3 to n once
- Space Complexity: O(1) - we only use three variables regardless of input size

### Keywords and Hints
- "Tribonacci sequence" - similar to Fibonacci but with three terms instead of two
- "Tn+3 = Tn + Tn+1 + Tn+2" - recurrence relation indicating how each new term depends on previous terms
- "0 <= n <= 37" - small constraint suggesting a straightforward iterative approach is sufficient
- "fit within a 32-bit integer" - no need for big integer handling

### Key Insights
- The problem can be solved with dynamic programming, but we only need to keep track of the last three values at any time
- Unlike traditional DP problems that require an array to store all solutions, this can be optimized to use only three variables
- The base cases (T0 = 0, T1 = 1, T2 = 1) are explicitly given

### Solution Intuition
- To get any Tribonacci number, we just need to know the previous three numbers
- Instead of storing the entire sequence, we can "slide" through the sequence by keeping track of just three values
- After calculating each new value, we shift our window of three values to the right

### Solution Explanation
1. Handle the base cases: T0 = 0, T1 = 1, T2 = 1
2. For n ≥ 3, initialize three variables to track the last three Tribonacci numbers
3. Iterate from 3 to n, calculating each new number as the sum of the previous three
4. At each step, update the three variables by shifting them (t0 becomes t1, t1 becomes t2, and t2 becomes the new sum)
5. After the iteration completes, t2 contains the nth Tribonacci number

### Analogy
Think of the Tribonacci calculation like a moving escalator with three steps. Initially, there are people standing on the steps with values 0, 1, and 1 (representing T0, T1, and T2).

At each moment, the escalator moves one step forward. The person on the first step moves to the second, the person on the second step moves to the third, and the person on the third step moves off. A new person then steps onto the first step with a value equal to the sum of the values carried by all three people who were on the steps before the movement.

For example:
- Start: [0, 1, 1]
- Move: [1, 1, 2] (the new person carries 0+1+1=2)
- Move: [1, 2, 4] (the new person carries 1+1+2=4)
- Move: [2, 4, 7] (the new person carries 1+2+4=7)

This continues until we've had n movements, and the value carried by the person on the third step is our answer.

## [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def minCostClimbingStairs(cost):
    n = len(cost)
    # Initialize dp array
    dp = [0] * (n + 1)

    # Base cases
    # dp[0] and dp[1] are both 0 because we can start from either step 0 or step 1

    # Fill the dp array
    for i in range(2, n + 1):
        dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])

    return dp[n]
```

### Computational Complexity
- Time complexity: O(n) - we iterate through the array once
- Space complexity: O(n) - we use an additional array of size n+1 for the dp table

Note: Space complexity can be optimized to O(1) by using only two variables instead of an array.

### Keywords and Hints
- "Climb one or two steps" suggests a dynamic programming approach where we have multiple choices at each step
- "Minimum cost" indicates an optimization problem
- "Return the minimum cost to reach the top" suggests we need to make optimal decisions at each step
- The ability to start from index 0 or 1 suggests unique base cases

### Key insights
1. At each step, we have two options: climb one step or climb two steps
2. We need to choose the option that minimizes the total cost
3. The problem has optimal substructure: the minimum cost to reach step i depends on the minimum costs to reach steps i-1 and i-2
4. We can use dynamic programming to build up our solution from smaller subproblems

### Solution Intuition
To reach the ith step, we can either:
1. Come from the (i-1)th step, paying its cost
2. Come from the (i-2)th step, paying its cost

Since we want the minimum cost, we'll choose the cheaper of these two options. We can build this solution from the bottom up, calculating the minimum cost to reach each step until we reach the top.

### Solution explanation
1. Create a dp array where dp[i] represents the minimum cost to reach the ith step
2. Initialize dp[0] = dp[1] = 0 since we can start from either step 0 or step 1 without paying any cost
3. For steps 2 and beyond, calculate dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
4. Return dp[n], where n is the length of the cost array, representing the top of the stairs

Note that dp[i] represents the minimum cost to reach the ith step, not including the cost of the ith step itself. This is why we add cost[i-1] when coming from the (i-1)th step.

### Analogy
Imagine you're collecting tolls on a highway where:
- Each mile marker has a specific toll amount (the cost)
- You have a special vehicle that can jump either 1 or 2 mile markers at a time
- You need to reach the end of the highway paying the minimum total tolls

At each decision point, you look ahead and consider:
1. Pay the toll at the current marker and jump 1 marker forward
2. Pay the toll at the current marker and jump 2 markers forward

You keep a record of the minimum toll paid to reach each marker. When you reach the final destination, you've optimized your route to pay the least amount of money.

The key insight is similar to planning a road trip where you're considering different routes and their associated costs to find the most economical path to your destination.

## [House Robber](https://leetcode.com/problems/house-robber/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def rob(nums):
    if not nums:
        return 0

    if len(nums) == 1:
        return nums[0]

    # Initialize dp array
    dp = [0] * len(nums)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])

    # Fill dp array
    for i in range(2, len(nums)):
        dp[i] = max(dp[i-1], dp[i-2] + nums[i])

    return dp[-1]
```

### Computational Complexity
- **Time Complexity**: O(n) where n is the number of houses. We iterate through the array once.
- **Space Complexity**: O(n) for the dp array.

### Keywords and Hints
- "adjacent houses have security systems connected" - This is the key constraint indicating we can't rob adjacent houses, suggesting we need to make a choice at each step.
- "maximum amount of money" - Optimization problem, hinting at dynamic programming.
- The examples show that the optimal solution requires skipping some houses, suggesting a pattern of selecting non-adjacent elements.
- The problem asks for a maximum value subject to constraints, which is a common characteristic of dynamic programming problems.

### Key insights
1. At each house, we have two options: rob it or skip it.
2. If we rob the current house, we can't rob the previous one.
3. If we skip the current house, the maximum money remains the same as the previous house.
4. This creates an optimal substructure where the solution to the larger problem can be constructed from solutions to smaller subproblems.
5. We need to track the maximum money at each house.

### Solution Intuition
For each house, we need to decide whether to rob it or skip it. If we rob it, we add its money to the maximum amount we could have stolen from houses before the previous one (since we can't rob adjacent houses). If we skip it, the maximum amount remains the same as what we could have stolen up to the previous house. We choose the option that gives us more money.

This can be expressed as:
- `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`
where `dp[i]` represents the maximum amount that can be robbed up to house `i`.

### Solution explanation
1. We use a dynamic programming approach with an array `dp` where `dp[i]` represents the maximum amount of money that can be robbed up to house `i`.
2. Base cases:
   - If there are no houses, return 0.
   - If there is only one house, rob it and return its value.
   - For the first house, `dp[0] = nums[0]` (rob it).
   - For the second house, `dp[1] = max(nums[0], nums[1])` (rob either the first or second house, whichever has more money).
3. For each subsequent house `i` (where `i ≥ 2`), we have two options:
   - Rob the current house and add its value to the maximum amount robbed up to two houses back: `nums[i] + dp[i-2]`
   - Skip the current house and keep the maximum amount robbed up to the previous house: `dp[i-1]`
4. We choose the maximum of these two options: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`.
5. After processing all houses, the last element of the `dp` array contains the maximum amount that can be robbed.

### Analogy
Imagine you're planning a road trip through a series of cities, each with a tourist attraction worth visiting. However, your car has a peculiar constraint: after visiting an attraction in one city, it needs a full day of maintenance, so you must skip the next city's attraction.

Each city's attraction has a certain "enjoyment value" (represented by the money in the houses). You want to maximize your total enjoyment during the trip.

At each city, you face a choice:
1. Visit the attraction in this city, gaining its enjoyment value, but then you must skip the next city.
2. Skip this city's attraction, maintaining the same total enjoyment from your previous stops.

As you progress through your journey, at each city you decide which option gives you more enjoyment in the long run. Your final decision at the last city represents the maximum possible enjoyment for your entire trip, just as the final value in our dp array represents the maximum amount of money that can be robbed.

## [Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def numTilings(n: int) -> int:
    MOD = 10**9 + 7

    # Base cases
    if n == 1:
        return 1
    if n == 2:
        return 2

    # dp[i][0] = number of ways to completely tile a 2xi board
    # dp[i][1] = number of ways to tile a 2xi board with top right cell missing
    # dp[i][2] = number of ways to tile a 2xi board with bottom right cell missing
    dp = [[0, 0, 0] for _ in range(n + 1)]

    # Initialize base cases
    dp[1][0] = 1  # One way to tile a 2x1 board (vertical domino)
    dp[1][1] = 0  # Can't have a missing cell in a 2x1 board
    dp[1][2] = 0  # Can't have a missing cell in a 2x1 board

    dp[2][0] = 2  # Two ways to tile a 2x2 board (2 horizontal or 2 vertical dominoes)
    dp[2][1] = 1  # One way to tile a 2x2 board with top right cell missing (L tromino)
    dp[2][2] = 1  # One way to tile a 2x2 board with bottom right cell missing (flipped L tromino)

    # Fill the dp table
    for i in range(3, n + 1):
        dp[i][0] = (dp[i-1][0] + dp[i-2][0] + dp[i-1][1] + dp[i-1][2]) % MOD
        dp[i][1] = (dp[i-2][0] + dp[i-1][2]) % MOD
        dp[i][2] = (dp[i-2][0] + dp[i-1][1]) % MOD

    return dp[n][0]
```

### Computational Complexity
- **Time Complexity**: O(n), as we only need to iterate through the array once from 3 to n, with constant work for each iteration.
- **Space Complexity**: O(n), for storing the dp array with three states for each value from 1 to n.

### Keywords and Hints
- **"two types of tiles"**: This suggests a dynamic programming approach where we need to consider various ways to place different types of tiles.
- **"return it modulo 10^9 + 7"**: This is a common requirement for problems with potentially large answers, indicating we should be careful with overflow.
- **"Two tilings are different if..."**: This emphasizes that we need to count distinct arrangements by considering how cells are covered.
- The examples showing different configurations hint that we need to carefully analyze and account for partial tilings.

### Key insights
1. The problem can be solved using dynamic programming by keeping track of different states of the board.
2. We need to track three states:
   - Fully tiled board of size 2×i
   - Board of size 2×i with top-right cell missing
   - Board of size 2×i with bottom-right cell missing
3. The second and third states are symmetric, but both need to be tracked for recurrence relations.
4. The recurrence relations account for all possible ways to extend a tiling by adding dominoes or trominoes.

### Solution Intuition
Instead of trying to directly count all possible tile arrangements, we can build up the solution by defining subproblems. At each step of filling a 2×n board, we can either have:
1. A completely filled board up to column i
2. A board filled up to column i with the top-right cell missing
3. A board filled up to column i with the bottom-right cell missing

By understanding how these states transition to each other (e.g., how adding a domino or tromino changes the state), we can develop recurrence relations to count all possible tilings.

### Solution explanation
The solution uses dynamic programming with three states for each board width:
1. `dp[i][0]`: number of ways to completely tile a 2×i board
2. `dp[i][1]`: number of ways to tile a 2×i board with top-right cell missing
3. `dp[i][2]`: number of ways to tile a 2×i board with bottom-right cell missing

The recurrence relations are:
- `dp[i][0] = dp[i-1][0] + dp[i-2][0] + dp[i-1][1] + dp[i-1][2]`
  - `dp[i-1][0]`: Add a vertical domino
  - `dp[i-2][0]`: Add two horizontal dominoes
  - `dp[i-1][1]`: Add a tromino to fill the top-right gap
  - `dp[i-1][2]`: Add a tromino to fill the bottom-right gap
- `dp[i][1] = dp[i-2][0] + dp[i-1][2]`
  - `dp[i-2][0]`: Add a tromino that leaves the top-right cell empty
  - `dp[i-1][2]`: Add a horizontal domino to the bottom, leaving the top-right empty
- `dp[i][2] = dp[i-2][0] + dp[i-1][1]`
  - `dp[i-2][0]`: Add a tromino that leaves the bottom-right cell empty
  - `dp[i-1][1]`: Add a horizontal domino to the top, leaving the bottom-right empty

We start with base cases for n=1 and n=2, then iterate to compute all values up to n, and return `dp[n][0]`.

### Analogy
Imagine you're playing with a very special jigsaw puzzle. The board is like a sidewalk (2 units high) of varying length, and you have two types of stones to place: rectangular stones (dominoes) and L-shaped stones (trominoes).

Think of the process like building a sidewalk by laying down these special stones. At each point of construction:
- You might have a completely finished section of sidewalk
- You might have a section with a small gap in the upper corner
- You might have a section with a small gap in the lower corner

As you continue building, you have different options based on your current state:
- If your sidewalk is complete up to a point, you can add stones in multiple ways
- If you have a gap, you have limited ways to continue building

Just like a real construction project where you track different states of completion, our algorithm tracks these different states and counts how many different ways we can complete the sidewalk for each possible length.
