# Bit Manipulation

## [Counting Bits](https://leetcode.com/problems/counting-bits/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def countBits(n: int) -> list[int]:
    ans = [0] * (n + 1)
    for i in range(1, n + 1):
        ans[i] = ans[i >> 1] + (i & 1)
    return ans
```

### Computational Complexity
- Time Complexity: O(n) - we iterate through the numbers from 1 to n exactly once
- Space Complexity: O(n) - we store an array of size n+1 to return the result

### Keywords and Hints
- "number of 1's in the binary representation" - this suggests bit manipulation
- "linear time O(n)" - indicates we need to avoid calculating each number's bits independently
- "possibly in a single pass" - hints at using previously calculated results
- "without using any built-in function" - means we need to implement our own bit counting logic

### Key insights
1. The number of bits in i can be related to the number of bits in i/2
2. When we right shift a number (i >> 1), we essentially divide it by 2 and remove the least significant bit
3. If a number is even, removing the last bit doesn't change the bit count
4. If a number is odd, removing the last bit reduces the count by 1

### Solution Intuition
For any number i, we can express it as:
- Number of 1 bits in (i / 2) + whether the least significant bit is 1

For example:
- 5 (101) has 2 bits set
- 2 (10) has 1 bit set
- 5 >> 1 = 2, and 5 is odd (last bit is 1)
- So, bits in 5 = bits in 2 + 1 = 1 + 1 = 2

This gives us a recurrence relation:
- bits[i] = bits[i >> 1] + (i & 1)

### Solution explanation
1. We create an array `ans` to store the number of 1 bits for each number from 0 to n
2. We initialize ans[0] = 0 (since 0 has no bits set)
3. For each number i from 1 to n:
   - We calculate the number of bits using the formula: ans[i] = ans[i >> 1] + (i & 1)
   - i >> 1 gives us the number with its last bit removed (integer division by 2)
   - i & 1 checks if the last bit is 1 (for odd numbers) or 0 (for even numbers)
4. We return the final ans array

### Analogy
Think of counting bits like tracking generations in a family tree with a peculiar inheritance rule:

Each person (number) inherits all the traits (1 bits) from their parent (number/2). Additionally, every odd-numbered person has one extra trait that their parent doesn't have.

For example:
- Person #4 (100) inherited all traits from Person #2 (10)
- Person #5 (101) inherited all traits from Person #2 (10) plus one additional trait

When you want to know how many traits a person has, you don't need to check them individually. You can simply look up their parent's traits and add 1 if they're odd-numbered. This allows you to fill out the entire family tree by only looking at each person once (O(n) time) rather than examining every trait of every person (O(n log n) time).

## [Single Number](https://leetcode.com/problems/single-number/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def singleNumber(nums):
    result = 0
    for num in nums:
        result ^= num
    return result
```

### Computational Complexity
- Time Complexity: O(n) where n is the length of the input array. We iterate through each element exactly once.
- Space Complexity: O(1) since we only use a single variable regardless of input size.

### Keywords and Hints
- "Every element appears twice except for one" - This hints at a parity problem where we need to find an odd occurrence.
- "Linear runtime complexity" - Indicates we need a solution that doesn't rely on sorting (which would be O(n log n)).
- "Constant extra space" - Rules out hash-based solutions that would use O(n) space.
- These constraints together strongly point to a bitwise operation solution.

### Key insights
- When a number is XORed with itself, the result is 0.
- When a number is XORed with 0, the result is the number itself.
- XOR operation is commutative and associative, meaning the order of operations doesn't matter.
- Therefore, if we XOR all numbers in the array, the duplicates will cancel out (become 0), and only the single number will remain.

### Solution Intuition
- XOR has the property that a ⊕ a = 0 (any number XORed with itself gives 0)
- 0 ⊕ a = a (any number XORed with 0 gives the number itself)
- So if we XOR all numbers together, pairs will cancel out to 0, and the unique number will remain

### Solution explanation
The algorithm works by initializing a variable `result` to 0 and then XORing it with each number in the array. Because of the properties of XOR, when a number appears twice, it effectively cancels itself out (as a ⊕ a = 0). Since all numbers except one appear exactly twice, they will all cancel out, leaving only the single number in the result. This allows us to find the single number in O(n) time with O(1) space.

