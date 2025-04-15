# Trie

## [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

### Computational Complexity
- **Time Complexity**:
  - `insert()`: O(m) where m is the length of the word
  - `search()`: O(m) where m is the length of the word
  - `startsWith()`: O(m) where m is the length of the prefix
- **Space Complexity**:
  - O(n * m) where n is the number of words and m is the average length of words
  - In the worst case, if all words are completely different, we need a separate node for each character

### Keywords and Hints
- "Prefix tree" - directly indicates that a trie data structure is needed
- "Efficiently store and retrieve keys in a dataset of strings" - hints at the need for a specialized data structure beyond arrays or hashmaps
- "Applications such as autocomplete and spellchecker" - suggests we need a structure with fast prefix lookup
- "startsWith" method - emphasizes the need for efficient prefix checking, which is a strength of tries

### Key insights
1. A trie is a specialized tree data structure specifically designed for efficient string operations
2. Each node represents a single character in the word path
3. Child nodes are represented as a map/dictionary where keys are characters and values are child nodes
4. Words are represented as paths from the root to leaf nodes (or marked nodes)
5. The key differentiator is marking end-of-word nodes to distinguish between complete words and prefixes

### Solution Intuition
The trie works like a tree where each path from root to a node represents a prefix. If the node is marked as end-of-word, then that path represents a complete word in our dictionary.

When inserting a word, we navigate down the tree, creating new nodes for characters not yet in the path. When searching, we follow the same path but only return true if we reach a node marked as end-of-word. For prefix checking, we just need to verify that the path exists, regardless of whether it's marked as a complete word.

### Solution explanation
The implementation uses two classes:
1. `TrieNode` - represents each character in the trie:
   - Has a hash map/dictionary of children nodes
   - Has a boolean flag indicating if this node represents the end of a valid word

2. `Trie` - the main class that provides the required operations:
   - `insert()`: Traverses or creates a path for the word through the trie, marking the last node as end-of-word
   - `search()`: Traverses the trie following the path of the word, returns true only if the final node is marked as end-of-word
   - `startsWith()`: Traverses the trie following the path of the prefix, returns true if the complete path exists (regardless of end-of-word status)

### Analogy
A trie is like a dictionary organized by letter sequences, similar to how a library organizes books. Imagine a library where each floor represents a different first letter, each room on that floor represents a second letter, each shelf a third letter, and so on.

When you insert "apple", you go to the 'a' floor, then the 'p' room, then the 'p' shelf, then the 'l' section, and finally the 'e' book location, where you place a marker indicating "this is a complete word."

When searching, you follow the same navigation path. If you reach the final destination and find the "complete word" marker, the word exists. If there's no marker, it means you've found a prefix but not a complete word.

When checking if a prefix exists, you only need to ensure you can navigate to that location, regardless of whether there's a "complete word" marker or not.

This hierarchical organization makes prefixes efficient to check, which is why libraries and dictionaries often use alphabetical order - it's a real-world implementation of similar prefix-based organization.

## [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
def suggestedProducts(products, searchWord):
    products.sort()  # Sort products lexicographically
    result = []
    prefix = ""

    for c in searchWord:
        prefix += c
        i = 0
        suggestions = []

        # Find products with matching prefix
        for product in products:
            if product.startswith(prefix) and i < 3:
                suggestions.append(product)
                i += 1

        result.append(suggestions)

    return result
```

A more efficient solution using binary search:

```python
def suggestedProducts(products, searchWord):
    products.sort()  # Sort products lexicographically
    result = []
    prefix = ""

    for c in searchWord:
        prefix += c

        # Binary search to find the first index where prefix matches
        left, right = 0, len(products) - 1
        while left <= right:
            mid = (left + right) // 2
            if products[mid] < prefix:
                left = mid + 1
            else:
                right = mid - 1

        # Collect up to 3 products that start with the prefix
        suggestions = []
        for i in range(left, min(left + 3, len(products))):
            if products[i].startswith(prefix):
                suggestions.append(products[i])
            else:
                break

        result.append(suggestions)

    return result
```

### Computational Complexity

For the first solution:
- Time Complexity: O(n log n + m * n), where n is the length of products array and m is the length of searchWord. Sorting takes O(n log n) and for each character in searchWord, we scan through products which is O(n).
- Space Complexity: O(n), for storing the result.

For the optimized solution using binary search:
- Time Complexity: O(n log n + m log n), where n is the length of products array and m is the length of searchWord. Sorting takes O(n log n) and for each character in searchWord, we perform a binary search that takes O(log n).
- Space Complexity: O(n), for storing the result.

### Keywords and Hints

Key terms in the problem:
- "Suggested products should have common prefix" - This suggests we need to find all products that share a prefix with each substring of the searchWord.
- "Lexicographically minimums products" - This indicates we need to sort the products array to get the lexicographically smallest words first.
- "After each character of searchWord is typed" - This tells us we need to build up the prefix one character at a time and find matches at each step.

### Key insights

1. Sorting the products array lexicographically allows us to easily find the lexicographically smallest matches.
2. For each character in the searchWord, we extend our prefix and find matching products.
3. Binary search can be used to efficiently find the starting index of potential matches in a sorted array.
4. Only checking up to 3 matches at each step is sufficient since the problem only asks for at most 3 suggestions.

### Solution Intuition

The intuition is to simulate the autocomplete experience as a user types each character:
1. Sort all products to ensure we can easily find the lexicographically smallest matches.
2. As each character is typed, extend our prefix and find products that match the current prefix.
3. If we've sorted our array, we can use binary search to quickly find where our matches begin.
4. Once we find where the prefix matches begin, we can take up to 3 consecutive products (if they match the prefix).

### Solution explanation

The solution works as follows:
1. We first sort the products array in lexicographical order.
2. For each character in the searchWord, we extend our current prefix.
3. We use binary search to find the first index in the sorted products array where the prefix could match.
4. Starting from that index, we check up to 3 products to see if they start with our current prefix.
5. We add the matching products to our suggestions for the current prefix.
6. We repeat this process for each character in the searchWord.
7. The result is a list of suggested products for each prefix (as each character is typed).

### Analogy

Imagine you're at a library with books arranged alphabetically on shelves. You're looking for books about "mouse":

1. You start by telling the librarian you want books that begin with "m".
   - The librarian goes to the "m" section and picks the first three books: maybe "machine," "magic," and "map".

2. Then you clarify you want books starting with "mo".
   - The librarian moves further along the shelf and finds the "mo" section, offering "mobile," "model," and "modern".

3. Next, you specify "mou".
   - The librarian moves to the "mou" section and finds "mouse" and "mountain".

4. Finally, you complete your search with "mouse".
   - The librarian stays in the same area and finds "mouse," "mousepad," and "mousing".

This is similar to our algorithm: as you type each letter, the system narrows down the search in the sorted list of products, finding the most relevant matches at each step. The binary search is like the librarian quickly navigating to the right section of the shelves without having to check every book.
