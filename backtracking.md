# Backtracking

## [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def letterCombinations(digits: str) -> list[str]:
    # Return empty list for empty input
    if not digits:
        return []

    # Mapping of digits to letters
    phone_map = {
        '2': 'abc',
        '3': 'def',
        '4': 'ghi',
        '5': 'jkl',
        '6': 'mno',
        '7': 'pqrs',
        '8': 'tuv',
        '9': 'wxyz'
    }

    result = []

    def backtrack(index: int, current: str):
        # If we've processed all digits, add the combination to our result
        if len(current) == len(digits):
            result.append(current)
            return

        # Get the letters corresponding to the current digit
        letters = phone_map[digits[index]]

        # Try each letter and move to the next digit
        for letter in letters:
            backtrack(index + 1, current + letter)

    # Start backtracking from the first digit with an empty string
    backtrack(0, "")
    return result
```

### Computational Complexity
- **Time Complexity**: O(4^N * N), where N is the length of the input digits string. In the worst case, each digit maps to 4 letters (for digits 7 and 9), and we need to generate all combinations, which would be 4^N. Each combination requires O(N) time to construct.
- **Space Complexity**: O(N) for the recursion stack (depth of the recursion tree) plus O(4^N) for storing all the combinations. Overall, it's O(4^N).

### Keywords and Hints
- "All possible combinations" is a key phrase that often suggests a backtracking or recursive approach.
- The problem involves mapping digits to multiple letters, creating a branching decision tree.
- The constraint "0 <= digits.length <= 4" suggests that we can afford to generate all combinations (since even with all 4 digits being '9', we'd have at most 4^4 = 256 combinations).
- The request to "return all possible letter combinations" suggests that we need to enumerate all possibilities, not just count them.

### Key insights
- This is a classic backtracking problem where we need to explore all possible combinations.
- Each digit maps to a set of letters, creating branches in our decision tree.
- We need to build combinations by selecting one letter from each digit's mapping.
- The total number of combinations is the product of the number of letters for each digit.

### Solution Intuition
- For each digit, we have several letter choices. We need to try each one.
- After selecting a letter for the current digit, we move to the next digit and repeat.
- When we've processed all digits, we've formed a complete combination.
- Backtracking helps us explore all these choices systematically.

### Solution explanation
1. Create a mapping of digits to their corresponding letters.
2. Use a backtracking function that builds combinations one letter at a time.
3. At each step, we consider all possible letters for the current digit.
4. For each letter, we add it to our current combination and recursively process the next digit.
5. When we've processed all digits, we add the formed combination to our result.
6. Handle the edge case: if the input string is empty, return an empty array.

### Analogy
Think of this problem like navigating a multi-floor building where each floor represents a digit:
- You start on the ground floor (with an empty combination).
- Each floor has several doors (letters corresponding to the digit).
- You must choose one door on each floor and proceed to the next floor.
- Once you reach the top floor, you've formed a complete path (combination).
- To find all possible paths, you need to try every door on every floor.
- After exploring one path, you backtrack to a previous floor and try a different door.

This analogy captures the essence of backtracking - we systematically explore all possibilities by making a choice, moving forward, and then backtracking to try other choices.

## [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def combinationSum3(k, n):
    results = []

    def backtrack(start, combination, remaining_sum):
        # Base cases
        if len(combination) == k:
            if remaining_sum == 0:
                results.append(combination[:])
            return

        # Early termination if we can't reach the target
        if remaining_sum < 0 or len(combination) > k:
            return

        # Try each possible number
        for i in range(start, 10):
            combination.append(i)
            backtrack(i + 1, combination, remaining_sum - i)
            combination.pop()  # Backtrack

    backtrack(1, [], n)
    return results
```

### Computational Complexity
- **Time Complexity**: O(C(9,k) * k), where C(9,k) is the binomial coefficient (number of ways to choose k items from 9), which represents the number of possible combinations, and k is for creating each combination.
- **Space Complexity**: O(k) for the recursion stack and storing each combination (not counting the output space).

### Keywords and Hints
- "Combinations" suggests we need to find subsets, which often indicates backtracking
- "k numbers" indicates a fixed-length subset problem
- "Each number is used at most once" means we need to avoid reusing numbers
- "Only numbers 1 through 9 are used" limits our search space to just 9 numbers
- These constraints together point to a backtracking solution with pruning

### Key insights
1. This is a fixed-length subset problem (exactly k numbers)
2. We need to find combinations that sum to a specific target (n)
3. The search space is small and constrained (only digits 1-9)
4. Each number can only be used once (no duplicates)
5. Early pruning can be applied for efficiency

### Solution Intuition
The problem is asking for combinations (not permutations), which means the order doesn't matter. When solving combination problems, a common approach is to use backtracking. The idea is to build combinations incrementally, by making choices (including a number or not), then undoing those choices to explore other possibilities.

Since we're limited to using exactly k numbers from 1-9, and each number can only be used once, the search space is reasonably small. We can systematically explore all possible combinations by starting from 1 and moving up to 9, deciding at each step whether to include the current number in our combination.

### Solution explanation
The solution uses backtracking to find all valid combinations:

1. We define a recursive `backtrack` function that takes three parameters:
   - `start`: the current number we're considering (from 1 to 9)
   - `combination`: the current combination being built
   - `remaining_sum`: the remaining sum we need to reach (initially n)

2. Base cases:
   - If we've selected k numbers (len(combination) == k), check if the remaining sum is 0
   - If it is, we've found a valid combination; add it to our results
   - If not, this path won't lead to a solution

3. For each recursive call, we try all possible numbers from `start` to 9:
   - Add the current number to our combination
   - Recursively call backtrack with the next number, and the remaining sum decreased by the current number
   - Remove the current number from our combination (backtrack) to try other possibilities

4. We use early termination to avoid unnecessary recursion:
   - If remaining_sum becomes negative
   - If we've already selected more than k numbers

This approach systematically explores all possible combinations while efficiently pruning invalid paths.

### Analogy
Think of this problem like a locksmith trying different combinations on a safe with k dials. Each dial can be set to a number between 1 and 9, but no two dials can have the same number. The safe will only open if the sum of all dial positions equals exactly n.

The locksmith works methodically: starting with the first dial at position 1, then trying different positions for subsequent dials. If at any point the locksmith realizes the sum is already too large, or that even setting all remaining dials to their smallest possible values won't reach the target, they can immediately abandon that approach and try a different combination.

This is exactly what our backtracking algorithm does - it tries different "dial settings" (numbers in our combination), and backtracks whenever it determines a particular path won't yield a valid solution. The methodical exploration with early abandonment of invalid paths is what makes both the locksmith and our algorithm efficient.