### Analogy
Imagine a room full of people forming pairs, with one person left without a partner. If each pair wears the same color shirt, and you're tasked with finding the person without a partner, you could use a "cancellation" approach.

If each color cancels itself out when counted twice (like red + red = nothing, blue + blue = nothing), then after accounting for all people, the only color that would remain is the one worn by the single person without a partner.

The XOR operation works exactly like this color-cancellation system - pairs cancel out completely, leaving only the element that appears once. Just as you wouldn't need to separately track each color in the room (saving space), the XOR operation handles the cancellation without requiring additional memory for each number.

## [Minimum Flips to Make a OR b Equal to c](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def minFlips(a: int, b: int, c: int) -> int:
    flips = 0

    # Process each bit position
    while a > 0 or b > 0 or c > 0:
        # Extract the least significant bit of each number
        bit_a = a & 1
        bit_b = b & 1
        bit_c = c & 1

        # Check the condition (a OR b) == c for the current bit
        if (bit_a | bit_b) != bit_c:
            if bit_c == 0:
                # If c has a 0 but (a OR b) has a 1, we need to flip all 1s to 0s
                flips += bit_a + bit_b
            else:
                # If c has a 1 but (a OR b) has a 0, we need one flip to make a 1
                flips += 1

        # Right shift all numbers to process the next bit
        a >>= 1
        b >>= 1
        c >>= 1

    return flips
```

### Computational Complexity
- **Time Complexity**: O(log(max(a, b, c))), since we process each bit of the numbers and the number of bits is log of the maximum value.
- **Space Complexity**: O(1), as we use a constant amount of extra space regardless of input size.

### Keywords and Hints
- **Bitwise OR operation**: The problem explicitly asks about making `a OR b == c`, which suggests we need to work at the bit level.
- **Flips**: Each flip changes a single bit, meaning we need to count the minimum number of bit changes required.
- **Positives numbers**: Since all numbers are positive, we can process them bit by bit until all numbers become 0.

### Key insights
1. We need to compare each bit position of `a OR b` with the corresponding bit of `c`.
2. For any bit position where `(a OR b) != c`, we need to determine how many flips are needed:
   - If `c` has a 0 but `a OR b` has a 1, we need to flip all 1's in `a` and `b` to 0's.
   - If `c` has a 1 but `a OR b` has a 0, we only need to flip one bit (either in `a` or `b`) to make it 1.
3. We can process the numbers bit by bit from least significant to most significant until all become 0.

### Solution Intuition
The intuition is to check each bit position of the three numbers and determine if flips are needed. Since the bitwise OR of two 1's or one 1 and one 0 results in a 1, and the OR of two 0's results in a 0, we have two cases to consider:
1. If `c` has a 0 at a position, then both `a` and `b` must have 0's at that position (otherwise the OR will be 1).
2. If `c` has a 1 at a position, then at least one of `a` or `b` must have a 1 at that position (otherwise the OR will be 0).

### Solution explanation
The solution iterates through each bit position of the three numbers:
1. Extract the least significant bit of each number using the bitwise AND with 1.
2. Check if `(bit_a | bit_b) != bit_c`:
   - If `bit_c` is 0 but `(bit_a | bit_b)` is 1, then we need to flip all the 1's to 0's. That means we add `bit_a + bit_b` flips (which will be either 1 or 2).
   - If `bit_c` is 1 but `(bit_a | bit_b)` is 0, then we need to flip one bit to make it 1. That's 1 flip.
3. Right shift all numbers by 1 to process the next bit.
4. Continue until all numbers become 0.
5. Return the total number of flips.

### Analogy
Imagine you have two light switches (representing bits in `a` and `b`) and a light bulb (representing the bit in `c`). The light bulb follows OR logic — it illuminates if either or both switches are ON.

Now, your task is to adjust the switches (with minimal effort) so that the light bulb's state matches a desired state:
- If the bulb should be OFF (c has 0), then both switches must be OFF. If any switch is ON, you need to flip it OFF.
- If the bulb should be ON (c has 1), at least one switch must be ON. If both are OFF, you only need to flip one switch to ON.

For each position in our binary numbers, we're essentially checking if our "light setup" (a and b) produces the desired "illumination pattern" (c), and if not, making the minimum number of switch flips to achieve it.
