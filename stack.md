# Stack

## [Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def removeStars(s: str) -> str:
    stack = []

    for char in s:
        if char == '*':
            if stack:  # If stack is not empty
                stack.pop()  # Remove the top element
        else:
            stack.append(char)  # Add the character to the stack

    return ''.join(stack)
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the length of the input string. We iterate through each character in the string once, and each operation (push or pop) on the stack takes O(1) time.
- **Space Complexity**: O(n) in the worst case, where the stack needs to store most or all of the characters in the input string.

### Keywords and Hints
- "Remove the closest non-star character to its left" - This suggests a Last-In-First-Out (LIFO) behavior, which is a characteristic of stack data structures.
- "Performing the removals from left to right" - This hints at processing the string sequentially from left to right.
- "The operation is always possible" - This guarantees that we'll never encounter a star with no character to remove, which simplifies our solution.

### Key insights
- Each time we encounter a star, we need to remove the most recently added non-star character.
- A stack is perfect for this "last-in, first-out" behavior.
- We only need to keep track of characters that haven't been "canceled" by stars.

### Solution Intuition
The problem can be viewed as a sequence of character additions and deletions. When we see a regular character, we add it to our result. When we see a star, we delete the most recently added character. This naturally maps to a stack's push and pop operations.

### Solution explanation
1. Initialize an empty stack to store characters.
2. Iterate through each character in the input string:
   - If the character is a star ('*'), pop the top element from the stack (if the stack is not empty).
   - If the character is not a star, push it onto the stack.
3. After processing all characters, the stack contains only the characters that weren't "canceled" by stars.
4. Convert the stack to a string and return it as the result.

This approach ensures that each star removes the closest non-star character to its left, as the top of the stack always represents the most recently added character that hasn't been removed yet.

### Analogy
Imagine you're stacking books on a table, and you have a special rule: whenever you come across a bookmark (representing a star), you must remove the most recently placed book along with the bookmark itself.

As you go through a pile of books and bookmarks:
- If you pick up a book, you add it to your stack on the table.
- If you pick up a bookmark, you remove the topmost book from the stack along with the bookmark.

After processing all books and bookmarks, the books remaining on the table represent the final result. This is exactly how our algorithm works: adding characters to the stack and removing them when stars are encountered.

This analogy captures the LIFO nature of the problem, where the most recently added item is the first to be removed when a deletion operation is required.

## [Asteroid Collision](https://leetcode.com/problems/asteroid-collision/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def asteroidCollision(asteroids):
    stack = []

    for asteroid in asteroids:
        # Handle the collision
        while stack and asteroid < 0 and stack[-1] > 0:
            collision = stack[-1] + asteroid

            # If the top asteroid in stack is smaller, it explodes
            if collision < 0:
                stack.pop()
                continue
            # If they're equal, both explode
            elif collision == 0:
                stack.pop()
                asteroid = 0  # Marking current asteroid to be ignored
                break
            # If the current asteroid is smaller, it explodes
            else:
                asteroid = 0  # Marking current asteroid to be ignored
                break

        # If the asteroid wasn't destroyed, add it to the stack
        if asteroid != 0:
            stack.append(asteroid)

    return stack
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the number of asteroids. Each asteroid is processed once and may be compared with asteroids in the stack, but each asteroid can only be popped from the stack once throughout the entire algorithm.
- **Space Complexity**: O(n) for the stack in the worst case (when no collisions occur).

### Keywords and Hints
- **"Collision"**: This hints at interaction between elements, which often suggests a stack-based approach.
- **"Moving in opposite directions"**: This suggests we need to track direction and only process collisions in certain cases.
- **"Explode"**: Elements get removed when certain conditions are met, which aligns with stack operations.
- **"Same direction will never meet"**: This means we only need to worry about collisions between elements of opposite signs.

### Key insights
1. **Stack to track potential collisions**: Using a stack allows us to efficiently handle the interaction between asteroids.
2. **Direction matters**: Only asteroids moving in opposite directions can collide (positive value moving right, negative value moving left).
3. **Collision management**: When a negative asteroid (moving left) encounters positive asteroids (moving right) in the stack, we need to compare sizes to determine which one explodes.
4. **Order of processing**: Process asteroids from left to right to correctly simulate their movement and potential collisions.

### Solution Intuition
The intuition is to simulate the asteroid movement from left to right. For each asteroid, we check if it will collide with any asteroid already in our result. A collision can only happen if the current asteroid is moving left (negative) and the previous asteroid is moving right (positive). When a collision happens, the smaller asteroid explodes, and if they're the same size, both explode.

