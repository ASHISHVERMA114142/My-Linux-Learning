# Dynamic Programming - Complete End-to-End Guide

## Table of Contents
1. [Introduction & Theory](#introduction--theory)
2. [The DP Thinking Framework](#the-dp-thinking-framework)
3. [Top-Down vs Bottom-Up](#top-down-vs-bottom-up)
4. [State Design & Transitions](#state-design--transitions)
5. [Space Optimization Techniques](#space-optimization-techniques)
6. [DP Pattern Categories](#dp-pattern-categories)
   - [Pattern 1: Linear DP (1D)](#pattern-1-linear-dp-1d)
   - [Pattern 2: Grid/2D DP](#pattern-2-grid2d-dp)
   - [Pattern 3: 0/1 Knapsack Family](#pattern-3-01-knapsack-family)
   - [Pattern 4: Unbounded Knapsack](#pattern-4-unbounded-knapsack)
   - [Pattern 5: Longest Common Subsequence (LCS)](#pattern-5-longest-common-subsequence-lcs)
   - [Pattern 6: Longest Increasing Subsequence (LIS)](#pattern-6-longest-increasing-subsequence-lis)
   - [Pattern 7: Palindrome DP](#pattern-7-palindrome-dp)
   - [Pattern 8: Interval / Range DP](#pattern-8-interval--range-dp)
   - [Pattern 9: Partition DP (MCM Pattern)](#pattern-9-partition-dp-mcm-pattern)
   - [Pattern 10: String DP](#pattern-10-string-dp)
   - [Pattern 11: Tree DP](#pattern-11-tree-dp)
   - [Pattern 12: DP on Subsets / Bitmask DP](#pattern-12-dp-on-subsets--bitmask-dp)
   - [Pattern 13: Digit DP](#pattern-13-digit-dp)
   - [Pattern 14: DP with State Machines](#pattern-14-dp-with-state-machines)
   - [Pattern 15: Game Theory DP](#pattern-15-game-theory-dp)
   - [Pattern 16: Counting DP](#pattern-16-counting-dp)
   - [Pattern 17: DP on Graphs (DAG)](#pattern-17-dp-on-graphs-dag)
   - [Pattern 18: Probability / Expected Value DP](#pattern-18-probability--expected-value-dp)
7. [How to Identify Which DP Pattern to Use](#how-to-identify-which-dp-pattern-to-use)
8. [Curated Problem List (100+ problems)](#curated-problem-list)
9. [Common Pitfalls & Debugging Tips](#common-pitfalls--debugging-tips)
10. [Study Plan (4-Week Roadmap)](#study-plan)

---

## Introduction & Theory

### What is Dynamic Programming?

Dynamic Programming (DP) is an algorithmic paradigm that solves complex problems by breaking them into **overlapping subproblems** and storing their results to avoid redundant computation.

### Two Key Properties

A problem can be solved with DP if and only if it has:

**1. Optimal Substructure**
The optimal solution to the problem can be constructed from optimal solutions of its subproblems.
```
optimal(problem) = combine(optimal(subproblem1), optimal(subproblem2), ...)
```

**2. Overlapping Subproblems**
The same subproblems are solved multiple times during recursion.
```
                    fib(5)
                  /        \
             fib(4)         fib(3)     ← fib(3) computed twice!
            /     \         /    \
         fib(3)  fib(2)  fib(2) fib(1) ← fib(2) computed thrice!
         /   \
      fib(2) fib(1)
```

### DP vs Other Paradigms

| Paradigm | Subproblems Overlap? | Makes Greedy Choice? | Explores All? |
|---|---|---|---|
| **Dynamic Programming** | Yes | No | Yes |
| **Divide & Conquer** | No | No | Yes |
| **Greedy** | N/A | Yes | No |
| **Backtracking** | Possible | No | Yes (prune) |

### When to Think DP?

Ask yourself these questions:
1. **Can I break this into smaller subproblems?**
2. **Are the subproblems overlapping?** (Same computation repeated)
3. **Does the problem ask for optimization?** (min, max, count, existence)
4. **Does the problem have choices at each step?** (take/skip, include/exclude)

**Keyword triggers in problem statements:**
- "Find the minimum/maximum..."
- "Count the number of ways..."
- "Is it possible to..."
- "Find the longest/shortest..."
- "What is the minimum cost to..."

---

## The DP Thinking Framework

### Step-by-Step Approach (STAR Method)

```
S - State:     What information do I need to uniquely describe a subproblem?
T - Transition: How does a bigger problem relate to smaller subproblems?
A - Answer:     Where is the final answer stored?
R - Reduction:  Can I reduce space complexity?
```

### Detailed Breakdown

**Step 1: Define the State (Most Critical)**
- What changes between subproblems?
- What variables do I need to describe a subproblem?
- Common state variables: index, remaining capacity, last chosen element, flags

**Step 2: Write the Recurrence Relation**
- Express `dp[state]` in terms of smaller states
- Consider all possible choices/transitions at each state

**Step 3: Identify Base Cases**
- What are the smallest/simplest subproblems?
- What happens when index = 0, capacity = 0, string is empty?

**Step 4: Determine Computation Order**
- Bottom-up: fill smaller states first
- Ensure all dependent states are computed before the current state

**Step 5: Extract the Answer**
- Is it `dp[n]`, `dp[n][W]`, `max(dp[i])`, or something else?

**Step 6: Optimize Space (if needed)**
- Can you use rolling array? 
- Does current state only depend on previous row/state?

### Visual: The DP Problem-Solving Pipeline

```
Problem Statement
       │
       ▼
┌─────────────────┐
│ Identify choices │  ← What decision at each step?
│ at each step     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Define STATE     │  ← What params uniquely describe subproblem?
│ dp[i], dp[i][j] │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Write RECURRENCE │  ← dp[i] = f(dp[i-1], dp[i-2], ...)
│ relation         │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Set BASE CASES   │  ← dp[0] = ?, dp[1] = ?
└────────┬────────┘
         │
         ▼
┌────────────────────┐
│ Choose approach:   │
│ Top-Down (memo)    │
│ OR Bottom-Up (tab) │
└────────┬───────────┘
         │
         ▼
┌─────────────────┐
│ Optimize SPACE   │  ← Rolling array, 1D from 2D, etc.
└─────────────────┘
```

---

## Top-Down vs Bottom-Up

### Top-Down (Memoization)

**Approach:** Recursive + cache results. Start from the original problem and break down.

```java
// Template: Top-Down DP
Map<String, Integer> memo = new HashMap<>();

int solve(int state1, int state2) {
    // Base case
    if (/* terminal condition */) return /* base value */;

    // Check memo
    String key = state1 + "," + state2;
    if (memo.containsKey(key)) return memo.get(key);

    // Recurrence: try all choices
    int result = /* combine subproblem solutions */;

    memo.put(key, result);
    return result;
}
```

**Pros:**
- Easier to think about (natural recursion)
- Only computes states that are actually needed
- Good when state space is sparse

**Cons:**
- Recursion overhead (stack frames)
- Risk of stack overflow for deep recursion
- Slightly slower due to hash map lookups

### Bottom-Up (Tabulation)

**Approach:** Iterative. Fill a table starting from base cases.

```java
// Template: Bottom-Up DP
int solve(int n) {
    int[] dp = new int[n + 1];

    // Base cases
    dp[0] = /* base value */;

    // Fill table
    for (int i = 1; i <= n; i++) {
        dp[i] = /* recurrence using dp[i-1], dp[i-2], ... */;
    }

    return dp[n];
}
```

**Pros:**
- No recursion overhead
- No stack overflow risk
- Easier to optimize space
- Generally faster (array access vs hash map)

**Cons:**
- May compute unnecessary states
- Harder to think about computation order for complex states
- Need to be careful about iteration direction

### When to Use Which?

| Scenario | Preferred |
|---|---|
| Sparse state space (few states actually visited) | Top-Down |
| Need all states computed | Bottom-Up |
| Complex state with many dimensions | Top-Down (easier to code) |
| Need space optimization | Bottom-Up |
| Problem has clear left-to-right ordering | Bottom-Up |
| Tree/graph DP | Top-Down |

---

## State Design & Transitions

### Common State Patterns

```
dp[i]          → Answer considering elements 0..i
dp[i][j]       → Answer considering elements i..j (interval)
dp[i][j]       → Answer using first i items with capacity/budget j
dp[i][j]       → Answer matching first i chars of s1 with first j chars of s2
dp[mask]        → Answer considering subset represented by bitmask
dp[i][k]       → Answer at position i with k groups/transactions
dp[i][0/1]     → Answer at position i with binary state (hold/not hold)
dp[i][j][k]    → 3D state (rare, try to reduce)
```

### Transition Types

**1. Take or Skip (Most Common)**
```
dp[i] = max(dp[i-1],                    // skip item i
            dp[i-1] + value[i])          // take item i
```

**2. Try All Previous**
```
dp[i] = max(dp[j] + cost) for all valid j < i
```

**3. Try All Splits**
```
dp[i][j] = min(dp[i][k] + dp[k+1][j] + cost) for k in [i, j-1]
```

**4. Include/Exclude with Capacity**
```
dp[i][w] = max(dp[i-1][w],                       // exclude
               dp[i-1][w-wt[i]] + val[i])         // include (if w >= wt[i])
```

**5. Match / Don't Match (String DP)**
```
if s1[i] == s2[j]:
    dp[i][j] = dp[i-1][j-1] + 1
else:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

---

## Space Optimization Techniques

### Technique 1: Rolling Array (2 rows → constant rows)

When `dp[i]` only depends on `dp[i-1]`:

```java
// Before: O(n * W) space
int[][] dp = new int[n + 1][W + 1];
for (int i = 1; i <= n; i++)
    for (int w = 0; w <= W; w++)
        dp[i][w] = Math.max(dp[i-1][w], dp[i-1][w - wt[i]] + val[i]);

// After: O(W) space using single 1D array (iterate W backwards!)
int[] dp = new int[W + 1];
for (int i = 1; i <= n; i++)
    for (int w = W; w >= wt[i]; w--)   // ← BACKWARDS for 0/1 knapsack
        dp[w] = Math.max(dp[w], dp[w - wt[i]] + val[i]);
```

### Technique 2: Two Variables (when only dp[i-1] and dp[i-2] needed)

```java
// Before
int[] dp = new int[n + 1];
dp[1] = 1; dp[2] = 1;
for (int i = 3; i <= n; i++) dp[i] = dp[i-1] + dp[i-2];

// After
int prev2 = 1, prev1 = 1;
for (int i = 3; i <= n; i++) {
    int curr = prev1 + prev2;
    prev2 = prev1;
    prev1 = curr;
}
```

### Technique 3: 2D → 1D (LCS style)

```java
// When dp[i][j] depends on dp[i-1][j], dp[i][j-1], dp[i-1][j-1]
int[] dp = new int[m + 1];
for (int i = 1; i <= n; i++) {
    int prev = 0; // stores dp[i-1][j-1]
    for (int j = 1; j <= m; j++) {
        int temp = dp[j]; // save dp[i-1][j] before overwrite
        if (s1.charAt(i-1) == s2.charAt(j-1))
            dp[j] = prev + 1;
        else
            dp[j] = Math.max(dp[j], dp[j-1]);
        prev = temp;
    }
}
```

---

## DP Pattern Categories

---

### Pattern 1: Linear DP (1D)

**State:** `dp[i]` = answer considering elements `0..i` (or ending at index `i`)

**Key Idea:** Each state depends on a fixed number of previous states.

**Template:**
```java
// Type A: dp[i] depends on dp[i-1], dp[i-2], etc.
int[] dp = new int[n];
dp[0] = /* base */;
for (int i = 1; i < n; i++) {
    dp[i] = /* f(dp[i-1], dp[i-2], ..., nums[i]) */;
}
return dp[n - 1]; // or max(dp)

// Type B: dp[i] depends on best of all dp[j] where j < i
int[] dp = new int[n];
Arrays.fill(dp, /* base */);
for (int i = 1; i < n; i++) {
    for (int j = 0; j < i; j++) {
        if (/* condition(j, i) */) {
            dp[i] = Math.max(dp[i], dp[j] + /* transition cost */);
        }
    }
}
return Arrays.stream(dp).max().getAsInt();
```

**Example: House Robber**
```java
// You can't rob adjacent houses. Maximize total money.
// State: dp[i] = max money robbing from houses 0..i
// Recurrence: dp[i] = max(dp[i-1], dp[i-2] + nums[i])

public int rob(int[] nums) {
    if (nums.length == 1) return nums[0];
    int prev2 = 0, prev1 = nums[0];
    for (int i = 1; i < nums.length; i++) {
        int curr = Math.max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Climbing Stairs | Easy | [LC 70](https://leetcode.com/problems/climbing-stairs/) |
| 2 | House Robber | Medium | [LC 198](https://leetcode.com/problems/house-robber/) |
| 3 | House Robber II | Medium | [LC 213](https://leetcode.com/problems/house-robber-ii/) |
| 4 | Maximum Subarray (Kadane's) | Medium | [LC 53](https://leetcode.com/problems/maximum-subarray/) |
| 5 | Maximum Product Subarray | Medium | [LC 152](https://leetcode.com/problems/maximum-product-subarray/) |
| 6 | Decode Ways | Medium | [LC 91](https://leetcode.com/problems/decode-ways/) |
| 7 | Jump Game | Medium | [LC 55](https://leetcode.com/problems/jump-game/) |
| 8 | Jump Game II | Medium | [LC 45](https://leetcode.com/problems/jump-game-ii/) |
| 9 | Min Cost Climbing Stairs | Easy | [LC 746](https://leetcode.com/problems/min-cost-climbing-stairs/) |
| 10 | Delete and Earn | Medium | [LC 740](https://leetcode.com/problems/delete-and-earn/) |
| 11 | Trapping Rain Water | Hard | [LC 42](https://leetcode.com/problems/trapping-rain-water/) |

---

### Pattern 2: Grid/2D DP

**State:** `dp[i][j]` = answer to reach/process cell `(i, j)`

**Key Idea:** Move through a 2D grid, typically right or down. Each cell depends on its neighbors.

**Template:**
```java
int[][] dp = new int[m][n];
dp[0][0] = grid[0][0]; // or 1 for path counting

// Fill first row and first column
for (int j = 1; j < n; j++) dp[0][j] = dp[0][j-1] + grid[0][j];
for (int i = 1; i < m; i++) dp[i][0] = dp[i-1][0] + grid[i][0];

// Fill rest
for (int i = 1; i < m; i++) {
    for (int j = 1; j < n; j++) {
        dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
        // For counting paths: dp[i][j] = dp[i-1][j] + dp[i][j-1];
    }
}
return dp[m-1][n-1];
```

**Example: Unique Paths**
```java
// Count paths from top-left to bottom-right (only right or down moves)
public int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];
    Arrays.fill(dp[0], 1);
    for (int i = 0; i < m; i++) dp[i][0] = 1;

    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[i][j] = dp[i-1][j] + dp[i][j-1];

    return dp[m-1][n-1];
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Unique Paths | Medium | [LC 62](https://leetcode.com/problems/unique-paths/) |
| 2 | Unique Paths II (obstacles) | Medium | [LC 63](https://leetcode.com/problems/unique-paths-ii/) |
| 3 | Minimum Path Sum | Medium | [LC 64](https://leetcode.com/problems/minimum-path-sum/) |
| 4 | Triangle | Medium | [LC 120](https://leetcode.com/problems/triangle/) |
| 5 | Maximal Square | Medium | [LC 221](https://leetcode.com/problems/maximal-square/) |
| 6 | Dungeon Game | Hard | [LC 174](https://leetcode.com/problems/dungeon-game/) |
| 7 | Cherry Pickup | Hard | [LC 741](https://leetcode.com/problems/cherry-pickup/) |
| 8 | Cherry Pickup II | Hard | [LC 1463](https://leetcode.com/problems/cherry-pickup-ii/) |
| 9 | Minimum Falling Path Sum | Medium | [LC 931](https://leetcode.com/problems/minimum-falling-path-sum/) |

---

### Pattern 3: 0/1 Knapsack Family

**State:** `dp[i][w]` = best value using first `i` items with capacity `w`

**Key Idea:** For each item, you have exactly two choices: **take it** or **leave it** (0/1).

**Template:**
```java
// Full 2D version
int[][] dp = new int[n + 1][W + 1];
for (int i = 1; i <= n; i++) {
    for (int w = 0; w <= W; w++) {
        dp[i][w] = dp[i-1][w]; // don't take item i
        if (w >= weight[i-1]) {
            dp[i][w] = Math.max(dp[i][w],
                                dp[i-1][w - weight[i-1]] + value[i-1]); // take
        }
    }
}
return dp[n][W];

// Space-optimized 1D version (iterate capacity BACKWARDS)
int[] dp = new int[W + 1];
for (int i = 0; i < n; i++) {
    for (int w = W; w >= weight[i]; w--) {
        dp[w] = Math.max(dp[w], dp[w - weight[i]] + value[i]);
    }
}
return dp[W];
```

**Why iterate backwards in 1D?**
```
Forward iteration: dp[w - weight[i]] might already be updated in this round
                   → effectively allows using item i multiple times (unbounded)
Backward iteration: dp[w - weight[i]] is still from previous round
                    → each item used at most once (0/1)
```

**Example: Subset Sum**
```java
// Can we find a subset that sums to target?
public boolean canPartition(int[] nums, int target) {
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;
    for (int num : nums) {
        for (int j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }
    return dp[target];
}
```

**The 0/1 Knapsack Family Tree:**
```
                    0/1 Knapsack
                    /     |      \
           Subset Sum   Equal    Target Sum
              |        Partition
         Count of       |
        Subsets with  Minimum
         Given Sum    Subset Sum
                      Difference
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Partition Equal Subset Sum | Medium | [LC 416](https://leetcode.com/problems/partition-equal-subset-sum/) |
| 2 | Target Sum | Medium | [LC 494](https://leetcode.com/problems/target-sum/) |
| 3 | Last Stone Weight II | Medium | [LC 1049](https://leetcode.com/problems/last-stone-weight-ii/) |
| 4 | Ones and Zeroes | Medium | [LC 474](https://leetcode.com/problems/ones-and-zeroes/) |
| 5 | Profitable Schemes | Hard | [LC 879](https://leetcode.com/problems/profitable-schemes/) |
| 6 | 0/1 Knapsack (Classic) | Medium | [GFG](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1) |
| 7 | Count Subsets with Sum K | Medium | [GFG](https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1) |
| 8 | Minimum Subset Sum Difference | Medium | [GFG](https://www.geeksforgeeks.org/problems/minimum-sum-partition3317/1) |

---

### Pattern 4: Unbounded Knapsack

**State:** Same as 0/1 knapsack, but each item can be used **unlimited** times.

**Key Difference:** Use `dp[i][w - weight[i]]` (same row) instead of `dp[i-1][w - weight[i]]`.

**Template:**
```java
// 1D version (iterate capacity FORWARDS — allows reuse)
int[] dp = new int[W + 1];
for (int i = 0; i < n; i++) {
    for (int w = weight[i]; w <= W; w++) { // ← FORWARDS for unbounded
        dp[w] = Math.max(dp[w], dp[w - weight[i]] + value[i]);
    }
}
return dp[W];
```

**Example: Coin Change (Minimum coins)**
```java
// Find minimum coins to make amount
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);
    dp[0] = 0;
    for (int coin : coins) {
        for (int a = coin; a <= amount; a++) {
            dp[a] = Math.min(dp[a], dp[a - coin] + 1);
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```

**Example: Coin Change II (Count ways)**
```java
// Count number of combinations to make amount
public int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;
    for (int coin : coins) {          // outer loop: coins (for combinations)
        for (int a = coin; a <= amount; a++) {
            dp[a] += dp[a - coin];
        }
    }
    return dp[amount];
}
// NOTE: If you swap the loops (amount outer, coins inner) → counts PERMUTATIONS
```

**Combinations vs Permutations (Critical Difference):**
```
Combinations (order doesn't matter): coins outer loop, amount inner
  → {1,2} and {2,1} are counted as ONE way

Permutations (order matters): amount outer loop, coins inner
  → {1,2} and {2,1} are counted as TWO ways
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Coin Change | Medium | [LC 322](https://leetcode.com/problems/coin-change/) |
| 2 | Coin Change II | Medium | [LC 518](https://leetcode.com/problems/coin-change-ii/) |
| 3 | Combination Sum IV | Medium | [LC 377](https://leetcode.com/problems/combination-sum-iv/) |
| 4 | Perfect Squares | Medium | [LC 279](https://leetcode.com/problems/perfect-squares/) |
| 5 | Integer Break | Medium | [LC 343](https://leetcode.com/problems/integer-break/) |
| 6 | Rod Cutting | Medium | [GFG](https://www.geeksforgeeks.org/problems/rod-cutting0840/1) |
| 7 | Unbounded Knapsack | Medium | [GFG](https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1) |

---

### Pattern 5: Longest Common Subsequence (LCS)

**State:** `dp[i][j]` = LCS length of `s1[0..i-1]` and `s2[0..j-1]`

**Key Idea:** Compare characters at current positions. If they match, extend the LCS. Otherwise, try skipping from either string.

**Template:**
```java
int[][] dp = new int[m + 1][n + 1];
for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        if (s1.charAt(i-1) == s2.charAt(j-1)) {
            dp[i][j] = dp[i-1][j-1] + 1;
        } else {
            dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
        }
    }
}
return dp[m][n];
```

**Printing the actual LCS:**
```java
StringBuilder sb = new StringBuilder();
int i = m, j = n;
while (i > 0 && j > 0) {
    if (s1.charAt(i-1) == s2.charAt(j-1)) {
        sb.append(s1.charAt(i-1));
        i--; j--;
    } else if (dp[i-1][j] > dp[i][j-1]) {
        i--;
    } else {
        j--;
    }
}
return sb.reverse().toString();
```

**LCS Family Tree:**
```
                        LCS
                   /     |     \
    Longest Common    Shortest    Edit
     Substring        Common      Distance
                    Supersequence
                         |
                   Min Insertions/
                   Deletions to
                   Convert String
```

**Variations:**
```
Longest Common Substring:    dp[i][j] = dp[i-1][j-1] + 1  (match)
                             dp[i][j] = 0                   (no match)
                             answer = max of all dp[i][j]

Shortest Common Superseq:   len(s1) + len(s2) - LCS(s1, s2)

Min operations to convert:  len(s1) + len(s2) - 2 * LCS(s1, s2)
  (only insert + delete)

Min insertions for palindrome:  len(s) - LCS(s, reverse(s))
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Longest Common Subsequence | Medium | [LC 1143](https://leetcode.com/problems/longest-common-subsequence/) |
| 2 | Delete Operation for Two Strings | Medium | [LC 583](https://leetcode.com/problems/delete-operation-for-two-strings/) |
| 3 | Shortest Common Supersequence | Hard | [LC 1092](https://leetcode.com/problems/shortest-common-supersequence/) |
| 4 | Max Dot Product of Two Subsequences | Hard | [LC 1458](https://leetcode.com/problems/max-dot-product-of-two-subsequences/) |
| 5 | Longest Common Substring | Medium | [GFG](https://www.geeksforgeeks.org/problems/longest-common-substring1452/1) |
| 6 | Min Insertions to Make Palindrome | Hard | [LC 1312](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/) |
| 7 | Interleaving String | Medium | [LC 97](https://leetcode.com/problems/interleaving-string/) |
| 8 | Distinct Subsequences | Hard | [LC 115](https://leetcode.com/problems/distinct-subsequences/) |

---

### Pattern 6: Longest Increasing Subsequence (LIS)

**State:** `dp[i]` = length of LIS ending at index `i`

**Key Idea:** For each element, check all previous elements. If a previous element is smaller, we can extend that subsequence.

**Template (O(n²)):**
```java
int[] dp = new int[n];
Arrays.fill(dp, 1); // every element is a subsequence of length 1
int maxLen = 1;

for (int i = 1; i < n; i++) {
    for (int j = 0; j < i; j++) {
        if (nums[j] < nums[i]) {
            dp[i] = Math.max(dp[i], dp[j] + 1);
        }
    }
    maxLen = Math.max(maxLen, dp[i]);
}
return maxLen;
```

**Template (O(n log n) using patience sorting / binary search):**
```java
// tails[i] = smallest tail element for increasing subsequence of length i+1
public int lengthOfLIS(int[] nums) {
    List<Integer> tails = new ArrayList<>();
    for (int num : nums) {
        int pos = Collections.binarySearch(tails, num);
        if (pos < 0) pos = -(pos + 1);
        if (pos == tails.size()) {
            tails.add(num);
        } else {
            tails.set(pos, num);
        }
    }
    return tails.size();
}
```

**Visual: O(n log n) LIS (Patience Sorting)**
```
nums = [10, 9, 2, 5, 3, 7, 101, 18]

Process 10:  tails = [10]
Process 9:   tails = [9]         (replace 10)
Process 2:   tails = [2]         (replace 9)
Process 5:   tails = [2, 5]      (extend)
Process 3:   tails = [2, 3]      (replace 5)
Process 7:   tails = [2, 3, 7]   (extend)
Process 101: tails = [2, 3, 7, 101] (extend)
Process 18:  tails = [2, 3, 7, 18]  (replace 101)

LIS length = 4
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Longest Increasing Subsequence | Medium | [LC 300](https://leetcode.com/problems/longest-increasing-subsequence/) |
| 2 | Number of LIS | Medium | [LC 673](https://leetcode.com/problems/number-of-longest-increasing-subsequence/) |
| 3 | Russian Doll Envelopes | Hard | [LC 354](https://leetcode.com/problems/russian-doll-envelopes/) |
| 4 | Longest String Chain | Medium | [LC 1048](https://leetcode.com/problems/longest-string-chain/) |
| 5 | Largest Divisible Subset | Medium | [LC 368](https://leetcode.com/problems/largest-divisible-subset/) |
| 6 | Maximum Length of Pair Chain | Medium | [LC 646](https://leetcode.com/problems/maximum-length-of-pair-chain/) |
| 7 | Longest Bitonic Subsequence | Medium | [GFG](https://www.geeksforgeeks.org/problems/longest-bitonic-subsequence0824/1) |

---

### Pattern 7: Palindrome DP

**State:** `dp[i][j]` = whether `s[i..j]` is a palindrome, or property of substring `s[i..j]`

**Key Idea:** A string is palindrome if outer chars match AND inner substring is palindrome.

**Template:**
```java
// Check all palindromic substrings
boolean[][] dp = new boolean[n][n];

// Base: single characters
for (int i = 0; i < n; i++) dp[i][i] = true;

// Base: two character substrings
for (int i = 0; i < n - 1; i++)
    dp[i][i+1] = (s.charAt(i) == s.charAt(i+1));

// Fill for length 3 to n (iterate by length)
for (int len = 3; len <= n; len++) {
    for (int i = 0; i <= n - len; i++) {
        int j = i + len - 1;
        dp[i][j] = (s.charAt(i) == s.charAt(j)) && dp[i+1][j-1];
    }
}
```

**Example: Longest Palindromic Substring**
```java
public String longestPalindrome(String s) {
    int n = s.length();
    boolean[][] dp = new boolean[n][n];
    int start = 0, maxLen = 1;

    for (int i = 0; i < n; i++) dp[i][i] = true;

    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = (len == 2) || dp[i+1][j-1];
                if (dp[i][j] && len > maxLen) {
                    start = i;
                    maxLen = len;
                }
            }
        }
    }
    return s.substring(start, start + maxLen);
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Longest Palindromic Substring | Medium | [LC 5](https://leetcode.com/problems/longest-palindromic-substring/) |
| 2 | Longest Palindromic Subsequence | Medium | [LC 516](https://leetcode.com/problems/longest-palindromic-subsequence/) |
| 3 | Palindrome Partitioning II | Hard | [LC 132](https://leetcode.com/problems/palindrome-partitioning-ii/) |
| 4 | Palindromic Substrings (Count) | Medium | [LC 647](https://leetcode.com/problems/palindromic-substrings/) |
| 5 | Min Insertions for Palindrome | Hard | [LC 1312](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/) |
| 6 | Longest Palindrome by Concatenating | Medium | [LC 2131](https://leetcode.com/problems/longest-palindrome-by-concatenating-two-letter-words/) |

---

### Pattern 8: Interval / Range DP

**State:** `dp[i][j]` = answer for the subarray/substring from index `i` to `j`

**Key Idea:** Try all possible ways to split interval `[i, j]` into two parts.

**Template (iterate by length):**
```java
// Fill diagonally: length 1, then 2, then 3, ...
for (int len = 1; len <= n; len++) {
    for (int i = 0; i + len - 1 < n; i++) {
        int j = i + len - 1;
        if (len == 1) {
            dp[i][j] = /* base case */;
        } else {
            dp[i][j] = /* initial value (INF or 0) */;
            for (int k = i; k < j; k++) { // try all split points
                dp[i][j] = Math.min(dp[i][j],
                    dp[i][k] + dp[k+1][j] + /* merge cost */);
            }
        }
    }
}
return dp[0][n-1];
```

**Example: Burst Balloons**
```java
public int maxCoins(int[] nums) {
    int n = nums.length;
    int[] arr = new int[n + 2];
    arr[0] = arr[n + 1] = 1;
    for (int i = 0; i < n; i++) arr[i + 1] = nums[i];
    n += 2;

    int[][] dp = new int[n][n];
    for (int len = 2; len < n; len++) {
        for (int i = 0; i + len < n; i++) {
            int j = i + len;
            for (int k = i + 1; k < j; k++) {
                dp[i][j] = Math.max(dp[i][j],
                    dp[i][k] + dp[k][j] + arr[i] * arr[k] * arr[j]);
            }
        }
    }
    return dp[0][n - 1];
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Burst Balloons | Hard | [LC 312](https://leetcode.com/problems/burst-balloons/) |
| 2 | Minimum Cost to Merge Stones | Hard | [LC 1000](https://leetcode.com/problems/minimum-cost-to-merge-stones/) |
| 3 | Strange Printer | Hard | [LC 664](https://leetcode.com/problems/strange-printer/) |
| 4 | Minimum Score Triangulation | Medium | [LC 1039](https://leetcode.com/problems/minimum-score-triangulation-of-polygon/) |
| 5 | Stone Game | Medium | [LC 877](https://leetcode.com/problems/stone-game/) |
| 6 | Predict the Winner | Medium | [LC 486](https://leetcode.com/problems/predict-the-winner/) |

---

### Pattern 9: Partition DP (MCM Pattern)

**State:** `dp[i][j]` = minimum cost to process elements from `i` to `j`

**Key Idea:** "Where should I place the last partition/operation?" Try all split points.

This is closely related to Interval DP but specifically focuses on **partitioning** problems.

**Template:**
```java
// Top-down is often cleaner for partition DP
int[][] memo;

int solve(int i, int j) {
    if (i >= j) return 0; // base: single element or empty
    if (memo[i][j] != -1) return memo[i][j];

    int result = Integer.MAX_VALUE;
    for (int k = i; k < j; k++) { // partition at k
        int cost = solve(i, k) + solve(k + 1, j) + /* merge cost(i, k, j) */;
        result = Math.min(result, cost);
    }
    return memo[i][j] = result;
}
```

**Example: Matrix Chain Multiplication**
```java
// Minimize scalar multiplications for matrix chain A1 × A2 × ... × An
// dims[i] = rows of matrix i, dims[n] = cols of matrix n
int solve(int[] dims, int i, int j, int[][] memo) {
    if (i == j) return 0;
    if (memo[i][j] != -1) return memo[i][j];

    int result = Integer.MAX_VALUE;
    for (int k = i; k < j; k++) {
        int cost = solve(dims, i, k, memo)
                 + solve(dims, k + 1, j, memo)
                 + dims[i-1] * dims[k] * dims[j];
        result = Math.min(result, cost);
    }
    return memo[i][j] = result;
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Matrix Chain Multiplication | Medium | [GFG](https://www.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1) |
| 2 | Partition Array for Max Sum | Medium | [LC 1043](https://leetcode.com/problems/partition-array-for-maximum-sum/) |
| 3 | Palindrome Partitioning II | Hard | [LC 132](https://leetcode.com/problems/palindrome-partitioning-ii/) |
| 4 | Minimum Cost Tree From Leaf Values | Medium | [LC 1130](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/) |
| 5 | Super Egg Drop | Hard | [LC 887](https://leetcode.com/problems/super-egg-drop/) |
| 6 | Boolean Parenthesization | Hard | [GFG](https://www.geeksforgeeks.org/problems/boolean-parenthesization5610/1) |

---

### Pattern 10: String DP

**State:** `dp[i][j]` = answer considering first `i` chars of `s1` and first `j` chars of `s2`

**Key Idea:** Process characters one by one, making decisions based on matches/mismatches.

**Template: Edit Distance**
```java
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m + 1][n + 1];

    // Base cases: converting to/from empty string
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i-1) == word2.charAt(j-1)) {
                dp[i][j] = dp[i-1][j-1]; // chars match, no operation
            } else {
                dp[i][j] = 1 + Math.min(dp[i-1][j-1],  // replace
                                Math.min(dp[i-1][j],     // delete
                                         dp[i][j-1]));   // insert
            }
        }
    }
    return dp[m][n];
}
```

**Template: Wildcard / Regex Matching**
```java
// '?' matches one char, '*' matches any sequence
public boolean isMatch(String s, String p) {
    int m = s.length(), n = p.length();
    boolean[][] dp = new boolean[m + 1][n + 1];
    dp[0][0] = true;

    // '*' can match empty sequence
    for (int j = 1; j <= n; j++)
        if (p.charAt(j-1) == '*') dp[0][j] = dp[0][j-1];

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (p.charAt(j-1) == '*') {
                dp[i][j] = dp[i-1][j]    // '*' matches current char
                         || dp[i][j-1];   // '*' matches empty
            } else if (p.charAt(j-1) == '?' || s.charAt(i-1) == p.charAt(j-1)) {
                dp[i][j] = dp[i-1][j-1];
            }
        }
    }
    return dp[m][n];
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Edit Distance | Medium | [LC 72](https://leetcode.com/problems/edit-distance/) |
| 2 | Wildcard Matching | Hard | [LC 44](https://leetcode.com/problems/wildcard-matching/) |
| 3 | Regular Expression Matching | Hard | [LC 10](https://leetcode.com/problems/regular-expression-matching/) |
| 4 | Word Break | Medium | [LC 139](https://leetcode.com/problems/word-break/) |
| 5 | Word Break II | Hard | [LC 140](https://leetcode.com/problems/word-break-ii/) |
| 6 | Distinct Subsequences | Hard | [LC 115](https://leetcode.com/problems/distinct-subsequences/) |
| 7 | Scramble String | Hard | [LC 87](https://leetcode.com/problems/scramble-string/) |

---

### Pattern 11: Tree DP

**State:** Computed via DFS; `dp[node]` = answer for the subtree rooted at `node`

**Key Idea:** Solve for children first (post-order), then combine at parent.

**Template:**
```java
int[] dp; // dp[node] = answer for subtree rooted at node

void dfs(TreeNode node) {
    if (node == null) return;

    dfs(node.left);
    dfs(node.right);

    // Combine children results
    dp[node] = f(dp[node.left], dp[node.right], node.val);
}

// For problems needing two states per node (e.g., include/exclude node)
int[] dfs(TreeNode node) {
    if (node == null) return new int[]{0, 0};

    int[] left = dfs(node.left);
    int[] right = dfs(node.right);

    int include = node.val + left[1] + right[1];  // include this node
    int exclude = Math.max(left[0], left[1])       // exclude this node
                + Math.max(right[0], right[1]);

    return new int[]{include, exclude};
}
```

**Example: House Robber III (Tree)**
```java
public int rob(TreeNode root) {
    int[] result = dfs(root);
    return Math.max(result[0], result[1]);
}

// returns [rob_this_node, skip_this_node]
int[] dfs(TreeNode node) {
    if (node == null) return new int[]{0, 0};
    int[] left = dfs(node.left);
    int[] right = dfs(node.right);

    int robThis = node.val + left[1] + right[1];
    int skipThis = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

    return new int[]{robThis, skipThis};
}
```

**Example: Diameter of Binary Tree**
```java
int diameter = 0;

public int diameterOfBinaryTree(TreeNode root) {
    height(root);
    return diameter;
}

int height(TreeNode node) {
    if (node == null) return 0;
    int left = height(node.left);
    int right = height(node.right);
    diameter = Math.max(diameter, left + right);
    return 1 + Math.max(left, right);
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | House Robber III | Medium | [LC 337](https://leetcode.com/problems/house-robber-iii/) |
| 2 | Diameter of Binary Tree | Easy | [LC 543](https://leetcode.com/problems/diameter-of-binary-tree/) |
| 3 | Binary Tree Maximum Path Sum | Hard | [LC 124](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |
| 4 | Longest ZigZag Path | Medium | [LC 1372](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/) |
| 5 | Binary Tree Cameras | Hard | [LC 968](https://leetcode.com/problems/binary-tree-cameras/) |
| 6 | Unique BSTs | Medium | [LC 96](https://leetcode.com/problems/unique-binary-search-trees/) |
| 7 | Unique BSTs II | Medium | [LC 95](https://leetcode.com/problems/unique-binary-search-trees-ii/) |

---

### Pattern 12: DP on Subsets / Bitmask DP

**State:** `dp[mask]` = answer when the set of selected elements is represented by `mask`

**Key Idea:** Use a bitmask integer where bit `i` being 1 means element `i` is included.

**Constraint:** Typically `n ≤ 20` (since `2^n` states).

**Template:**
```java
int n = /* number of elements */;
int[] dp = new int[1 << n]; // 2^n states
Arrays.fill(dp, /* initial value */);
dp[0] = /* base: empty set */;

for (int mask = 0; mask < (1 << n); mask++) {
    if (dp[mask] == /* invalid */) continue;

    for (int i = 0; i < n; i++) {
        if ((mask & (1 << i)) != 0) continue; // i already selected
        int newMask = mask | (1 << i);
        dp[newMask] = Math.min(dp[newMask],
                               dp[mask] + /* cost of adding element i */);
    }
}
return dp[(1 << n) - 1]; // all elements selected
```

**Useful Bitmask Operations:**
```java
// Check if bit i is set
(mask >> i) & 1  // or  (mask & (1 << i)) != 0

// Set bit i
mask | (1 << i)

// Clear bit i
mask & ~(1 << i)

// Count set bits
Integer.bitCount(mask)

// Iterate over all subsets of mask
for (int sub = mask; sub > 0; sub = (sub - 1) & mask) {
    // sub is a subset of mask
}

// Lowest set bit
mask & (-mask)
```

**Example: Travelling Salesman Problem (TSP)**
```java
// dp[mask][i] = min cost to visit all cities in mask, ending at city i
int[][] dp = new int[1 << n][n];
for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE);
dp[1][0] = 0; // start at city 0

for (int mask = 1; mask < (1 << n); mask++) {
    for (int u = 0; u < n; u++) {
        if (dp[mask][u] == Integer.MAX_VALUE) continue;
        if ((mask & (1 << u)) == 0) continue;
        for (int v = 0; v < n; v++) {
            if ((mask & (1 << v)) != 0) continue;
            int newMask = mask | (1 << v);
            dp[newMask][v] = Math.min(dp[newMask][v],
                                       dp[mask][u] + dist[u][v]);
        }
    }
}
// Answer: min over all i of dp[(1<<n)-1][i] + dist[i][0]
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Partition to K Equal Sum Subsets | Medium | [LC 698](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/) |
| 2 | Shortest Path Visiting All Nodes | Hard | [LC 847](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) |
| 3 | Parallel Courses II | Hard | [LC 1494](https://leetcode.com/problems/parallel-courses-ii/) |
| 4 | Maximum AND Sum of Array | Hard | [LC 2172](https://leetcode.com/problems/maximum-and-sum-of-array/) |
| 5 | Can I Win | Medium | [LC 464](https://leetcode.com/problems/can-i-win/) |
| 6 | Minimum XOR Sum of Two Arrays | Hard | [LC 1879](https://leetcode.com/problems/minimum-xor-sum-of-two-arrays/) |
| 7 | Find the Shortest Superstring | Hard | [LC 943](https://leetcode.com/problems/find-the-shortest-superstring/) |

---

### Pattern 13: Digit DP

**State:** `dp[pos][tight][...extra states]`

**Key Idea:** Count numbers in range `[0, N]` satisfying some property by building numbers digit by digit.

- `pos`: current digit position (left to right)
- `tight`: whether we're still bounded by the upper limit
- Extra states depend on the property (sum of digits, last digit, etc.)

**Template:**
```java
String num;
int[][][] memo;

int solve(int pos, boolean tight, int extraState) {
    if (pos == num.length()) {
        return /* check if valid */;
    }

    if (memo[pos][tight ? 1 : 0][extraState] != -1)
        return memo[pos][tight ? 1 : 0][extraState];

    int limit = tight ? (num.charAt(pos) - '0') : 9;
    int result = 0;

    for (int digit = 0; digit <= limit; digit++) {
        result += solve(pos + 1,
                        tight && (digit == limit),
                        /* update extra state with digit */);
    }

    return memo[pos][tight ? 1 : 0][extraState] = result;
}

// To count in range [L, R]: solve(R) - solve(L-1)
```

**Example: Count Numbers with Unique Digits**
```java
// Count numbers in [0, 10^n) with all distinct digits
public int countNumbersWithUniqueDigits(int n) {
    if (n == 0) return 1;
    int result = 10; // for n=1
    int uniqueDigits = 9, available = 9;
    for (int i = 2; i <= n && available > 0; i++) {
        uniqueDigits *= available;
        result += uniqueDigits;
        available--;
    }
    return result;
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Count Numbers with Unique Digits | Medium | [LC 357](https://leetcode.com/problems/count-numbers-with-unique-digits/) |
| 2 | Numbers At Most N Given Digit Set | Hard | [LC 902](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/) |
| 3 | Non-negative Integers without Consecutive Ones | Hard | [LC 600](https://leetcode.com/problems/non-negative-integers-without-consecutive-ones/) |
| 4 | Count Special Integers | Hard | [LC 2376](https://leetcode.com/problems/count-special-integers/) |
| 5 | Digit Count in Range | Hard | [LC 1067](https://leetcode.com/problems/digit-count-in-range/) |
| 6 | Numbers with Repeated Digits | Hard | [LC 1012](https://leetcode.com/problems/numbers-with-repeated-digits/) |

---

### Pattern 14: DP with State Machines

**State:** `dp[i][state]` where `state` represents a finite machine state

**Key Idea:** Model the problem as transitions between states. At each step, you can either stay in the current state or transition to another.

**Visual:**
```
Best Time to Buy and Sell Stock with Cooldown:

         buy          sell
  REST ------→ HOLD ------→ COOLDOWN
   ↑             |              |
   |             ↓              |
   |           (hold)           |
   └────────────────────────────┘
                (rest/wait)
```

**Template: Stock Problems**
```java
// General stock problem with at most k transactions
// States: dp[i][j][0] = max profit on day i, j transactions done, NOT holding
//         dp[i][j][1] = max profit on day i, j transactions done, HOLDING

int maxProfit(int k, int[] prices) {
    int n = prices.length;
    if (n == 0) return 0;

    int[][][] dp = new int[n][k + 1][2];

    for (int j = 0; j <= k; j++) {
        dp[0][j][0] = 0;
        dp[0][j][1] = -prices[0];
    }

    for (int i = 1; i < n; i++) {
        for (int j = 0; j <= k; j++) {
            dp[i][j][0] = Math.max(dp[i-1][j][0],         // rest
                                   dp[i-1][j][1] + prices[i]); // sell
            if (j > 0)
                dp[i][j][1] = Math.max(dp[i-1][j][1],      // hold
                                       dp[i-1][j-1][0] - prices[i]); // buy
            else
                dp[i][j][1] = dp[i-1][j][1];
        }
    }
    return dp[n-1][k][0];
}
```

**Example: Buy and Sell with Cooldown (simplified)**
```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    if (n < 2) return 0;

    int[] hold = new int[n];    // holding stock
    int[] sold = new int[n];    // just sold
    int[] rest = new int[n];    // cooldown / resting

    hold[0] = -prices[0];
    sold[0] = 0;
    rest[0] = 0;

    for (int i = 1; i < n; i++) {
        hold[i] = Math.max(hold[i-1], rest[i-1] - prices[i]);
        sold[i] = hold[i-1] + prices[i];
        rest[i] = Math.max(rest[i-1], sold[i-1]);
    }
    return Math.max(sold[n-1], rest[n-1]);
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Best Time to Buy and Sell Stock | Easy | [LC 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) |
| 2 | Best Time — II (unlimited) | Medium | [LC 122](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) |
| 3 | Best Time — III (2 transactions) | Hard | [LC 123](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/) |
| 4 | Best Time — IV (k transactions) | Hard | [LC 188](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) |
| 5 | Best Time — with Cooldown | Medium | [LC 309](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) |
| 6 | Best Time — with Transaction Fee | Medium | [LC 714](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) |

---

### Pattern 15: Game Theory DP

**State:** `dp[i][j]` = optimal outcome for the current player given game state `[i..j]`

**Key Idea:** Assume both players play optimally. Current player maximizes, opponent minimizes (or equivalently, current player maximizes their relative advantage).

**Template:**
```java
// Minimax approach
int solve(int[] arr, int i, int j, boolean isMaximizer, int[][] memo) {
    if (i > j) return 0;
    if (memo[i][j] != -1) return memo[i][j];

    if (isMaximizer) {
        return memo[i][j] = Math.max(
            arr[i] + solve(arr, i+1, j, false, memo),
            arr[j] + solve(arr, i, j-1, false, memo)
        );
    } else {
        return memo[i][j] = Math.min(
            solve(arr, i+1, j, true, memo),
            solve(arr, i, j-1, true, memo)
        );
    }
}

// Cleaner: dp[i][j] = max(score_current_player - score_opponent) for arr[i..j]
int solve(int[] arr, int i, int j, int[][] memo) {
    if (i > j) return 0;
    if (memo[i][j] != Integer.MIN_VALUE) return memo[i][j];

    return memo[i][j] = Math.max(
        arr[i] - solve(arr, i+1, j, memo),  // take left
        arr[j] - solve(arr, i, j-1, memo)   // take right
    );
}
// Player 1 wins if solve(arr, 0, n-1) >= 0
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Predict the Winner | Medium | [LC 486](https://leetcode.com/problems/predict-the-winner/) |
| 2 | Stone Game | Medium | [LC 877](https://leetcode.com/problems/stone-game/) |
| 3 | Stone Game II | Medium | [LC 1140](https://leetcode.com/problems/stone-game-ii/) |
| 4 | Stone Game III | Hard | [LC 1406](https://leetcode.com/problems/stone-game-iii/) |
| 5 | Stone Game IV | Hard | [LC 1510](https://leetcode.com/problems/stone-game-iv/) |
| 6 | Can I Win | Medium | [LC 464](https://leetcode.com/problems/can-i-win/) |
| 7 | Nim Game | Easy | [LC 292](https://leetcode.com/problems/nim-game/) |

---

### Pattern 16: Counting DP

**State:** `dp[i]` = number of ways to reach state `i`

**Key Idea:** Sum up all the ways to arrive at a state from valid previous states.

**Template:**
```java
int[] dp = new int[n + 1];
dp[0] = 1; // one way to have nothing

for (int i = 1; i <= n; i++) {
    for (/* all valid transitions to state i */) {
        dp[i] = (dp[i] + dp[/* previous state */]) % MOD;
    }
}
return dp[n];
```

**Example: Decode Ways**
```java
public int numDecodings(String s) {
    int n = s.length();
    int[] dp = new int[n + 1];
    dp[0] = 1;
    dp[1] = s.charAt(0) != '0' ? 1 : 0;

    for (int i = 2; i <= n; i++) {
        int oneDigit = Integer.parseInt(s.substring(i-1, i));
        int twoDigit = Integer.parseInt(s.substring(i-2, i));

        if (oneDigit >= 1) dp[i] += dp[i-1];
        if (twoDigit >= 10 && twoDigit <= 26) dp[i] += dp[i-2];
    }
    return dp[n];
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Decode Ways | Medium | [LC 91](https://leetcode.com/problems/decode-ways/) |
| 2 | Unique Paths | Medium | [LC 62](https://leetcode.com/problems/unique-paths/) |
| 3 | Knight Dialer | Medium | [LC 935](https://leetcode.com/problems/knight-dialer/) |
| 4 | Count Vowels Permutation | Hard | [LC 1220](https://leetcode.com/problems/count-vowels-permutation/) |
| 5 | Number of Ways to Stay | Medium | [LC 1269](https://leetcode.com/problems/number-of-ways-to-stay-in-the-same-place-after-some-steps/) |
| 6 | Count Sorted Vowel Strings | Medium | [LC 1641](https://leetcode.com/problems/count-sorted-vowel-strings/) |
| 7 | Domino and Tromino Tiling | Medium | [LC 790](https://leetcode.com/problems/domino-and-tromino-tiling/) |

---

### Pattern 17: DP on Graphs (DAG)

**State:** `dp[node]` = answer for paths starting/ending at `node`

**Key Idea:** Use topological ordering to ensure all predecessors are processed first.

**Template:**
```java
// Longest path in DAG
int[] dp = new int[V];
Arrays.fill(dp, -1);

int longestPath(int u, List<List<int[]>> adj) {
    if (dp[u] != -1) return dp[u];
    dp[u] = 0;
    for (int[] edge : adj.get(u)) {
        int v = edge[0], w = edge[1];
        dp[u] = Math.max(dp[u], longestPath(v, adj) + w);
    }
    return dp[u];
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Longest Increasing Path in Matrix | Hard | [LC 329](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/) |
| 2 | All Paths From Source to Target | Medium | [LC 797](https://leetcode.com/problems/all-paths-from-source-to-target/) |
| 3 | Number of Ways to Arrive at Destination | Medium | [LC 1976](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/) |
| 4 | Longest Path With Different Adjacent Characters | Hard | [LC 2246](https://leetcode.com/problems/longest-path-with-different-adjacent-characters/) |

---

### Pattern 18: Probability / Expected Value DP

**State:** `dp[state]` = probability or expected value of reaching `state`

**Key Idea:** Expected value = sum of (probability × value) for all outcomes.

**Template:**
```java
// dp[i] = expected number of steps from state i to goal
double[] dp = new double[n + 1];
dp[goal] = 0;

// Fill from goal back to start
for (int i = goal - 1; i >= 0; i--) {
    dp[i] = 1; // one step
    for (/* all transitions from i */) {
        dp[i] += probability * dp[next_state];
    }
}
```

**Problems to Solve:**

| # | Problem | Difficulty | Link |
|---|---------|-----------|------|
| 1 | Knight Probability in Chessboard | Medium | [LC 688](https://leetcode.com/problems/knight-probability-in-chessboard/) |
| 2 | New 21 Game | Medium | [LC 837](https://leetcode.com/problems/new-21-game/) |
| 3 | Soup Servings | Medium | [LC 808](https://leetcode.com/problems/soup-servings/) |
| 4 | Toss Strange Coins | Medium | [LC 1230](https://leetcode.com/problems/toss-strange-coins/) |

---

## How to Identify Which DP Pattern to Use

### Decision Flowchart

```
Start
  │
  ├─ Is it about SUBSEQUENCES of two strings?
  │    → LCS Pattern (#5)
  │
  ├─ Is it about SUBSEQUENCE of one string (ordering)?
  │    → LIS Pattern (#6)
  │
  ├─ Is it about SUBSTRINGS and palindromes?
  │    → Palindrome DP (#7)
  │
  ├─ Is it about MATCHING two strings (edit/transform)?
  │    → String DP (#10)
  │
  ├─ Does it involve a GRID?
  │    → Grid DP (#2)
  │
  ├─ Does it involve a TREE?
  │    → Tree DP (#11)
  │
  ├─ Is it about selecting items with a WEIGHT/CAPACITY constraint?
  │    ├─ Each item used once? → 0/1 Knapsack (#3)
  │    └─ Items reusable?     → Unbounded Knapsack (#4)
  │
  ├─ Does it ask to PARTITION/SPLIT an array optimally?
  │    → Interval/Partition DP (#8, #9)
  │
  ├─ Is n ≤ 20 and about SUBSETS/ASSIGNMENTS?
  │    → Bitmask DP (#12)
  │
  ├─ Is it about counting numbers in a RANGE with digit properties?
  │    → Digit DP (#13)
  │
  ├─ Does it involve BUYING/SELLING or STATES that transition?
  │    → State Machine DP (#14)
  │
  ├─ Is it a TWO-PLAYER game with optimal play?
  │    → Game Theory DP (#15)
  │
  ├─ Does it ask "count the number of WAYS"?
  │    → Counting DP (#16)
  │
  └─ Does it involve a DIRECTED GRAPH?
       → DAG DP (#17)
```

### Quick Reference: Problem Keywords → Pattern

| Keywords in Problem | Likely Pattern |
|---|---|
| "minimum cost", "maximum profit", array | Linear DP |
| grid, matrix, paths, robot | Grid DP |
| knapsack, weight, capacity, subset sum | 0/1 Knapsack |
| coins, unlimited supply, rod cutting | Unbounded Knapsack |
| two strings, common, subsequence | LCS |
| increasing, decreasing, order | LIS |
| palindrome, reverse | Palindrome DP |
| merge, split, parenthesization, balloons | Interval/Partition DP |
| matching, edit distance, transform | String DP |
| tree, root, children, subtree | Tree DP |
| assign, permutation, n ≤ 20 | Bitmask DP |
| range [L, R], digit, number count | Digit DP |
| buy, sell, stock, hold, cooldown | State Machine DP |
| game, player, turns, win/lose | Game Theory DP |
| "number of ways", "how many" | Counting DP |
| DAG, topological, path in graph | Graph DP |

---

## Curated Problem List

### Beginner (Start Here)

| # | Problem | Pattern | Link |
|---|---------|---------|------|
| 1 | Fibonacci Number | Linear | [LC 509](https://leetcode.com/problems/fibonacci-number/) |
| 2 | Climbing Stairs | Linear | [LC 70](https://leetcode.com/problems/climbing-stairs/) |
| 3 | Min Cost Climbing Stairs | Linear | [LC 746](https://leetcode.com/problems/min-cost-climbing-stairs/) |
| 4 | House Robber | Linear | [LC 198](https://leetcode.com/problems/house-robber/) |
| 5 | Maximum Subarray | Linear | [LC 53](https://leetcode.com/problems/maximum-subarray/) |
| 6 | Unique Paths | Grid | [LC 62](https://leetcode.com/problems/unique-paths/) |
| 7 | Minimum Path Sum | Grid | [LC 64](https://leetcode.com/problems/minimum-path-sum/) |
| 8 | Coin Change | Unbounded | [LC 322](https://leetcode.com/problems/coin-change/) |
| 9 | Longest Common Subsequence | LCS | [LC 1143](https://leetcode.com/problems/longest-common-subsequence/) |
| 10 | Longest Increasing Subsequence | LIS | [LC 300](https://leetcode.com/problems/longest-increasing-subsequence/) |

### Intermediate (Build Depth)

| # | Problem | Pattern | Link |
|---|---------|---------|------|
| 11 | House Robber II | Linear | [LC 213](https://leetcode.com/problems/house-robber-ii/) |
| 12 | Maximum Product Subarray | Linear | [LC 152](https://leetcode.com/problems/maximum-product-subarray/) |
| 13 | Decode Ways | Counting | [LC 91](https://leetcode.com/problems/decode-ways/) |
| 14 | Unique Paths II | Grid | [LC 63](https://leetcode.com/problems/unique-paths-ii/) |
| 15 | Triangle | Grid | [LC 120](https://leetcode.com/problems/triangle/) |
| 16 | Maximal Square | Grid | [LC 221](https://leetcode.com/problems/maximal-square/) |
| 17 | Partition Equal Subset Sum | 0/1 Knapsack | [LC 416](https://leetcode.com/problems/partition-equal-subset-sum/) |
| 18 | Target Sum | 0/1 Knapsack | [LC 494](https://leetcode.com/problems/target-sum/) |
| 19 | Coin Change II | Unbounded | [LC 518](https://leetcode.com/problems/coin-change-ii/) |
| 20 | Perfect Squares | Unbounded | [LC 279](https://leetcode.com/problems/perfect-squares/) |
| 21 | Word Break | String | [LC 139](https://leetcode.com/problems/word-break/) |
| 22 | Longest Palindromic Substring | Palindrome | [LC 5](https://leetcode.com/problems/longest-palindromic-substring/) |
| 23 | Longest Palindromic Subsequence | Palindrome | [LC 516](https://leetcode.com/problems/longest-palindromic-subsequence/) |
| 24 | Edit Distance | String | [LC 72](https://leetcode.com/problems/edit-distance/) |
| 25 | Best Time to Buy/Sell Stock IV | State Machine | [LC 188](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) |
| 26 | Best Time — Cooldown | State Machine | [LC 309](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) |
| 27 | Longest String Chain | LIS | [LC 1048](https://leetcode.com/problems/longest-string-chain/) |
| 28 | Delete Operation for Two Strings | LCS | [LC 583](https://leetcode.com/problems/delete-operation-for-two-strings/) |
| 29 | Stone Game | Game Theory | [LC 877](https://leetcode.com/problems/stone-game/) |
| 30 | Knight Dialer | Counting | [LC 935](https://leetcode.com/problems/knight-dialer/) |

### Advanced (Interview Ready)

| # | Problem | Pattern | Link |
|---|---------|---------|------|
| 31 | Burst Balloons | Interval | [LC 312](https://leetcode.com/problems/burst-balloons/) |
| 32 | Palindrome Partitioning II | Partition | [LC 132](https://leetcode.com/problems/palindrome-partitioning-ii/) |
| 33 | Regular Expression Matching | String | [LC 10](https://leetcode.com/problems/regular-expression-matching/) |
| 34 | Wildcard Matching | String | [LC 44](https://leetcode.com/problems/wildcard-matching/) |
| 35 | Russian Doll Envelopes | LIS | [LC 354](https://leetcode.com/problems/russian-doll-envelopes/) |
| 36 | Super Egg Drop | Partition | [LC 887](https://leetcode.com/problems/super-egg-drop/) |
| 37 | Shortest Common Supersequence | LCS | [LC 1092](https://leetcode.com/problems/shortest-common-supersequence/) |
| 38 | Binary Tree Max Path Sum | Tree | [LC 124](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |
| 39 | Binary Tree Cameras | Tree | [LC 968](https://leetcode.com/problems/binary-tree-cameras/) |
| 40 | Cherry Pickup II | Grid | [LC 1463](https://leetcode.com/problems/cherry-pickup-ii/) |
| 41 | Dungeon Game | Grid | [LC 174](https://leetcode.com/problems/dungeon-game/) |
| 42 | Distinct Subsequences | String | [LC 115](https://leetcode.com/problems/distinct-subsequences/) |
| 43 | Interleaving String | LCS | [LC 97](https://leetcode.com/problems/interleaving-string/) |
| 44 | Shortest Path Visiting All Nodes | Bitmask | [LC 847](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) |
| 45 | Profitable Schemes | 0/1 Knapsack | [LC 879](https://leetcode.com/problems/profitable-schemes/) |
| 46 | Scramble String | String/Interval | [LC 87](https://leetcode.com/problems/scramble-string/) |
| 47 | Longest Increasing Path in Matrix | Graph | [LC 329](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/) |
| 48 | Minimum Cost to Merge Stones | Interval | [LC 1000](https://leetcode.com/problems/minimum-cost-to-merge-stones/) |
| 49 | Stone Game III | Game Theory | [LC 1406](https://leetcode.com/problems/stone-game-iii/) |
| 50 | Count Special Integers | Digit | [LC 2376](https://leetcode.com/problems/count-special-integers/) |

---

## Common Pitfalls & Debugging Tips

### Top 10 Mistakes

**1. Wrong Base Case**
```
Symptom: Off-by-one errors, wrong answer for small inputs
Fix:     Always verify dp[0], dp[1] manually with examples
```

**2. Wrong Iteration Direction**
```
Symptom: 0/1 knapsack gives wrong answer (treats as unbounded)
Fix:     0/1 knapsack → iterate capacity BACKWARDS
         Unbounded    → iterate capacity FORWARDS
```

**3. Forgetting to Handle Edge Cases**
```
- Empty array/string
- Single element
- All elements same
- Negative numbers
- Integer overflow (use long or modular arithmetic)
```

**4. State Not Capturing Enough Information**
```
Symptom: Same dp[i] should have different values in different scenarios
Fix:     Add another dimension to the state
```

**5. Off-by-One in String/Array Indexing**
```
When dp is (n+1) sized for n characters:
  dp[i][j] refers to first i chars of s1 and first j chars of s2
  Characters are s1.charAt(i-1) and s2.charAt(j-1)   ← NOT s1.charAt(i)!
```

**6. Modular Arithmetic Mistakes**
```
Always mod AFTER addition: (a + b) % MOD
For subtraction: ((a - b) % MOD + MOD) % MOD   ← prevent negative
Never mod before comparison (max/min operations)
```

**7. Not Initializing DP Array Properly**
```
Min problems → Initialize with Integer.MAX_VALUE (or INF)
Max problems → Initialize with Integer.MIN_VALUE (or 0)
Boolean       → Initialize with false
Count         → Initialize with 0 (base case dp[0] = 1 usually)
```

**8. Confusing Subsequence vs Substring**
```
Subsequence: elements in order, not necessarily contiguous
             → dp[i][j] carries forward: max(dp[i-1][j], dp[i][j-1])
Substring:   contiguous elements
             → dp[i][j] resets to 0 on mismatch
```

**9. Space Optimization Breaking Correctness**
```
When reducing 2D to 1D, make sure to handle the dependency order correctly.
Draw the dependency arrows in the DP table first!
```

**10. TLE from Top-Down with HashMap**
```
If state space is dense, switch to bottom-up with array.
Arrays are ~5x faster than HashMap for lookups.
```

### Debugging Checklist

```
□ Print the entire DP table for a small example
□ Verify base cases manually
□ Check iteration direction matches dependencies
□ Verify the answer location (dp[n]? dp[n][W]? max of dp[]?)
□ Test edge cases: n=0, n=1, all same elements
□ Check for integer overflow
□ Verify modular arithmetic if required
```

---

## Study Plan

### Week 1: Foundations

| Day | Topic | Problems |
|-----|-------|----------|
| 1 | Theory + Fibonacci + Climbing Stairs | LC 509, 70, 746 |
| 2 | Linear DP: House Robber series | LC 198, 213 |
| 3 | Linear DP: Kadane's + Product | LC 53, 152 |
| 4 | Grid DP: Paths + Min Cost | LC 62, 63, 64 |
| 5 | Grid DP: Triangle + Maximal Square | LC 120, 221 |
| 6 | 0/1 Knapsack: Theory + Subset Sum | GFG 0/1 Knapsack, LC 416 |
| 7 | 0/1 Knapsack: Target Sum + Variations | LC 494, 1049 |

### Week 2: Classic Patterns

| Day | Topic | Problems |
|-----|-------|----------|
| 8 | Unbounded Knapsack: Coin Change | LC 322, 518 |
| 9 | Unbounded Knapsack: Combo vs Permutation | LC 377, 279, GFG Rod Cutting |
| 10 | LCS: Theory + Problems | LC 1143, 583, 1092 |
| 11 | LIS: O(n²) + O(n log n) | LC 300, 673, 1048 |
| 12 | Palindrome DP | LC 5, 516, 647 |
| 13 | String DP: Edit Distance + Matching | LC 72, 44, 139 |
| 14 | Review + Redo missed problems | — |

### Week 3: Advanced Patterns

| Day | Topic | Problems |
|-----|-------|----------|
| 15 | Interval / Range DP | LC 312, 877, 486 |
| 16 | Partition DP (MCM) | GFG MCM, LC 1043, 132 |
| 17 | State Machine: Stock series | LC 121, 122, 123, 188, 309, 714 |
| 18 | Tree DP | LC 337, 543, 124, 968 |
| 19 | Game Theory DP | LC 877, 1140, 1406, 464 |
| 20 | Counting DP | LC 91, 935, 1220, 790 |
| 21 | Review + Redo missed problems | — |

### Week 4: Expert Level

| Day | Topic | Problems |
|-----|-------|----------|
| 22 | Bitmask DP | LC 698, 847, 464 |
| 23 | Digit DP | LC 357, 902, 2376 |
| 24 | Graph DP (DAG) | LC 329, 797, 2246 |
| 25 | Probability DP | LC 688, 837 |
| 26 | Mixed practice (hard) | LC 10, 87, 1000 |
| 27 | Mixed practice (hard) | LC 879, 943, 174 |
| 28 | Full review + Mock interview | Pick 3 random hard |

---

## Additional Resources

### Must-Read Articles
- [Atcoder DP Contest](https://atcoder.jp/contests/dp/tasks) — 26 classic DP problems, gold standard for practice
- [CSES Problem Set - DP Section](https://cses.fi/problemset/) — Excellent set of DP problems
- [LeetCode DP Study Plan](https://leetcode.com/studyplan/dynamic-programming/)

### Video Resources
- Striver's DP Series (YouTube) — 56 videos covering all patterns
- Neetcode DP Playlist (YouTube) — Concise explanations with visuals
- Abdul Bari's DP lectures (YouTube) — Great for theory/intuition

---

## Cheat Sheet: All Patterns at a Glance

```
┌─────────────────────────────────────────────────────────────────┐
│  PATTERN             │  STATE              │  TIME      │ SPACE │
├──────────────────────┼─────────────────────┼────────────┼───────┤
│  Linear DP           │  dp[i]              │  O(n)      │ O(1)  │
│  Grid DP             │  dp[i][j]           │  O(m×n)    │ O(n)  │
│  0/1 Knapsack        │  dp[i][w] → dp[w]   │  O(n×W)    │ O(W)  │
│  Unbounded Knapsack  │  dp[w]              │  O(n×W)    │ O(W)  │
│  LCS                 │  dp[i][j]           │  O(m×n)    │ O(n)  │
│  LIS                 │  dp[i] or tails[]   │  O(n log n)│ O(n)  │
│  Palindrome          │  dp[i][j]           │  O(n²)     │ O(n²) │
│  Interval/Range      │  dp[i][j]           │  O(n³)     │ O(n²) │
│  Partition (MCM)     │  dp[i][j]           │  O(n³)     │ O(n²) │
│  String DP           │  dp[i][j]           │  O(m×n)    │ O(n)  │
│  Tree DP             │  dp[node]           │  O(n)      │ O(n)  │
│  Bitmask DP          │  dp[mask]           │  O(2ⁿ×n)   │ O(2ⁿ) │
│  Digit DP            │  dp[pos][tight][..] │  O(D×S)    │ O(D×S)│
│  State Machine       │  dp[i][state]       │  O(n×S)    │ O(S)  │
│  Game Theory         │  dp[i][j]           │  O(n²)     │ O(n²) │
│  Counting            │  dp[i]              │  O(n×T)    │ O(n)  │
│  Graph (DAG)         │  dp[node]           │  O(V+E)    │ O(V)  │
│  Probability         │  dp[state]          │  varies    │varies │
└─────────────────────────────────────────────────────────────────┘

Key iteration rules:
  0/1 Knapsack (1D):    capacity BACKWARDS  ← each item once
  Unbounded Knapsack:   capacity FORWARDS   ← items reusable
  Combinations:         items outer loop
  Permutations:         amount outer loop
  Interval DP:          iterate by LENGTH
  LCS / String DP:      row by row, left to right
```
