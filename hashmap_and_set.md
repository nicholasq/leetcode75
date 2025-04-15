# Hashmap / Set

## [Find the Difference of Two Arrays](https://leetcode.com/problems/find-the-difference-of-two-arrays/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def findDifference(nums1, nums2):
    set1 = set(nums1)
    set2 = set(nums2)

    result1 = list(set1 - set2)
    result2 = list(set2 - set1)

    return [result1, result2]
```

### Computational Complexity
- Time Complexity: O(n + m) where n is the length of nums1 and m is the length of nums2. Converting arrays to sets takes O(n) and O(m) time, and the set difference operations also take O(n) and O(m) time.
- Space Complexity: O(n + m) for storing the sets and result lists.

### Keywords and Hints
- "Distinct integers" hints at using sets to eliminate duplicates
- "Not present in" suggests a difference operation between collections
- The problem asks for elements that exist in one array but not the other, which directly maps to the set difference operation
- The arrays can be large (up to 1000 elements), so an efficient solution is needed

### Key insights
- We need to find elements unique to each array
- Set operations are perfect for this problem, particularly the set difference operation
- Converting the arrays to sets automatically handles duplicates
- Python's set difference operator (`-`) directly implements what we need

### Solution Intuition
The intuition is to convert both arrays into sets to leverage set operations. Sets automatically remove duplicates and offer an efficient way to check if an element exists in a collection. The set difference operation (A - B) gives all elements in set A that are not in set B, which is exactly what the problem asks for.

### Solution explanation
1. Convert both input arrays nums1 and nums2 to sets to remove duplicates
2. Find elements unique to nums1 by taking the set difference: set1 - set2
3. Find elements unique to nums2 by taking the set difference: set2 - set1
4. Convert both results back to lists and return them as a single list

### Analogy
Think of this algorithm like sorting through two toy collections. Imagine two children, Alice and Bob, each with their own toy box. To find which toys are unique to Alice, we'd place all of Alice's toys and all of Bob's toys on separate tables, removing any duplicates within each collection. Then we'd go through Alice's table and remove any toy that also appears on Bob's table. The remaining toys on Alice's table are unique to her collection. We'd do the same for Bob's collection.

The set conversion is like organizing toys on the tables (with duplicates removed), and the set difference is like the comparison process of removing common toys from each table, leaving only the unique ones.

## [Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def uniqueOccurrences(arr):
    # Count occurrences of each value
    count_dict = {}
    for num in arr:
        count_dict[num] = count_dict.get(num, 0) + 1

    # Check if the counts are all unique
    occurrences = count_dict.values()
    return len(occurrences) == len(set(occurrences))
```

### Computational Complexity
- Time Complexity: O(n), where n is the length of the input array. We iterate through the array once to count occurrences, and then convert values to a set.
- Space Complexity: O(n) in the worst case, where all elements in the array are unique, requiring storage for each element's count.

### Keywords and Hints
- "number of occurrences" suggests we need to count elements
- "unique" is the key word indicating we need to check for duplicate counts
- The problem focuses on the frequency of elements, not the elements themselves
- The constraint on array length (≤ 1000) suggests a simple counting approach is efficient enough

### Key insights
1. We need to track how many times each value appears in the array
2. The challenge is determining if any two different values appear the same number of times
3. A hash map for counting followed by a uniqueness check on the counts is a straightforward solution
4. We can leverage a set's property of containing only unique elements to check for duplicate counts

### Solution Intuition
The intuition is to first count how many times each number appears in the array, and then check if there are any duplicate counts. If we have the same count for different numbers, then the counts aren't unique, and we should return false. Otherwise, return true.

### Solution explanation
1. Create a dictionary (hash map) to count the occurrences of each number in the array
2. Iterate through the array, incrementing the count for each number encountered
3. Extract the count values from the dictionary
4. Compare the number of unique counts (using a set) with the total number of unique numbers
5. If they're equal, all counts are unique, so return true; otherwise, return false

### Analogy
Think of this problem like taking attendance in different classrooms of a school. Each classroom (representing a number in our array) has students (representing occurrences). We want to know if each classroom has a different number of students.

First, we go around counting how many students are in each classroom and write it down on a clipboard (our dictionary). Then, we look at our list of counts to see if any two classrooms have the same number of students. If each classroom has a unique count of students, we return true. If we find classrooms with identical student counts, we return false.

This works because just like we're checking if each number's frequency is unique in our algorithm, we're checking if each classroom has a unique number of students in our analogy.

## [Determine if Two Strings Are Close](https://leetcode.com/problems/determine-if-two-strings-are-close/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
from collections import Counter

def closeStrings(word1: str, word2: str) -> bool:
    # If the lengths are different, they cannot be close
    if len(word1) != len(word2):
        return False

    # Count the frequency of each character in both words
    counter1 = Counter(word1)
    counter2 = Counter(word2)

    # Check if both strings have the same set of characters
    if counter1.keys() != counter2.keys():
        return False

    # Check if the frequency counts (regardless of which character has which count)
    # are the same in both strings
    return sorted(counter1.values()) == sorted(counter2.values())
