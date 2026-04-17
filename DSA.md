# DSA CHAMP GUIDE
### The Ultimate Pattern Reference  — Java

> Master these 33 patterns, solve 100+ problems, become unstoppable.

---

# TABLE OF CONTENTS

1. [Constraint Decoder](#constraint-decoder)
2. [Problem-Solving Framework](#problem-solving-framework)
3. [Pattern Trigger Cheatsheet](#pattern-trigger-cheatsheet)
4. [Confusion Zones](#confusion-zones)
5. [P01-P09: Fundamental Patterns](#fundamental-patterns)
6. [P10-P19: Intermediate Patterns](#intermediate-patterns)
7. [P20-P33: Advanced Patterns](#advanced-patterns)
8. [Sorting Algorithm Selector](#sorting-algorithm-selector)
9. [Must-Do Problem Bank (100+)](#must-do-problem-bank)

---

# CONSTRAINT DECODER

> **Rule #1: Read constraints BEFORE writing code. Constraints ARE the hint.**

| Constraint | Target Complexity | Patterns to Consider |
|---|---|---|
| n ≤ 10 | O(n!) or O(2^n) | Backtracking, brute force permutations |
| n ≤ 20 | O(2^n) or O(2^n·n) | Bitmask DP, backtracking, TSP |
| n ≤ 100 | O(n³) | Floyd-Warshall, 3 nested loops |
| n ≤ 1,000 | O(n²) | 2 nested loops, DP with O(n²) states |
| n ≤ 100,000 | O(n log n) | Sorting, heap, BFS/DFS. **NO O(n²)!** |
| n ≤ 1,000,000 | O(n) or O(n log n) | Linear scan, hash map, two pointers, sliding window |
| n ≤ 10^9 | O(log n) | Binary search ONLY. Math formula. |

### Value Range Hints
| Constraint | Signal |
|---|---|
| values 1..n | Cyclic sort — place at index val-1 |
| sorted array | Two pointers, binary search |
| sum may overflow | Use `long` in Java! |
| "O(1) extra space" | In-place: cyclic sort, two pointers, Floyd's |

---

# PROBLEM-SOLVING FRAMEWORK

```
STEP 1: READ + EXAMPLES       (~3 min)
STEP 2: CONSTRAINTS            → Decode n → target complexity
STEP 3: BRUTE FORCE            → Say it aloud
STEP 4: PATTERN MATCH          → Which of 33 patterns?
STEP 5: OPTIMIZE               → BUD: Bottleneck, Unnecessary, Duplicated work
STEP 6: CODE + DRY RUN         → Write, then trace with small example
```

### Optimization Triggers
| From Brute Force... | Optimize To... |
|---|---|
| Repeated work? | Memoization / DP |
| Sorted input unused? | Binary Search or Two Pointers |
| O(n²) nested loops? | Sliding Window or HashMap |
| Repeatedly get min/max? | Heap |
| Repeated prefix queries? | Trie |
| Graph connectivity? | Union-Find |
| Range queries? | Prefix Sum or Segment Tree |

---

# PATTERN TRIGGER CHEATSHEET

| Trigger Keyword | Pattern |
|---|---|
| sorted array + pair/triplet + sum | Two Pointers (← →) |
| subarray/substring + longest/shortest/count | Sliding Window |
| fixed size k | Sliding Window (Fixed) |
| subarray + exact sum = k | Prefix Sum + HashMap |
| max subarray sum (pos & neg) | Kadane's Algorithm |
| cycle in linked list | Fast/Slow Pointers |
| overlapping intervals | Merge Intervals |
| values in range [1..n] | Cyclic Sort |
| search in sorted / answer in range | Binary Search |
| shortest path (unweighted) | BFS |
| shortest path (weighted) | Dijkstra |
| all possibilities/combinations/permutations | Backtracking |
| top K / K-th largest | Heap (size K) |
| next/previous greater/smaller | Monotonic Stack |
| connected components | Union-Find or BFS/DFS |
| prefix/word matching | Trie |
| dependency ordering | Topological Sort |
| count/max/min + overlapping subproblems | Dynamic Programming |
| local optimal = global optimal | Greedy |
| majority element | Boyer-Moore Voting |

---

# CONFUSION ZONES

### DP vs Greedy vs Backtracking
| Trigger | Pattern |
|---|---|
| Count / max / min / possible | DP |
| All answers / print all | Backtracking |
| Pick ONE best, never undo | Greedy |
| Pick/not pick + ALL answers | Backtracking |
| Pick/not pick + count/max/min | DP (Knapsack) |

### BFS vs DFS
| Trigger | Pattern |
|---|---|
| Shortest path / min steps | BFS |
| Level-by-level processing | BFS |
| All paths / exists a path | DFS |
| Path/depth/subtree/recursion | DFS |

### HashMap vs Trie
| Trigger | Pattern |
|---|---|
| Exact word lookup | HashMap |
| Prefix search / autocomplete | Trie |
| Multiple word search on grid | Trie + DFS |

---

# FUNDAMENTAL PATTERNS

---

## P01: Two Pointers
**Level:** Fundamental | **Mnemonic:** "Two runners on a track — one from each end, meet in the middle."

### What It Is
Two indices moving through a sorted data structure — either toward each other (opposite ends) or same direction. Eliminates O(n²) nested loops by making smart pointer movements based on comparisons.

### When to Use (Identification)
- Sorted array + pair/triplet + sum/target → Two Pointer (← →)
- Sorted + reverse / palindrome / compare both ends → Two Pointer (← →)
- Sorted + duplicate removal / in-place modify → Two Pointer (→ →)
- Two sorted arrays + merge/intersect → Two Pointer (→ →)
- **Brute Force Signal**: O(n²) nested loop on sorted input → Two Pointers

### Variations
1. **Opposite Direction (← →)**: Two Sum II, Container With Most Water, Valid Palindrome, 3Sum
2. **Same Direction (→ →)**: Remove Duplicates, Move Zeroes, merge two sorted arrays
3. **Three Pointers**: 3Sum (fix one, two-pointer on rest)
4. **Next Permutation**: Find pivot from right, swap, reverse suffix

### Java Template
```java
// Opposite direction
int left = 0, right = nums.length - 1;
while (left < right) {
    int sum = nums[left] + nums[right];
    if (sum == target) return new int[]{left, right};
    else if (sum < target) left++;
    else right--;
}

// Same direction (remove duplicates)
int slow = 0;
for (int fast = 1; fast < nums.length; fast++) {
    if (nums[fast] != nums[slow]) {
        nums[++slow] = nums[fast];
    }
}
```

### Complexity
| Approach | Time | Space |
|---|---|---|
| Brute force (nested loops) | O(n²) | O(1) |
| Two Pointers (opposite) | O(n) | O(1) |
| Two Pointers (3Sum) | O(n²) | O(1) |
| Sort + Two Pointers | O(n log n) | O(1) |

### Practice Problems — `src/Two_Pointers/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Pair with Target Sum | [Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) |
| 2 | Remove Duplicates | [Remove Duplicates](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) |
| 3 | Squaring a Sorted Array | [Squares of Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/) |
| 4 | Triplet Sum to Zero | [3Sum](https://leetcode.com/problems/3sum/) |
| 5 | Triplet Sum Close to Target | [3Sum Closest](https://leetcode.com/problems/3sum-closest/) |
| 6 | Triplets Smaller Sum | [3Sum Smaller](https://leetcode.com/problems/3sum-smaller/) |
| 7 | Subarrays Product < Target | [Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/) |
| 8 | Dutch National Flag | [Sort Colors](https://leetcode.com/problems/sort-colors/) |

### Must-Do Extra
- [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) — Easy
- [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) — Medium
- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) — Hard

---

## P02: Sliding Window
**Level:** Fundamental | **Mnemonic:** "A moving window on a train — slide the frame, what's outside doesn't matter."

### What It Is
Maintain a window (contiguous subarray/substring) that expands right and shrinks left. Fixed-size (window = k) or variable-size (grows/shrinks based on condition). Reduces O(n²) subarray checks to O(n).

### When to Use (Identification)
- Array/String + subarray/substring + longest/shortest/count/max/min → Sliding Window
- Fixed size k → Fixed Sliding Window
- Condition (≤k, ≥k, unique, distinct) → Variable Sliding Window
- **NOT Sliding Window**: max subarray sum with negatives → Kadane's; exact sum = k → Prefix Sum + HashMap

### Variations
1. **Fixed Window**: Max sum of subarray of size k
2. **Variable Window (shrink left)**: Smallest subarray with sum ≥ S
3. **Variable + HashMap**: Longest substring with at most K distinct chars
4. **Sliding Window + Heap**: Smallest range covering K lists

### Java Template
```java
// Variable-size window (longest substring no repeat)
Map<Character, Integer> map = new HashMap<>();
int left = 0, maxLen = 0;
for (int right = 0; right < s.length(); right++) {
    char c = s.charAt(right);
    if (map.containsKey(c)) left = Math.max(left, map.get(c) + 1);
    map.put(c, right);
    maxLen = Math.max(maxLen, right - left + 1);
}

// Fixed-size window
int windowSum = 0, maxSum = 0;
for (int i = 0; i < nums.length; i++) {
    windowSum += nums[i];
    if (i >= k) windowSum -= nums[i - k];
    if (i >= k - 1) maxSum = Math.max(maxSum, windowSum);
}
```

### Complexity
| Approach | Time | Space |
|---|---|---|
| Brute force (all subarrays) | O(n²) | O(1) |
| Fixed Sliding Window | O(n) | O(1) |
| Variable Window | O(n) | O(k) |
| Variable + HashMap | O(n) | O(min(n, charset)) |

### Practice Problems — `src/Sliding_Window/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Max Sum Subarray of Size K | [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/) |
| 2 | Smallest Subarray >= S | [Min Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/) |
| 3 | Longest Substring K Distinct | [Longest Substring K Distinct](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/) |
| 4 | Fruits into Baskets | [Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/) |
| 5 | No-Repeat Substring | [Longest Substring No Repeating](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |

### Must-Do Extra
- [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) — Hard
- [Permutation in String](https://leetcode.com/problems/permutation-in-string/) — Medium
- [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) — Medium
- [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) — Easy

---

## P03: Fast & Slow Pointers
**Level:** Fundamental | **Mnemonic:** "Tortoise & Hare — if there's a loop, the hare laps the tortoise."

### What It Is
Floyd's cycle detection: slow moves 1 step, fast moves 2. If cycle exists, they meet inside it. Also finds middle of linked list and detects happy numbers.

### When to Use
- Cycle detection / loop in linked list → Fast/Slow
- Find middle of linked list → Fast/Slow (fast at end → slow at middle)
- Happy Number detection → Fast/Slow on digit-square sequence
- Find duplicate number in [1..n] array → Floyd's on array

### Java Template
```java
// Detect cycle
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) return true;
}
return false;

// Find cycle start: after meeting, reset one to head, both move 1 step
// Find middle: when fast reaches end, slow is at middle
```

### Complexity: O(n) time, O(1) space for all variations

### Practice Problems — `src/Fast_And_Slow_Pointers/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | LinkedList Cycle | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) |
| 2 | Start of Cycle | [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) |
| 3 | Happy Number | [Happy Number](https://leetcode.com/problems/happy-number/) |
| 4 | Middle of LinkedList | [Middle of Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) |
| 5 | Palindrome LinkedList | [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) |
| 6 | Rearrange LinkedList | [Reorder List](https://leetcode.com/problems/reorder-list/) |

### Must-Do Extra
- [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) — Medium (Floyd's on array)

---

## P04: Merge Intervals
**Level:** Fundamental | **Mnemonic:** "Calendar conflicts — sort by start, merge if overlap."

### What It Is
Sort intervals by start time, then merge overlapping ones. If `current.start ≤ previous.end` → overlap.

### When to Use
- Overlapping intervals / merge ranges → Merge Intervals
- Insert interval into sorted list → Merge Intervals
- Meeting rooms / scheduling conflicts → Merge Intervals

### Java Template
```java
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
List<int[]> merged = new ArrayList<>();
for (int[] interval : intervals) {
    if (merged.isEmpty() || merged.get(merged.size() - 1)[1] < interval[0])
        merged.add(interval);
    else
        merged.get(merged.size() - 1)[1] =
            Math.max(merged.get(merged.size() - 1)[1], interval[1]);
}
```

### Complexity: O(n log n) time (sort), O(n) space

### Practice Problems — `src/Merge_Intervals/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Merge Intervals | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) |
| 2 | Insert Interval | [Insert Interval](https://leetcode.com/problems/insert-interval/) |
| 3 | Intervals Intersection | [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/) |
| 4 | Conflicting Appointments | [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) |
| 5 | Min Meeting Rooms | [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) |

### Must-Do Extra
- [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) — Medium

---

## P05: Cyclic Sort
**Level:** Fundamental | **Mnemonic:** "Every number has a home — place each at index (val-1)."

### What It Is
For arrays with numbers in range [1..n], place each number at its correct index in O(n) time, O(1) space. After placing, mismatched indices reveal missing/duplicate numbers.

### When to Use
- Values in range [1..n] or [0..n] → Cyclic Sort
- Find missing / duplicate / corrupt pair → Cyclic Sort
- **Constraint**: O(1) extra space + values in known range

### Java Template
```java
int i = 0;
while (i < nums.length) {
    int j = nums[i] - 1; // correct index for value
    if (nums[i] != nums[j]) swap(nums, i, j);
    else i++;
}
// After sort: scan for mismatches
```

### Complexity: O(n) time, O(1) space

### Practice Problems — `src/Cyclic_Sort/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Cyclic Sort | — |
| 2 | Find Missing Number | [Missing Number](https://leetcode.com/problems/missing-number/) |
| 3 | Find All Missing | [Find All Disappeared Numbers](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/) |
| 4 | Find Duplicate | [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) |
| 5 | Find All Duplicates | [Find All Duplicates](https://leetcode.com/problems/find-all-duplicates-in-an-array/) |
| 6 | Corrupt Pair | [Set Mismatch](https://leetcode.com/problems/set-mismatch/) |
| 7 | Smallest Missing Positive | [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) |

---

## P06: LinkedList In-Place Reversal
**Level:** Fundamental | **Mnemonic:** "Flip the arrows — prev/curr/next pointer dance."

### What It Is
Reverse linked list nodes in-place using three pointers: prev, curr, next. No extra space.

### Java Template
```java
ListNode prev = null, curr = head;
while (curr != null) {
    ListNode next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}
return prev; // new head
```

### Complexity: O(n) time, O(1) space

### Practice Problems — `src/LinkedList_In_Place_Traversal/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Reverse LinkedList | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) |
| 2 | Reverse Sub-List | [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/) |
| 3 | Reverse K-Group | [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) |

### Must-Do Extra
- [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) — Easy
- [Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) — Medium
- [LRU Cache](https://leetcode.com/problems/lru-cache/) — Medium (LinkedList + HashMap)

---

## P07: Stack
**Level:** Fundamental | **Mnemonic:** "Stack of plates — LIFO. Remember LAST SEEN to decide about CURRENT."

### What It Is
LIFO structure. Use when order matters and you need nearest/previous/next element context. Monotonic stack maintains sorted invariant for next greater/smaller problems.

### When to Use
- Next/Previous/Nearest + Greater/Smaller → **Monotonic Stack**
- Balanced parentheses / pair matching → Stack
- Expression evaluation (infix/postfix) → Stack
- Largest rectangle / histogram → Monotonic Stack
- **Brute Force Signal**: Two nested loops where inner depends on outer index → Stack

### Brute Force → Stack (from PDF)
- Inner loop `0 → i` → Next Smaller Element (Left)
- Inner loop `i → 0` → Next Greater Element (Left)
- Inner loop `i → n` → Next Greater Element (Right)
- Inner loop `n → i` → Next Smaller Element (Right)

### Java Template
```java
// Next Greater Element (Monotonic Stack)
Deque<Integer> stack = new ArrayDeque<>();
int[] res = new int[n];
Arrays.fill(res, -1);
for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && nums[stack.peek()] < nums[i])
        res[stack.pop()] = nums[i];
    stack.push(i);
}
```

### Complexity: O(n) time, O(n) space

### Practice Problems — `src/Stack/` and `src/Monotonic_Stack/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Valid Parentheses | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) |
| 2 | Min Stack | [Min Stack](https://leetcode.com/problems/min-stack/) |
| 3 | Eval Reverse Polish | [Evaluate Reverse Polish](https://leetcode.com/problems/evaluate-reverse-polish-notation/) |
| 4 | Daily Temperatures | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) |
| 5 | Next Greater Element I | [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/) |
| 6 | Largest Rect Histogram | [Largest Rectangle](https://leetcode.com/problems/largest-rectangle-in-histogram/) |
| 7 | Basic Calculator | [Basic Calculator](https://leetcode.com/problems/basic-calculator/) |

---

## P08: Island / Matrix Traversal
**Level:** Fundamental | **Mnemonic:** "Flood the island — DFS/BFS on a 2D grid, mark visited."

### What It Is
4-directional grid traversal using DFS or BFS. Visit cells, mark visited. Count connected components = count islands.

### Java Template
```java
int[][] dirs = {{0,1},{1,0},{0,-1},{-1,0}};
void dfs(int[][] grid, int r, int c) {
    if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length
        || grid[r][c] != 1) return;
    grid[r][c] = 0; // mark visited
    for (int[] d : dirs) dfs(grid, r + d[0], c + d[1]);
}
```

### Complexity: O(M×N) time, O(M×N) space (recursion stack / queue)

### Practice Problems — `src/Island_Matrix_Traversal/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Number of Islands | [Number of Islands](https://leetcode.com/problems/number-of-islands/) |
| 2 | Max Area of Island | [Max Area of Island](https://leetcode.com/problems/max-area-of-island/) |
| 3 | Flood Fill | [Flood Fill](https://leetcode.com/problems/flood-fill/) |
| 4 | Rotting Oranges | [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) |
| 5 | Pacific Atlantic | [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/) |
| 6 | Surrounded Regions | [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/) |

---

## P09: Prefix Sum
**Level:** Fundamental | **Mnemonic:** "Running total — pre-compute sums, get any range in O(1)."

### What It Is
Cumulative sums: `prefix[i] = prefix[i-1] + nums[i-1]`. Range sum = `prefix[r+1] - prefix[l]` in O(1). Combined with HashMap for "subarray sum = k" problems.

### When to Use
- Range sum query → Prefix Sum
- Subarray sum = k (with negatives) → Prefix Sum + HashMap
- Product of array except self → Prefix product

### Java Template
```java
// Subarray sum = k (HashMap approach)
Map<Integer, Integer> map = new HashMap<>();
map.put(0, 1);
int sum = 0, count = 0;
for (int num : nums) {
    sum += num;
    count += map.getOrDefault(sum - k, 0);
    map.put(sum, map.getOrDefault(sum, 0) + 1);
}
```

### Complexity: O(n) time, O(n) space

### Practice Problems — `src/Prefix_Sum/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Subarray Sum Equals K | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) |
| 2 | Product Except Self | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) |
| 3 | Range Sum Query | [Range Sum Query Immutable](https://leetcode.com/problems/range-sum-query-immutable/) |
| 4 | Contiguous Array | [Contiguous Array](https://leetcode.com/problems/contiguous-array/) |

---

# INTERMEDIATE PATTERNS

---

## P10: Hash Map / Hash Set
**Level:** Intermediate | **Mnemonic:** "Index cards — O(1) lookup, trade space for time."

### What It Is
HashMap/HashSet for O(1) lookups. Count frequencies, detect duplicates, store visited states, complement lookups (Two Sum pattern).

### When to Use
- Count/frequency/occurrences → HashMap
- Fast lookup "does X exist?" → HashMap/HashSet
- Duplicate/unique/distinct → HashSet
- Unsorted + pair/complement → HashMap (Two Sum style)
- Replace O(n²) nested loop → HashMap makes it O(n)
- Grouping problems → HashMap

### Java Constructs
- `HashMap<K,V>` — `put()`, `get()`, `containsKey()`, `getOrDefault()`, `entrySet()`
- `HashSet<E>` — `add()`, `contains()`, `remove()`
- `LinkedHashMap` — insertion-ordered (LRU cache)

### Practice Problems — `src/HashMap_HashTable/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Two Sum | [Two Sum](https://leetcode.com/problems/two-sum/) |
| 2 | Valid Anagram | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) |
| 3 | Group Anagrams | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) |
| 4 | Contains Duplicate | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) |
| 5 | Top K Frequent | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) |

### Must-Do Extra
- [Ransom Note](https://leetcode.com/problems/ransom-note/) — Easy
- [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) — Medium
- [LRU Cache](https://leetcode.com/problems/lru-cache/) — Medium

---

## P11: Tree BFS (Level Order)
**Level:** Intermediate | **Mnemonic:** "Wave by wave — process level by level using a queue."

### What It Is
Queue-based traversal. Process all nodes at current level before next. Lock level size: `int size = queue.size()` before inner loop.

### When to Use
- Level order traversal → BFS
- Zigzag / left side / right side view → BFS
- K distance from node / nearest → BFS
- Minimum depth → BFS (first leaf = answer)

### Java Template
```java
Queue<TreeNode> queue = new LinkedList<>();
queue.offer(root);
while (!queue.isEmpty()) {
    int size = queue.size(); // lock level size!
    for (int i = 0; i < size; i++) {
        TreeNode node = queue.poll();
        // process node
        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
    }
}
```

### Complexity: O(n) time, O(n) space (queue holds one level)

### Practice Problems — `src/Tree_Breadth_First_Search/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Level Order Traversal | [Binary Tree Level Order](https://leetcode.com/problems/binary-tree-level-order-traversal/) |
| 2 | Reverse Level Order | [Binary Tree Level Order II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/) |
| 3 | Zigzag Traversal | [Zigzag Level Order](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/) |
| 4 | Level Averages | [Average of Levels](https://leetcode.com/problems/average-of-levels-in-binary-tree/) |
| 5 | Right Side View | [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) |

---

## P12: Tree DFS (Recursive)
**Level:** Intermediate | **Mnemonic:** "Go deep first — preorder/inorder/postorder, pass state down/up."

### What It Is
Recursive tree traversal. Pass extra state (min/max for BST, path sum, depth). Return values bubble up. Global variable pattern for diameter/max path sum.

### When to Use
- Height/depth/diameter → DFS
- Root to leaf path / path sum → DFS
- Subtree match / subtree sum → DFS
- LCA / ancestor tracking → DFS + parent tracking
- Inorder/preorder/postorder → DFS

### Java Template
```java
// Max depth
int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}

// Diameter (global max pattern)
int maxDiam = 0;
int dfs(TreeNode node) {
    if (node == null) return 0;
    int l = dfs(node.left), r = dfs(node.right);
    maxDiam = Math.max(maxDiam, l + r);
    return 1 + Math.max(l, r);
}
```

### Complexity: O(n) time, O(h) space (h = tree height, worst case n)

### Practice Problems — `src/Tree_Depth_First_Search/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Path Sum | [Path Sum](https://leetcode.com/problems/path-sum/) |
| 2 | All Paths for a Sum | [Path Sum II](https://leetcode.com/problems/path-sum-ii/) |
| 3 | Max Path Sum | [Binary Tree Max Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |

### Must-Do Extra
- [Maximum Depth](https://leetcode.com/problems/maximum-depth-of-binary-tree/) — Easy
- [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) — Easy
- [Same Tree](https://leetcode.com/problems/same-tree/) — Easy
- [Validate BST](https://leetcode.com/problems/validate-binary-search-tree/) — Medium
- [Lowest Common Ancestor](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) — Medium
- [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) — Easy
- [Kth Smallest in BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) — Medium
- [Construct from Preorder + Inorder](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) — Medium
- [Serialize/Deserialize Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) — Hard

---

## P13: Graph (BFS/DFS)
**Level:** Intermediate | **Mnemonic:** "Web of connections — adjacency list, visited set, explore."

### What It Is
Graph traversal using BFS (shortest path, min steps) or DFS (all paths, cycle detection). Build adjacency list, track visited.

### When to Use
- Connected components / count reachable → BFS/DFS
- Shortest path (unweighted) → BFS
- All paths / exists a path → DFS
- Cycle detection (undirected) → DFS / Union Find
- Cycle detection (directed) → DFS coloring / Topological Sort
- Island counting / flood fill → DFS on grid

### Java Template
```java
// BFS on graph
Map<Integer, List<Integer>> adj = new HashMap<>();
Queue<Integer> queue = new LinkedList<>();
Set<Integer> visited = new HashSet<>();
queue.offer(start); visited.add(start);
while (!queue.isEmpty()) {
    int node = queue.poll();
    for (int nb : adj.getOrDefault(node, List.of())) {
        if (!visited.contains(nb)) {
            visited.add(nb);
            queue.offer(nb);
        }
    }
}
```

### Complexity: O(V+E) time, O(V+E) space

### Practice Problems — `src/Graph/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Clone Graph | [Clone Graph](https://leetcode.com/problems/clone-graph/) |
| 2 | Course Schedule | [Course Schedule](https://leetcode.com/problems/course-schedule/) |
| 3 | Number of Islands | [Number of Islands](https://leetcode.com/problems/number-of-islands/) |

### Must-Do Extra
- [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) — Medium
- [Word Ladder](https://leetcode.com/problems/word-ladder/) — Hard
- [Accounts Merge](https://leetcode.com/problems/accounts-merge/) — Medium

---

## P14: Heap / Priority Queue
**Level:** Intermediate | **Mnemonic:** "Always know the peak — instant access to min/max."

### What It Is
Binary heap for O(log n) insert/extract. Use for "top K" or "best element" problems. NOT for full sorting.

### When to Use
- Top K / Kth element → Heap (size K)
- Best element repeatedly (min/max) → Heap
- Priority / scheduling → Heap
- Stream + maintain top elements → Heap
- Merge K sorted lists → Min Heap
- Median of stream → Two Heaps (Min + Max)

### Java Template
```java
// Min Heap (default)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
// Max Heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

// Top K pattern: keep heap of size K
for (int num : nums) {
    minHeap.offer(num);
    if (minHeap.size() > k) minHeap.poll();
}
```

### Complexity: O(n log k) for Top K, O(log n) per insert/poll

### Practice Problems — `src/Heap/` and `src/Top_K_Elements/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Kth Largest Element | [Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array/) |
| 2 | Top K Frequent | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) |
| 3 | K Closest Points | [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) |
| 4 | Find Median Stream | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) |
| 5 | Merge K Sorted Lists | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) |
| 6 | Task Scheduler | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) |

---

## P15: Modified Binary Search
**Level:** Intermediate | **Mnemonic:** "Dictionary search — open to middle, throw away half."

### What It Is
Not just sorted arrays. "Can I binary search on the answer?" Works whenever search space is monotonic.

### When to Use
- Sorted array + target → Binary Search
- Answer in numeric range + isValid() → Binary Search on Answer
- Rotated sorted array / peak → Binary Search
- First/last occurrence → Binary Search with boundary check

### Java Template
```java
// Standard — never get it wrong
int lo = 0, hi = n - 1;
while (lo <= hi) {
    int mid = lo + (hi - lo) / 2;
    if (nums[mid] == target) return mid;
    else if (nums[mid] < target) lo = mid + 1;
    else hi = mid - 1;
}

// Binary Search on Answer
int lo = minAns, hi = maxAns;
while (lo < hi) {
    int mid = lo + (hi - lo) / 2;
    if (feasible(mid)) hi = mid;
    else lo = mid + 1;
}
return lo;
```

### Complexity: O(log n) time, O(1) space

### Practice Problems — `src/Modified_Binary_Search/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Binary Search | [Binary Search](https://leetcode.com/problems/binary-search/) |
| 2 | Search Rotated Array | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) |
| 3 | Find Min Rotated | [Find Min in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) |
| 4 | Koko Eating Bananas | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) |
| 5 | First Bad Version | [First Bad Version](https://leetcode.com/problems/first-bad-version/) |

### Must-Do Extra
- [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) — Hard
- [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/) — Medium
- [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) — Medium

---

## P16: Top K Elements
**Level:** Intermediate — See P14 (Heap). Same pattern: min-heap of size K.

---

## P17: K-Way Merge
**Level:** Intermediate | **Mnemonic:** "Merge K sorted streams — min-heap picks smallest from K fronts."

### What It Is
Merge K sorted lists using a min-heap. Pick smallest among K current fronts, advance that list's pointer.

### Java Template
```java
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
// Add first element from each list: {value, listIdx, elemIdx}
while (!pq.isEmpty()) {
    int[] curr = pq.poll();
    result.add(curr[0]);
    if (curr[2] + 1 < lists[curr[1]].length)
        pq.offer(new int[]{lists[curr[1]][curr[2]+1], curr[1], curr[2]+1});
}
```

### Complexity: O(N log K) time where N = total elements, K = lists

### Practice Problems — `src/K_Way_Merge/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Merge K Sorted Lists | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) |
| 2 | Kth Smallest in M Arrays | [Kth Smallest Element in Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) |
| 3 | Smallest Range K Lists | [Smallest Range Covering K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/) |

---

## P18: Greedy Algorithm
**Level:** Intermediate | **Mnemonic:** "Grab the best NOW — make locally optimal choice, trust it."

### What It Is
Make locally best choice at each step, never revisit. Key: if greedy choice never needs to be undone → Greedy over DP. Usually requires sorting first.

### When to Use
- Activity selection / interval scheduling → Greedy
- "Max events/tasks you can attend" → Greedy
- Always pick largest/smallest first → Greedy
- Job scheduling with deadlines → Greedy
- **Rule**: DP tries ALL choices. Greedy picks ONE. If no undo needed → Greedy.

### Practice Problems — `src/Greedy_Algorithm/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Jump Game | [Jump Game](https://leetcode.com/problems/jump-game/) |
| 2 | Jump Game II | [Jump Game II](https://leetcode.com/problems/jump-game-ii/) |
| 3 | Maximum Subarray | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) |
| 4 | Gas Station | [Gas Station](https://leetcode.com/problems/gas-station/) |
| 5 | Hand of Straights | [Hand of Straights](https://leetcode.com/problems/hand-of-straights/) |

### Must-Do Extra
- [Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/) — Hard
- [Partition Labels](https://leetcode.com/problems/partition-labels/) — Medium

---

## P19: Concurrency
**Level:** Intermediate — Niche pattern. See `src/Concurrency/` for problems. Focus on P01-P18 first.

---

# ADVANCED PATTERNS

---

## P20: Monotonic Stack
**Level:** Advanced | See P07 (Stack) for templates.

Monotonic Stack maintains a sorted invariant. Used for "next greater/smaller element" problems in O(n).

### Brute Force → Stack (from PDF)
- Inner loop `0 → i` → Next Smaller Element (Left)
- Inner loop `i → 0` → Next Greater Element (Left)
- Inner loop `i → n` → Next Greater Element (Right)
- Inner loop `n → i` → Next Smaller Element (Right)

### Must-Do
- [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/) — Easy
- [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) — Medium
- [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) — Hard
- [Online Stock Span](https://leetcode.com/problems/online-stock-span/) — Medium
- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) — Hard (stack approach)

---

## P21: Subsets / Backtracking
**Level:** Advanced | **Mnemonic:** "Decision tree — choose → explore → unchoose. Every path = one answer."

### What It Is
Explore all possibilities via recursion. At each step: make a choice, recurse, undo choice (backtrack). Generates all subsets, permutations, combinations.

### When to Use
- All subsets / permutations / combinations → Backtracking
- Pick/not pick + need ALL answers → Backtracking
- Constraint + validation (N-Queens, Sudoku) → Backtracking
- **NOT Backtracking**: count/max/min only → DP

### Java Template
```java
// Subsets
void backtrack(List<List<Integer>> res, List<Integer> curr, int[] nums, int start) {
    res.add(new ArrayList<>(curr));
    for (int i = start; i < nums.length; i++) {
        curr.add(nums[i]);              // choose
        backtrack(res, curr, nums, i + 1); // explore
        curr.remove(curr.size() - 1);   // unchoose
    }
}

// Permutations: swap-based or use boolean[] visited
// Duplicates: sort + skip if nums[i] == nums[i-1] && !used[i-1]
```

### Complexity: O(2^n) for subsets, O(n!) for permutations

### Practice Problems — `src/Subsets/` and `src/Backtracking/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Subsets | [Subsets](https://leetcode.com/problems/subsets/) |
| 2 | Subsets II | [Subsets II](https://leetcode.com/problems/subsets-ii/) |
| 3 | Permutations | [Permutations](https://leetcode.com/problems/permutations/) |
| 4 | Combination Sum | [Combination Sum](https://leetcode.com/problems/combination-sum/) |
| 5 | Word Search | [Word Search](https://leetcode.com/problems/word-search/) |
| 6 | Letter Combinations | [Letter Combinations](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) |
| 7 | N-Queens | [N-Queens](https://leetcode.com/problems/n-queens/) |
| 8 | Sudoku Solver | [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/) |

---

## P22: Bitwise XOR
**Level:** Advanced | **Mnemonic:** "XOR magic — same ^ same = 0, anything ^ 0 = anything."

### What It Is
XOR properties: a ^ a = 0, a ^ 0 = a, commutative, associative. Find unique elements, count bits, check powers of 2.

### Java Template
```java
// Single Number (XOR all → pairs cancel out)
int res = 0;
for (int n : nums) res ^= n;
return res;

// Check power of 2
boolean isPow2 = n > 0 && (n & (n - 1)) == 0;

// Count set bits
int count = 0;
while (n != 0) { count++; n &= (n - 1); }
```

### Practice Problems — `src/Bitwise_XOR/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Single Number | [Single Number](https://leetcode.com/problems/single-number/) |
| 2 | Two Single Numbers | [Single Number III](https://leetcode.com/problems/single-number-iii/) |
| 3 | Complement of Base 10 | [Complement of Base 10](https://leetcode.com/problems/complement-of-base-10-integer/) |
| 4 | Flip and Invert Image | [Flipping an Image](https://leetcode.com/problems/flipping-an-image/) |

### Must-Do Extra
- [Counting Bits](https://leetcode.com/problems/counting-bits/) — Easy
- [Reverse Bits](https://leetcode.com/problems/reverse-bits/) — Easy
- [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) — Easy
- [Missing Number](https://leetcode.com/problems/missing-number/) — Easy (XOR approach)
- [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/) — Medium

---

## P23: Dynamic Programming — 1D (Linear)
**Level:** Advanced | **Mnemonic:** "Textbook chapters — chapter 5 builds on 1-4. Can't skip."

### What It Is
Break problem into overlapping subproblems. `dp[i]` = answer for first i elements. Fill bottom-up. Ask: "What is dp[i] in terms of dp[i-1]?"

### When to Use
- "How many ways" / "is it possible" / "minimum cost" → DP
- Overlapping subproblems + optimal substructure → DP
- Climbing stairs / fibonacci → Linear DP
- House robber → Linear DP
- Longest increasing subsequence → Linear DP

### DP Identification Decision Tree (from PDF)
```
Can problem be defined by a STATE?
├── NO → Not DP
└── YES → Same state solved multiple times?
    ├── NO → Recursion / Divide & Conquer
    └── YES → DP ✅ → What defines the state?
        ├── Single index (i) → Linear DP
        ├── (index, capacity) → Knapsack DP
        ├── Range (left, right) → Interval DP
        ├── Coordinates (row, col) → Grid DP
        ├── Tree node → Tree DP
        ├── Bitmask → Bitmask DP
        └── Two sequences (i, j) → String DP
```

### Java Template
```java
// Climbing Stairs
int[] dp = new int[n + 1];
dp[0] = 1; dp[1] = 1;
for (int i = 2; i <= n; i++)
    dp[i] = dp[i - 1] + dp[i - 2];

// Space optimized
int prev2 = 1, prev1 = 1;
for (int i = 2; i <= n; i++) {
    int curr = prev1 + prev2;
    prev2 = prev1; prev1 = curr;
}

// Top-Down with Memoization
Map<Integer, Integer> memo = new HashMap<>();
int solve(int n) {
    if (memo.containsKey(n)) return memo.get(n);
    if (n <= 1) return n;
    int res = solve(n-1) + solve(n-2);
    memo.put(n, res);
    return res;
}
```

### Complexity: O(n) time, O(n) or O(1) space (optimized)

### Practice Problems — `src/Dynamic_Programming/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Climbing Stairs | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) |
| 2 | House Robber | [House Robber](https://leetcode.com/problems/house-robber/) |
| 3 | House Robber II | [House Robber II](https://leetcode.com/problems/house-robber-ii/) |
| 4 | Decode Ways | [Decode Ways](https://leetcode.com/problems/decode-ways/) |
| 5 | Word Break | [Word Break](https://leetcode.com/problems/word-break/) |
| 6 | LIS | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) |
| 7 | Coin Change | [Coin Change](https://leetcode.com/problems/coin-change/) |

---

## P24: Dynamic Programming — 2D (Grid / String)
**Level:** Advanced

### What It Is
`dp[i][j]` = answer for subproblem of size i×j. LCS, edit distance, unique paths all use this.

### Java Template
```java
// Longest Common Subsequence
int[][] dp = new int[m + 1][n + 1];
for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        if (text1.charAt(i-1) == text2.charAt(j-1))
            dp[i][j] = dp[i-1][j-1] + 1;
        else
            dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
    }
}

// Unique Paths (grid DP)
int[][] dp = new int[m][n];
Arrays.fill(dp[0], 1);
for (int i = 0; i < m; i++) dp[i][0] = 1;
for (int i = 1; i < m; i++)
    for (int j = 1; j < n; j++)
        dp[i][j] = dp[i-1][j] + dp[i][j-1];
```

### Complexity: O(m×n) time and space, can sometimes optimize space to O(n)

### Must-Do
- [Unique Paths](https://leetcode.com/problems/unique-paths/) — Medium
- [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) — Medium
- [Edit Distance](https://leetcode.com/problems/edit-distance/) — Medium
- [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) — Medium
- [Maximal Square](https://leetcode.com/problems/maximal-square/) — Medium
- [Interleaving String](https://leetcode.com/problems/interleaving-string/) — Medium

---

## P25: Knapsack DP
**Level:** Advanced

### What It Is
Pick/not-pick with capacity constraint. 0/1 Knapsack: each item once. Unbounded: items reused.

### When to Use (from PDF)
- Pick/not pick + capacity/budget → Knapsack
- "Minimum cost to reach target" → Knapsack
- "Can we make sum = k?" → Knapsack
- Coin change / unbounded knapsack → Knapsack

### Java Template
```java
// 0/1 Knapsack (iterate capacity BACKWARDS)
for (int i = 0; i < n; i++)
    for (int w = W; w >= wt[i]; w--)
        dp[w] = Math.max(dp[w], dp[w - wt[i]] + val[i]);

// Unbounded Knapsack (iterate FORWARDS)
for (int i = 0; i < n; i++)
    for (int w = wt[i]; w <= W; w++)
        dp[w] = Math.max(dp[w], dp[w - wt[i]] + val[i]);
```

### Must-Do
- [Coin Change](https://leetcode.com/problems/coin-change/) — Medium
- [Coin Change II](https://leetcode.com/problems/coin-change-ii/) — Medium
- [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) — Medium
- [Target Sum](https://leetcode.com/problems/target-sum/) — Medium

---

## P26: Trie
**Level:** Advanced | **Mnemonic:** "Prefix tree — each path spells a word."

### What It Is
Tree of characters. Each path from root = a word. O(L) insert/search where L = word length.

### When to Use (from PDF)
- Prefix-based search/matching → Trie
- Autocomplete / suggestions → Trie
- Search multiple words on grid → Trie + DFS
- "Starts with" / "count words with prefix" → Trie
- **NOT Trie**: exact lookup only → HashMap

### Java Template
```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEnd;
}

void insert(String word) {
    TrieNode node = root;
    for (char c : word.toCharArray()) {
        if (node.children[c - 'a'] == null)
            node.children[c - 'a'] = new TrieNode();
        node = node.children[c - 'a'];
    }
    node.isEnd = true;
}

boolean search(String word) {
    TrieNode node = root;
    for (char c : word.toCharArray()) {
        if (node.children[c - 'a'] == null) return false;
        node = node.children[c - 'a'];
    }
    return node.isEnd;
}
```

### Practice Problems — `src/Trie/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Implement Trie | [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/) |
| 2 | Word Search II | [Word Search II](https://leetcode.com/problems/word-search-ii/) |
| 3 | Design Add & Search | [Design Add and Search Words](https://leetcode.com/problems/design-add-and-search-words-data-structure/) |

---

## P27: Topological Sort
**Level:** Advanced | **Mnemonic:** "Get dressed — can't wear shoes before socks. Process in-degree 0 first."

### What It Is
Order nodes in a DAG so every edge goes from earlier to later. Use Kahn's algorithm (BFS + in-degree). If not all nodes processed → cycle exists.

### When to Use
- Prerequisites / task scheduling → Topological Sort
- DAG + ordering → Topological Sort
- Cycle detection in directed graph → Topological Sort

### Java Template (Kahn's Algorithm)
```java
int[] inDegree = new int[n];
Map<Integer, List<Integer>> adj = new HashMap<>();
// Build graph and compute in-degrees...
Queue<Integer> queue = new LinkedList<>();
for (int i = 0; i < n; i++)
    if (inDegree[i] == 0) queue.offer(i);
int count = 0;
List<Integer> order = new ArrayList<>();
while (!queue.isEmpty()) {
    int u = queue.poll();
    order.add(u);
    count++;
    for (int v : adj.getOrDefault(u, List.of()))
        if (--inDegree[v] == 0) queue.offer(v);
}
return count == n; // false = cycle exists
```

### Complexity: O(V+E) time and space

### Practice Problems — `src/Topological_Sort/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Course Schedule | [Course Schedule](https://leetcode.com/problems/course-schedule/) |
| 2 | Course Schedule II | [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) |
| 3 | Alien Dictionary | [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/) |
| 4 | Min Height Trees | [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/) |

---

## P28: Union Find
**Level:** Advanced | **Mnemonic:** "Family boss — everyone has one boss. Merge families by connecting bosses."

### What It Is
Track connected components. `find()` gets root (path compression), `union()` merges sets (union by rank). Nearly O(1) amortized.

### When to Use
- Dynamically connecting components → Union Find
- "Are X and Y connected?" → Union Find
- Redundant connection / MST → Union Find

### Java Template
```java
int[] parent, rank;
void init(int n) {
    parent = new int[n]; rank = new int[n];
    for (int i = 0; i < n; i++) parent[i] = i;
}
int find(int x) {
    if (parent[x] != x) parent[x] = find(parent[x]); // path compression
    return parent[x];
}
void union(int a, int b) {
    int ra = find(a), rb = find(b);
    if (ra == rb) return;
    if (rank[ra] > rank[rb]) parent[rb] = ra;
    else if (rank[ra] < rank[rb]) parent[ra] = rb;
    else { parent[rb] = ra; rank[ra]++; }
}
```

### Complexity: O(α(n)) per operation ≈ O(1) amortized

### Practice Problems — `src/Union_Find/`
| # | Problem | LeetCode |
|---|---|---|
| 1 | Number of Islands (UF) | [Number of Islands](https://leetcode.com/problems/number-of-islands/) |
| 2 | Accounts Merge | [Accounts Merge](https://leetcode.com/problems/accounts-merge/) |
| 3 | Redundant Connection | [Redundant Connection](https://leetcode.com/problems/redundant-connection/) |
| 4 | Graph Valid Tree | [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/) |

---

## P29: Ordered Set
**Level:** Advanced — Uses `TreeMap`/`TreeSet` for O(log n) ordered operations.

### Java Constructs
- `TreeMap<K,V>` — `floorKey()`, `ceilingKey()`, `firstKey()`, `lastKey()`
- `TreeSet<E>` — `floor()`, `ceiling()`, `first()`, `last()`

### Must-Do
- [Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/) — Hard
- [My Calendar I](https://leetcode.com/problems/my-calendar-i/) — Medium

---

## P30: Dijkstra / Weighted Graph
**Level:** Advanced | **Mnemonic:** "GPS navigation — always expand the cheapest unvisited node."

### What It Is
Shortest path with non-negative weighted edges. Min-heap based. Never revisit settled nodes.

### Java Template
```java
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
int[] dist = new int[n];
Arrays.fill(dist, Integer.MAX_VALUE);
dist[src] = 0;
pq.offer(new int[]{src, 0});
while (!pq.isEmpty()) {
    int[] curr = pq.poll();
    if (curr[1] > dist[curr[0]]) continue; // stale
    for (int[] edge : adj.get(curr[0])) {
        int newDist = curr[1] + edge[1];
        if (newDist < dist[edge[0]]) {
            dist[edge[0]] = newDist;
            pq.offer(new int[]{edge[0], newDist});
        }
    }
}
```

### Complexity: O((V+E) log V) time

### Must-Do
- [Network Delay Time](https://leetcode.com/problems/network-delay-time/) — Medium
- [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/) — Medium
- [Cheapest Flights K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) — Medium
- [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/) — Hard

---

## P31: Divide & Conquer
**Level:** Advanced

### What It Is
Split into halves → solve each independently → merge results. Recursion on independent parts (no overlap → NOT DP).

### When to Use
- Independent subproblems (no overlap) → D&C
- Sorting (merge sort, quick sort) → D&C
- Count inversions → Merge Sort D&C

### Must-Do
- [Sort an Array](https://leetcode.com/problems/sort-an-array/) — Medium (Merge Sort)
- [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) — Hard

---

## P32: Interval Scheduling
**Level:** Advanced

### What It Is
Sort by end time. Greedy: pick interval ending earliest → maximizes count of non-overlapping intervals.

### Must-Do
- [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) — Medium
- [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) — Medium
- [Min Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) — Medium

---

## P33: Boyer-Moore Voting
**Level:** Advanced | **Mnemonic:** "Election — count up for candidate, down for others."

### What It Is
Find majority element (> n/2) in O(n) time O(1) space.

### Java Template
```java
int candidate = 0, count = 0;
for (int n : nums) {
    if (count == 0) candidate = n;
    count += (n == candidate) ? 1 : -1;
}
return candidate;
```

### Must-Do
- [Majority Element](https://leetcode.com/problems/majority-element/) — Easy
- [Majority Element II](https://leetcode.com/problems/majority-element-ii/) — Medium

---

# SORTING ALGORITHM SELECTOR

| Algorithm | Use When | Time | Stable? |
|---|---|---|---|
| Insertion Sort | Almost sorted / n ≤ 20 | O(n²) | Yes |
| Merge Sort | Stable + linked list + inversions | O(n log n) | Yes |
| Quick Sort | General purpose, in-place | O(n log n) avg | No |
| Heap Sort | In-place + worst case guarantee | O(n log n) | No |
| Counting Sort | Small integer range (0..k) | O(n+k) | Yes |
| Radix Sort | Fixed digit numbers | O(d·(n+k)) | Yes |
| Bucket Sort | Uniform float distribution | O(n) avg | Yes |

### Quick Decision
- **Small / almost sorted** → Insertion Sort
- **General purpose** → Quick Sort (`Arrays.sort()` uses dual-pivot quicksort for primitives)
- **Stable / linked list / inversions** → Merge Sort
- **Integer range known** → Counting / Radix Sort
- **Worst case matters** → Merge Sort or Heap Sort

> **Java specifics**: `Arrays.sort(int[])` = dual-pivot quicksort. `Arrays.sort(Object[])` = TimSort (stable). `Collections.sort()` = TimSort.

---

# MUST-DO PROBLEM BANK

> 100+ problems organized by difficulty. Solve at least 1 from each pattern, then grind the rest.

### EASY (20 problems — warm up)
| # | Problem | Pattern | Link |
|---|---|---|---|
| 1 | Two Sum | HashMap | [Link](https://leetcode.com/problems/two-sum/) |
| 2 | Valid Parentheses | Stack | [Link](https://leetcode.com/problems/valid-parentheses/) |
| 3 | Merge Two Sorted Lists | LinkedList | [Link](https://leetcode.com/problems/merge-two-sorted-lists/) |
| 4 | Best Time Buy/Sell Stock | Sliding Window | [Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) |
| 5 | Valid Palindrome | Two Pointers | [Link](https://leetcode.com/problems/valid-palindrome/) |
| 6 | Invert Binary Tree | Tree DFS | [Link](https://leetcode.com/problems/invert-binary-tree/) |
| 7 | Valid Anagram | HashMap | [Link](https://leetcode.com/problems/valid-anagram/) |
| 8 | Binary Search | Binary Search | [Link](https://leetcode.com/problems/binary-search/) |
| 9 | Flood Fill | Grid DFS | [Link](https://leetcode.com/problems/flood-fill/) |
| 10 | Linked List Cycle | Fast/Slow | [Link](https://leetcode.com/problems/linked-list-cycle/) |
| 11 | Climbing Stairs | DP 1D | [Link](https://leetcode.com/problems/climbing-stairs/) |
| 12 | Reverse Linked List | LinkedList | [Link](https://leetcode.com/problems/reverse-linked-list/) |
| 13 | Max Depth Binary Tree | Tree DFS | [Link](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |
| 14 | Contains Duplicate | HashSet | [Link](https://leetcode.com/problems/contains-duplicate/) |
| 15 | Maximum Subarray | Kadane's | [Link](https://leetcode.com/problems/maximum-subarray/) |
| 16 | Missing Number | Cyclic Sort/XOR | [Link](https://leetcode.com/problems/missing-number/) |
| 17 | Single Number | XOR | [Link](https://leetcode.com/problems/single-number/) |
| 18 | Majority Element | Boyer-Moore | [Link](https://leetcode.com/problems/majority-element/) |
| 19 | Diameter of Binary Tree | Tree DFS | [Link](https://leetcode.com/problems/diameter-of-binary-tree/) |
| 20 | Middle of Linked List | Fast/Slow | [Link](https://leetcode.com/problems/middle-of-the-linked-list/) |

### MEDIUM (60 problems — the core grind)
| # | Problem | Pattern | Link |
|---|---|---|---|
| 21 | 3Sum | Two Pointers | [Link](https://leetcode.com/problems/3sum/) |
| 22 | Container With Most Water | Two Pointers | [Link](https://leetcode.com/problems/container-with-most-water/) |
| 23 | Longest Substring No Repeat | Sliding Window | [Link](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |
| 24 | Min Window Substring | Sliding Window | [Link](https://leetcode.com/problems/minimum-window-substring/) |
| 25 | Find All Anagrams | Sliding Window | [Link](https://leetcode.com/problems/find-all-anagrams-in-a-string/) |
| 26 | Sort Colors | Two Pointers | [Link](https://leetcode.com/problems/sort-colors/) |
| 27 | Group Anagrams | HashMap | [Link](https://leetcode.com/problems/group-anagrams/) |
| 28 | Product of Array Except Self | Prefix Sum | [Link](https://leetcode.com/problems/product-of-array-except-self/) |
| 29 | Top K Frequent Elements | Heap | [Link](https://leetcode.com/problems/top-k-frequent-elements/) |
| 30 | Merge Intervals | Intervals | [Link](https://leetcode.com/problems/merge-intervals/) |
| 31 | Insert Interval | Intervals | [Link](https://leetcode.com/problems/insert-interval/) |
| 32 | Non-overlapping Intervals | Greedy | [Link](https://leetcode.com/problems/non-overlapping-intervals/) |
| 33 | Search Rotated Sorted Array | Binary Search | [Link](https://leetcode.com/problems/search-in-rotated-sorted-array/) |
| 34 | Find Min Rotated Array | Binary Search | [Link](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) |
| 35 | Koko Eating Bananas | BS on Answer | [Link](https://leetcode.com/problems/koko-eating-bananas/) |
| 36 | Validate BST | Tree DFS | [Link](https://leetcode.com/problems/validate-binary-search-tree/) |
| 37 | Level Order Traversal | Tree BFS | [Link](https://leetcode.com/problems/binary-tree-level-order-traversal/) |
| 38 | Right Side View | Tree BFS | [Link](https://leetcode.com/problems/binary-tree-right-side-view/) |
| 39 | Lowest Common Ancestor | Tree DFS | [Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) |
| 40 | Kth Smallest in BST | Tree DFS | [Link](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) |
| 41 | Construct from Pre+In | Tree DFS | [Link](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) |
| 42 | Number of Islands | Grid DFS | [Link](https://leetcode.com/problems/number-of-islands/) |
| 43 | Clone Graph | Graph BFS | [Link](https://leetcode.com/problems/clone-graph/) |
| 44 | Course Schedule | Topo Sort | [Link](https://leetcode.com/problems/course-schedule/) |
| 45 | Course Schedule II | Topo Sort | [Link](https://leetcode.com/problems/course-schedule-ii/) |
| 46 | Pacific Atlantic | Grid DFS | [Link](https://leetcode.com/problems/pacific-atlantic-water-flow/) |
| 47 | Rotting Oranges | Multi-BFS | [Link](https://leetcode.com/problems/rotting-oranges/) |
| 48 | Accounts Merge | Union Find | [Link](https://leetcode.com/problems/accounts-merge/) |
| 49 | Kth Largest Element | Heap | [Link](https://leetcode.com/problems/kth-largest-element-in-an-array/) |
| 50 | K Closest Points | Heap | [Link](https://leetcode.com/problems/k-closest-points-to-origin/) |
| 51 | Task Scheduler | Heap+Greedy | [Link](https://leetcode.com/problems/task-scheduler/) |
| 52 | Implement Trie | Trie | [Link](https://leetcode.com/problems/implement-trie-prefix-tree/) |
| 53 | Subsets | Backtracking | [Link](https://leetcode.com/problems/subsets/) |
| 54 | Permutations | Backtracking | [Link](https://leetcode.com/problems/permutations/) |
| 55 | Combination Sum | Backtracking | [Link](https://leetcode.com/problems/combination-sum/) |
| 56 | Letter Combinations | Backtracking | [Link](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) |
| 57 | Word Search | Backtracking | [Link](https://leetcode.com/problems/word-search/) |
| 58 | House Robber | DP 1D | [Link](https://leetcode.com/problems/house-robber/) |
| 59 | House Robber II | DP 1D | [Link](https://leetcode.com/problems/house-robber-ii/) |
| 60 | Coin Change | DP Knapsack | [Link](https://leetcode.com/problems/coin-change/) |
| 61 | Word Break | DP 1D | [Link](https://leetcode.com/problems/word-break/) |
| 62 | Decode Ways | DP 1D | [Link](https://leetcode.com/problems/decode-ways/) |
| 63 | Unique Paths | DP 2D | [Link](https://leetcode.com/problems/unique-paths/) |
| 64 | Longest Common Subseq | DP 2D | [Link](https://leetcode.com/problems/longest-common-subsequence/) |
| 65 | LIS | DP 1D | [Link](https://leetcode.com/problems/longest-increasing-subsequence/) |
| 66 | Longest Palindromic Sub | DP 2D | [Link](https://leetcode.com/problems/longest-palindromic-substring/) |
| 67 | Partition Equal Subset | DP Knapsack | [Link](https://leetcode.com/problems/partition-equal-subset-sum/) |
| 68 | Target Sum | DP Knapsack | [Link](https://leetcode.com/problems/target-sum/) |
| 69 | Edit Distance | DP 2D | [Link](https://leetcode.com/problems/edit-distance/) |
| 70 | Jump Game | Greedy | [Link](https://leetcode.com/problems/jump-game/) |
| 71 | Jump Game II | Greedy | [Link](https://leetcode.com/problems/jump-game-ii/) |
| 72 | Gas Station | Greedy | [Link](https://leetcode.com/problems/gas-station/) |
| 73 | Partition Labels | Greedy | [Link](https://leetcode.com/problems/partition-labels/) |
| 74 | Daily Temperatures | Monotonic Stack | [Link](https://leetcode.com/problems/daily-temperatures/) |
| 75 | Min Stack | Stack | [Link](https://leetcode.com/problems/min-stack/) |
| 76 | Subarray Sum = K | Prefix Sum | [Link](https://leetcode.com/problems/subarray-sum-equals-k/) |
| 77 | Network Delay Time | Dijkstra | [Link](https://leetcode.com/problems/network-delay-time/) |
| 78 | Cheapest Flights K Stops | Dijkstra | [Link](https://leetcode.com/problems/cheapest-flights-within-k-stops/) |
| 79 | LRU Cache | LinkedList+Map | [Link](https://leetcode.com/problems/lru-cache/) |
| 80 | Spiral Matrix | Array | [Link](https://leetcode.com/problems/spiral-matrix/) |

### HARD (20 problems — interview differentiators)
| # | Problem | Pattern | Link |
|---|---|---|---|
| 81 | Trapping Rain Water | Two Pointers/Stack | [Link](https://leetcode.com/problems/trapping-rain-water/) |
| 82 | Median of Two Sorted Arrays | Binary Search | [Link](https://leetcode.com/problems/median-of-two-sorted-arrays/) |
| 83 | Merge K Sorted Lists | K-Way Merge | [Link](https://leetcode.com/problems/merge-k-sorted-lists/) |
| 84 | Largest Rect Histogram | Monotonic Stack | [Link](https://leetcode.com/problems/largest-rectangle-in-histogram/) |
| 85 | Binary Tree Max Path Sum | Tree DFS | [Link](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |
| 86 | Serialize/Deserialize Tree | Tree BFS | [Link](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) |
| 87 | Word Ladder | Graph BFS | [Link](https://leetcode.com/problems/word-ladder/) |
| 88 | Alien Dictionary | Topo Sort | [Link](https://leetcode.com/problems/alien-dictionary/) |
| 89 | Basic Calculator | Stack | [Link](https://leetcode.com/problems/basic-calculator/) |
| 90 | Find Median Data Stream | Two Heaps | [Link](https://leetcode.com/problems/find-median-from-data-stream/) |
| 91 | Word Search II | Trie+DFS | [Link](https://leetcode.com/problems/word-search-ii/) |
| 92 | N-Queens | Backtracking | [Link](https://leetcode.com/problems/n-queens/) |
| 93 | Sudoku Solver | Backtracking | [Link](https://leetcode.com/problems/sudoku-solver/) |
| 94 | First Missing Positive | Cyclic Sort | [Link](https://leetcode.com/problems/first-missing-positive/) |
| 95 | Max Profit Job Schedule | DP+BS | [Link](https://leetcode.com/problems/maximum-profit-in-job-scheduling/) |
| 96 | Reverse Nodes K-Group | LinkedList | [Link](https://leetcode.com/problems/reverse-nodes-in-k-group/) |
| 97 | Swim in Rising Water | Dijkstra | [Link](https://leetcode.com/problems/swim-in-rising-water/) |
| 98 | Burst Balloons | Interval DP | [Link](https://leetcode.com/problems/burst-balloons/) |
| 99 | Longest Valid Parens | Stack/DP | [Link](https://leetcode.com/problems/longest-valid-parentheses/) |
| 100 | Count Smaller After Self | D&C/BIT | [Link](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) |

---

> **You made it to the end. Now go solve 100 problems and become the DSA champ.** 🏆