### Solution explanation
1. We use a stack to keep track of surviving asteroids.
2. For each asteroid in the input:
   - If the stack is empty or no collision is possible (current asteroid moving right or top of stack moving left), push the asteroid to the stack.
   - If a collision is possible (current asteroid moving left and top of stack moving right):
     - Compare sizes to determine which asteroid explodes.
     - If the top asteroid of the stack is smaller, pop it and continue checking with the next asteroid in the stack.
     - If both are of equal size, pop the top asteroid and ignore the current one.
     - If the current asteroid is smaller, just ignore it.
3. Return the stack as the final state of asteroids.

### Analogy
Think of this problem like a game of bowling, where the pins are arranged in a line. Each asteroid is like a bowling ball, with its size representing the weight of the ball, and its direction (+ or -) representing whether it's rolling from left to right or right to left.

When two bowling balls rolling in opposite directions meet, the heavier ball continues rolling, while the lighter one gets knocked off the lane. If they have the same weight, both get knocked off. Balls rolling in the same direction never interfere with each other.

The stack in our algorithm is like keeping track of which bowling balls remain on the lane as we introduce new balls one at a time from one end. We only need to check for collisions when a ball is rolling in a direction that could cause it to hit previously introduced balls.

## [Decode String](https://leetcode.com/problems/decode-string/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def decodeString(s: str) -> str:
    stack = []
    curr_num = 0
    curr_str = ""

    for c in s:
        if c.isdigit():
            curr_num = curr_num * 10 + int(c)
        elif c == '[':
            # Push the current number and string onto the stack
            stack.append((curr_str, curr_num))
            # Reset current string and number
            curr_str = ""
            curr_num = 0
        elif c == ']':
            # Pop the previous string and repeat count
            prev_str, repeat_count = stack.pop()
            # Update current string by adding the previous string and the repeated current string
            curr_str = prev_str + curr_str * repeat_count
        else:  # c is a letter
            curr_str += c

    return curr_str
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the length of the input string. Although we might have nested loops when multiplying strings, the total number of operations is still proportional to the output size, which is guaranteed not to exceed 10^5.
- **Space Complexity**: O(n), as the stack can grow up to the size of the input string in the worst case (highly nested patterns).

### Keywords and Hints
- **"k[encoded_string]"**: This pattern indicates a nested structure, which hints at using a stack.
- **"repeated exactly k times"**: This suggests we need to keep track of multipliers.
- **"square brackets are well-formed"**: This confirms we can use a stack-based approach for handling nested brackets.
- **"digits are only for those repeat numbers"**: This simplifies parsing, as we can easily identify when a number or a string segment begins/ends.

### Key insights
1. The problem involves a recursive structure (strings within brackets that may contain more brackets).
2. Stack data structure is ideal for handling nested expressions.
3. We need to keep track of both the current string segment and the multiplier.
4. When we encounter a closing bracket, we need to pop the previous context and apply the multiplication.

### Solution Intuition
The core idea is to use a stack to keep track of the nested structure. Each time we encounter an opening bracket, we save the current context (string built so far and the multiplier) and start fresh. When we encounter a closing bracket, we retrieve the previous context, apply the multiplication to the current string, and combine it with the previous string.

### Solution explanation
1. Initialize an empty stack, current number (curr_num) as 0, and current string (curr_str) as empty.
2. Iterate through each character in the input string:
   - If the character is a digit, update curr_num accordingly.
   - If the character is '[', push the current string and number onto the stack, then reset both.
   - If the character is ']', pop the previous string and repeat count from the stack, multiply the current string by the repeat count, and prepend the previous string.
   - If the character is a letter, append it to the current string.
3. Return the final curr_str, which contains the fully decoded string.

### Analogy
Think of this algorithm like assembling nested Russian dolls. Each opening bracket '[' is like opening a new doll and placing the previous doll (along with a note about how many times to replicate the new doll) aside. As you work inside the new doll, you might encounter more dolls (nested brackets).

When you see a closing bracket ']', it's like finishing the current doll. You look at the note you set aside to see how many copies of this doll you need, make those copies, and then place them inside the previous doll you were working on.

The stack represents your "aside" space where you store unfinished dolls and their replication instructions. Just as with Russian dolls, the innermost expressions (dolls) are processed first, and then integrated into their containing expressions.