```

### Computational Complexity
- **Time Complexity**: O(n log n), where n is the length of the strings. The main complexity comes from sorting the frequency counts (which is O(k log k) where k is the number of unique characters, but k ≤ 26 for lowercase English letters, so it's effectively O(1)). Counting characters is O(n).
- **Space Complexity**: O(k) where k is the number of unique characters in the strings, which is at most 26 for lowercase English letters, so effectively O(1).

### Keywords and Hints
- **"Swap any two existing characters"** - This operation allows us to reorder characters freely, meaning the order doesn't matter.
- **"Transform every occurrence of one existing character into another existing character"** - This operation allows us to change character frequencies with each other, but only if both characters exist in the string.
- The constraints mention only **lowercase English letters**, which limits the character set to 26 possibilities.
- The description of Operation 2 tells us that character transformations must be bidirectional (a → b and b → a), suggesting that character sets must match.

### Key insights
1. If two strings can be made close, they must have the same set of characters (same character set).
2. The frequencies of characters can be rearranged through Operations 1 and 2, but the multiset of frequencies must be identical.
3. Operation 1 allows character reordering, so character positions don't matter.
4. Operation 2 allows swapping frequencies between characters, but not introducing new characters.

### Solution Intuition
The problem can be broken down into checking if:
1. Both strings have exactly the same set of characters (regardless of count)
2. The frequency counts can be matched (regardless of which character has which count)

If both conditions are met, we can transform one string into another using the given operations.

### Solution explanation
1. First, check if the lengths of both strings are equal (if not, return false).
2. Count the frequency of each character in both strings using Counter.
3. Check if both strings have exactly the same set of characters (if not, return false).
4. Sort the frequency counts of both strings and check if they are identical.
5. If all checks pass, return true.

The solution works because:
- Operation 1 allows us to reorder characters, so the position doesn't matter
- Operation 2 allows us to swap character identities, so which character has which frequency doesn't matter, but the set of frequencies must be the same
- Both strings must contain exactly the same set of characters

### Analogy
Imagine you have two sets of colored balls, where each color appears a certain number of times. For example:
- Set 1: 3 red balls, 2 blue balls, 1 green ball
- Set 2: 2 red balls, 3 blue balls, 1 green ball

Operation 1 is like rearranging the order of balls within a set - this doesn't change anything important for our comparison.

Operation 2 is like repainting all balls of one color to another, and vice versa. So, in our example, you could repaint all red balls to blue and all blue balls to red in Set 1, giving you:
- Set 1 after repainting: 3 blue balls, 2 red balls, 1 green ball

Now Set 1 and Set 2 have different counts for each color, but the same overall pattern of counts (3 of one color, 2 of another, 1 of a third).

Two sets are "close" if:
1. They contain the same colors (even if in different amounts)
2. They have the same pattern of quantities (even if for different colors)

This matches our algorithm's checks for the same character set and the same sorted frequency counts.

## [Equal Row and Column Pairs](https://leetcode.com/problems/equal-row-and-column-pairs/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def equalPairs(grid):
    n = len(grid)
    count = 0

    # Create a dictionary to store the count of each row (as a tuple)
    row_counts = {}
    for row in grid:
        row_tuple = tuple(row)
        row_counts[row_tuple] = row_counts.get(row_tuple, 0) + 1

    # Check each column against the rows
    for j in range(n):
        # Extract the column as a tuple
        col = tuple(grid[i][j] for i in range(n))
        # If this column exists as a row, add the count of those rows
        if col in row_counts:
            count += row_counts[col]

    return count
```

### Computational Complexity
- **Time Complexity**: O(n²), where n is the size of the grid. We need to iterate through all n rows and all n columns once.
- **Space Complexity**: O(n²) in the worst case, where all rows are distinct and need to be stored in the dictionary.

### Keywords and Hints
- "equal row and column pairs" - suggests we need to compare rows and columns
- "same elements in the same order" - indicates we're looking for exact matches, not just the same set of elements
- "n x n integer matrix" - the square nature of the grid means we can directly compare rows and columns
- "return the number of pairs" - we need to count matches, not just check if any exist

### Key insights
1. We need to efficiently compare each row with each column
2. Using a hash map (dictionary) allows us to count rows and quickly check if a column matches any row
3. Converting rows and columns to tuples makes them hashable, so they can be used as dictionary keys
4. We don't need to compare each row with each column directly (which would be O(n³))

### Solution Intuition
The brute force approach would compare each row with each column element by element, resulting in O(n³) time complexity. Instead, we can use a hash map to store the pattern of each row and then check if any column matches a stored row pattern. This brings the time complexity down to O(n²).

### Solution explanation
1. Create a dictionary to count occurrences of each row (converted to tuple for hashability)
2. For each column in the grid, extract it as a tuple
3. Check if this column exists as a key in our row dictionary
4. If it does, add the count of such rows to our result
5. Return the total count of equal row-column pairs

The solution works by first recording all rows and their frequencies, then checking each column against this record. When a column matches a row pattern, we increment our count by the number of such rows.

### Analogy
Imagine a library where books (rows) are arranged on shelves. Each book has a specific sequence of chapters, and you're trying to find books where the chapter sequence matches exactly with a specific reference list (columns).

Instead of checking each book page by page against each reference list (brute force), you create an index card system. For each unique book structure, you create a card listing all books that follow that structure. Then, when checking a reference list, you just need to look up if there's a matching card in your index system and count how many books are listed on that card.

This is similar to our solution where we hash the rows, allowing quick lookup when we check each column, rather than comparing elements individually for every row-column pair.
