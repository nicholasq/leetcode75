# Queue

## [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
from collections import deque

class RecentCounter:
    def __init__(self):
        self.requests = deque()

    def ping(self, t: int) -> int:
        # Add the new request
        self.requests.append(t)

        # Remove requests outside the time frame [t-3000, t]
        while self.requests and self.requests[0] < t - 3000:
            self.requests.popleft()

        # Return the number of requests in the range
        return len(self.requests)
```

### Computational Complexity
- **Time Complexity**:
  - O(1) amortized time per ping operation. While in the worst case we might need to pop multiple elements, each element is pushed and popped at most once, so over n operations, it's O(n) total or O(1) amortized per operation.
- **Space Complexity**:
  - O(n) where n is the maximum number of requests that could be in the window of 3000ms at any time.

### Keywords and Hints
- **"within a certain time frame"**: This suggests we need a way to track elements within a specific time window
- **"past 3000 milliseconds"**: This defines a sliding window of fixed size
- **"strictly larger value of t"**: This means timestamps are received in increasing order, which is crucial because it allows for a queue-based approach
- **"inclusive range [t - 3000, t]"**: Clearly defines our window boundaries

### Key insights
1. We only care about requests within a 3000ms window, so older requests can be discarded
2. Since timestamps arrive in ascending order, we can use a queue to efficiently manage the window
3. As a new ping comes in, we can remove all older pings that fall outside the window
4. The remaining count in our data structure is exactly the answer we need

### Solution Intuition
The problem is asking for a sliding window of requests. With each new request, the window shifts to include the new request and all requests within the last 3000ms. Since elements arrive in sorted order of time, a queue is the perfect data structure: we can add new elements at one end and remove expired elements from the other end.

### Solution explanation
The solution uses a deque (double-ended queue) to track requests:
1. Initialize an empty deque
2. For each ping(t):
   - Add the new timestamp t to the right end of the deque
   - Remove timestamps from the left end that are outside the range [t-3000, t]
   - Return the length of the deque, which represents all valid requests in the window

This is efficient because we only process each request once when adding it and potentially once when removing it, resulting in amortized O(1) operations per ping.

### Analogy
Think of this problem like a line of people waiting to enter a concert venue with a specific rule: only people who arrived within the last 50 minutes can stay in line.

1. When a new person arrives (a new ping), they join the back of the line
2. The security guard (our algorithm) checks the front of the line and asks people when they arrived
3. Anyone who arrived more than 50 minutes ago is asked to leave the line
4. When someone asks how many people are in line (the result of ping), we simply count everyone still standing there

The beauty of this system is that the guard only needs to check from the front of the line because people arrive in order. The guard never needs to check people in the middle or back of the line because they arrived more recently than those at the front.

This matches our algorithm because:
- The deque is the line of people
- Adding to the right end is a new person joining
- Removing from the left end is removing people who arrived too long ago
- Counting the length is checking how many valid people are in line

## [Dota2 Senate](https://leetcode.com/problems/dota2-senate/?envType=study-plan-v2&envId=leetcode-75)

*Claude 3.7 Sonnet:*

### Code solution
```python
from collections import deque

def predictPartyVictory(senate: str) -> str:
    n = len(senate)
    radiant = deque()
    dire = deque()

    # Queue up the senators by their indices
    for i, party in enumerate(senate):
        if party == 'R':
            radiant.append(i)
        else:
            dire.append(i)

    # Simulate the banning process
    while radiant and dire:
        r_idx = radiant.popleft()
        d_idx = dire.popleft()

        # Whoever has lower index gets to ban the other
        # The banned senator is out, the surviving senator gets to vote in the next round
        if r_idx < d_idx:
            radiant.append(r_idx + n)  # +n to ensure they vote in the next round
        else:
            dire.append(d_idx + n)

    # Return the party that has senators remaining
    return "Radiant" if radiant else "Dire"
```

### Computational Complexity
- **Time Complexity**: O(n), where n is the length of the senate string. In the worst case, we process each senator once.
- **Space Complexity**: O(n) for storing the radiant and dire queues.

### Keywords and Hints
- "Round-based procedure" hints at a sequential process, suggesting a queue-based approach
- "Ban one senator's right" implies removing elements from consideration
- "Senators who have lost their rights will be skipped" indicates we need to track who is still active
- "Play the best strategy" suggests a greedy approach - it's optimal for a senator to ban the closest opponent

### Key insights
1. Each senator wants to ban the closest opponent in order to maximize their party's power
2. Using queues to track the order of senators is efficient for this round-based process
3. We need to handle the circular nature of the rounds, as senators who aren't banned continue to vote
4. The optimal strategy is greedy - ban the next opponent in line

### Solution Intuition
The problem can be solved by simulating the process using two queues - one for Radiant senators and one for Dire senators. Each queue will store the indices of active senators. In each round, we take the first senator from each queue and let the one with the smaller index ban the other. The winning senator then goes back into their respective queue (with an adjusted index to represent the next round).

### Solution explanation
1. Initialize two queues, one for Radiant senators and one for Dire senators
2. Fill the queues with the indices of senators from the input string
3. While both queues are non-empty:
   - Pop a senator from each queue
   - The senator with the smaller index gets to ban the other
   - The winning senator is added back to their queue with an adjusted index
4. Return the victory of the party whose queue is non-empty

The key insight is adding n to the index when a senator is readded to the queue. This ensures proper ordering for future rounds, as it represents that this senator will vote again in the next round, after all the current senators have voted.

### Analogy
Think of this as a political debate where speakers are arranged in a circle. Each speaker, when it's their turn, can either silence the next opponent or declare victory if no opponents remain.

Imagine a circular room with senators from both parties. Each senator has a number tag showing their position in line. When it's a senator's turn, they point to the closest opponent and say "you're out!" That opponent must leave the room. Then we continue around the circle. If a senator has no opponents left when it's their turn, they win the debate for their party.

The queues in our algorithm represent the line of senators from each party waiting for their turn to speak. We always let the senator with the earlier position speak first, which mirrors the rule in the problem that senators act in the given order.
