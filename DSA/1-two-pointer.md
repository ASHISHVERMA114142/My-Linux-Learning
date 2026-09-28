# Two Pointer Algorithm - Complete Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Core Concepts](#core-concepts)
3. [Pattern Categories](#pattern-categories)
4. [Problem-Solving Framework](#problem-solving-framework)
5. [Curated Problem List](#curated-problem-list)
6. [Advanced Techniques](#advanced-techniques)
7. [Common Pitfalls](#common-pitfalls)

---

## Introduction

The Two Pointer technique is a powerful algorithmic pattern that uses two pointers to traverse data structures, typically arrays or linked lists, to solve problems efficiently. It often reduces time complexity from O(n²) to O(n).

### When to Use Two Pointers?
- Problems involving sorted arrays
- Finding pairs/triplets with specific properties
- Partitioning arrays
- Sliding window problems
- Removing duplicates
- Reversing or rearranging elements

---

## Core Concepts

### 1. **Opposite Direction Pointers**
Two pointers start at opposite ends and move toward each other.

```
left →              ← right
[1, 2, 3, 4, 5, 6, 7, 8]
```

**Use Cases:**
- Finding pairs with target sum in sorted array
- Palindrome checking
- Container with most water

### 2. **Same Direction Pointers (Fast & Slow)**
Both pointers move in the same direction at different speeds.

```
slow →    fast →
[1, 2, 3, 4, 5, 6, 7, 8]
```

**Use Cases:**
- Removing duplicates
- Cycle detection
- Finding middle element
- In-place array modifications

### 3. **Sliding Window**
Two pointers maintain a window that slides through the array.

```
left →    right →
[1, 2, 3, 4, 5, 6, 7, 8]
    [window]
```

**Use Cases:**
- Subarray sum problems
- Longest substring problems
- Maximum/minimum in window

---

## Pattern Categories

### Pattern 1: Opposite Direction (Convergent)

**Template:**
```java
int left = 0, right = arr.length - 1;
while (left < right) {
    if (condition) {
        // Process and move pointers
        left++;
        // or right--;
    } else if (anotherCondition) {
        right--;
    } else {
        left++;
        right--;
    }
}
```

### Pattern 2: Same Direction (Fast & Slow)

**Template:**
```java
int slow = 0;
for (int fast = 0; fast < arr.length; fast++) {
    if (condition) {
        arr[slow] = arr[fast];
        slow++;
    }
}
```

### Pattern 3: Sliding Window (Variable Size)

**Template:**
```java
int left = 0, right = 0;
while (right < arr.length) {
    // Expand window
    addToWindow(arr[right]);

    // Shrink window if needed
    while (windowInvalid()) {
        removeFromWindow(arr[left]);
        left++;
    }

    // Update result
    updateResult();
    right++;
}
```

---

## Problem-Solving Framework

### Step 1: Identify the Pattern
- Is the array sorted? → Consider opposite direction
- Need to modify in-place? → Consider fast & slow
- Looking for subarray/substring? → Consider sliding window

### Step 2: Define Pointer Movement Rules
- What condition moves left pointer?
- What condition moves right pointer?
- When do we update the result?

### Step 3: Handle Edge Cases
- Empty array
- Single element
- All elements same
- No valid answer exists

### Step 4: Optimize
- Can we avoid extra space?
- Can we reduce comparisons?
- Are there early termination conditions?

---

## Curated Problem List

### 🟢 Level 1: Core Fundamentals (Master These First)

#### 1. **Two Sum II - Input Array Is Sorted**
- **Platform:** LeetCode #167
- **Difficulty:** Easy
- **Pattern:** Opposite Direction
- **Key Learning:** Basic two-pointer on sorted array
- **Link:** https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/

#### 2. **Remove Duplicates from Sorted Array**
- **Platform:** LeetCode #26
- **Difficulty:** Easy
- **Pattern:** Fast & Slow
- **Key Learning:** In-place modification with two pointers
- **Link:** https://leetcode.com/problems/remove-duplicates-from-sorted-array/

#### 3. **Valid Palindrome**
- **Platform:** LeetCode #125
- **Difficulty:** Easy
- **Pattern:** Opposite Direction
- **Key Learning:** String manipulation with two pointers
- **Link:** https://leetcode.com/problems/valid-palindrome/

#### 4. **Move Zeroes**
- **Platform:** LeetCode #283
- **Difficulty:** Easy
- **Pattern:** Fast & Slow
- **Key Learning:** Partitioning array elements
- **Link:** https://leetcode.com/problems/move-zeroes/

#### 5. **Reverse String**
- **Platform:** LeetCode #344
- **Difficulty:** Easy
- **Pattern:** Opposite Direction
- **Key Learning:** Basic pointer swapping
- **Link:** https://leetcode.com/problems/reverse-string/

---

### 🟡 Level 2: Intermediate Techniques

#### 6. **Container With Most Water**
- **Platform:** LeetCode #11
- **Difficulty:** Medium
- **Pattern:** Opposite Direction
- **Key Learning:** Greedy approach with two pointers, understanding when to move which pointer
- **Link:** https://leetcode.com/problems/container-with-most-water/

#### 7. **3Sum**
- **Platform:** LeetCode #15
- **Difficulty:** Medium
- **Pattern:** Opposite Direction + Iteration
- **Key Learning:** Combining two-pointer with outer loop, handling duplicates
- **Link:** https://leetcode.com/problems/3sum/

#### 8. **Sort Colors (Dutch National Flag)**
- **Platform:** LeetCode #75
- **Difficulty:** Medium
- **Pattern:** Three Pointers
- **Key Learning:** Partitioning with multiple pointers
- **Link:** https://leetcode.com/problems/sort-colors/

#### 9. **Remove Nth Node From End of List**
- **Platform:** LeetCode #19
- **Difficulty:** Medium
- **Pattern:** Fast & Slow (with gap)
- **Key Learning:** Maintaining fixed distance between pointers
- **Link:** https://leetcode.com/problems/remove-nth-node-from-end-of-list/

#### 10. **Linked List Cycle II**
- **Platform:** LeetCode #142
- **Difficulty:** Medium
- **Pattern:** Floyd's Cycle Detection
- **Key Learning:** Fast & slow pointer for cycle detection and finding entry point
- **Link:** https://leetcode.com/problems/linked-list-cycle-ii/

#### 11. **Trapping Rain Water**
- **Platform:** LeetCode #42
- **Difficulty:** Hard (but excellent for two-pointer practice)
- **Pattern:** Opposite Direction
- **Key Learning:** Maintaining max heights while moving pointers
- **Link:** https://leetcode.com/problems/trapping-rain-water/

#### 12. **Subarray Product Less Than K**
- **Platform:** LeetCode #713
- **Difficulty:** Medium
- **Pattern:** Sliding Window
- **Key Learning:** Variable size window with product condition
- **Link:** https://leetcode.com/problems/subarray-product-less-than-k/

---

### 🔴 Level 3: Advanced Applications

#### 13. **4Sum**
- **Platform:** LeetCode #18
- **Difficulty:** Medium
- **Pattern:** Nested Two Pointers
- **Key Learning:** Extending 3Sum pattern, optimization techniques
- **Link:** https://leetcode.com/problems/4sum/

#### 14. **Minimum Window Substring**
- **Platform:** LeetCode #76
- **Difficulty:** Hard
- **Pattern:** Sliding Window
- **Key Learning:** Complex window validation with hash maps
- **Link:** https://leetcode.com/problems/minimum-window-substring/

#### 15. **Longest Substring Without Repeating Characters**
- **Platform:** LeetCode #3
- **Difficulty:** Medium
- **Pattern:** Sliding Window
- **Key Learning:** Window with uniqueness constraint
- **Link:** https://leetcode.com/problems/longest-substring-without-repeating-characters/

#### 16. **Partition Labels**
- **Platform:** LeetCode #763
- **Difficulty:** Medium
- **Pattern:** Greedy + Two Pointers
- **Key Learning:** Dynamic end pointer based on character positions
- **Link:** https://leetcode.com/problems/partition-labels/

#### 17. **Minimum Size Subarray Sum**
- **Platform:** LeetCode #209
- **Difficulty:** Medium
- **Pattern:** Sliding Window
- **Key Learning:** Finding minimum window with sum constraint
- **Link:** https://leetcode.com/problems/minimum-size-subarray-sum/

#### 18. **3Sum Closest**
- **Platform:** LeetCode #16
- **Difficulty:** Medium
- **Pattern:** Opposite Direction + Optimization
- **Key Learning:** Tracking closest value while using two pointers
- **Link:** https://leetcode.com/problems/3sum-closest/

#### 19. **Intersection of Two Arrays II**
- **Platform:** LeetCode #350
- **Difficulty:** Easy (but good for sorted array technique)
- **Pattern:** Same Direction on Two Arrays
- **Key Learning:** Merging technique with two pointers
- **Link:** https://leetcode.com/problems/intersection-of-two-arrays-ii/

#### 20. **Merge Sorted Array**
- **Platform:** LeetCode #88
- **Difficulty:** Easy
- **Pattern:** Opposite Direction (from end)
- **Key Learning:** Merging in-place from the back
- **Link:** https://leetcode.com/problems/merge-sorted-array/

---

### 🏆 Level 4: Expert Challenges

#### 21. **Substring with Concatenation of All Words**
- **Platform:** LeetCode #30
- **Difficulty:** Hard
- **Pattern:** Sliding Window + Hash Map
- **Key Learning:** Complex window validation with word boundaries
- **Link:** https://leetcode.com/problems/substring-with-concatenation-of-all-words/

#### 22. **Longest Repeating Character Replacement**
- **Platform:** LeetCode #424
- **Difficulty:** Medium
- **Pattern:** Sliding Window
- **Key Learning:** Window with replacement constraint
- **Link:** https://leetcode.com/problems/longest-repeating-character-replacement/

#### 23. **Subarrays with K Different Integers**
- **Platform:** LeetCode #992
- **Difficulty:** Hard
- **Pattern:** Sliding Window
- **Key Learning:** "Exactly K" = "At Most K" - "At Most K-1"
- **Link:** https://leetcode.com/problems/subarrays-with-k-different-integers/

#### 24. **Find All Anagrams in a String**
- **Platform:** LeetCode #438
- **Difficulty:** Medium
- **Pattern:** Fixed Size Sliding Window
- **Key Learning:** Efficient character frequency matching
- **Link:** https://leetcode.com/problems/find-all-anagrams-in-a-string/

#### 25. **Boats to Save People**
- **Platform:** LeetCode #881
- **Difficulty:** Medium
- **Pattern:** Opposite Direction + Greedy
- **Key Learning:** Pairing strategy with two pointers
- **Link:** https://leetcode.com/problems/boats-to-save-people/

#### 26. **Longest Mountain in Array**
- **Platform:** LeetCode #845
- **Difficulty:** Medium
- **Pattern:** Two Pointers + Pattern Recognition
- **Key Learning:** Expanding from center technique
- **Link:** https://leetcode.com/problems/longest-mountain-in-array/

#### 27. **Fruit Into Baskets**
- **Platform:** LeetCode #904
- **Difficulty:** Medium
- **Pattern:** Sliding Window
- **Key Learning:** Window with at most K distinct elements
- **Link:** https://leetcode.com/problems/fruit-into-baskets/

#### 28. **Max Consecutive Ones III**
- **Platform:** LeetCode #1004
- **Difficulty:** Medium
- **Pattern:** Sliding Window
- **Key Learning:** Window with flip constraint
- **Link:** https://leetcode.com/problems/max-consecutive-ones-iii/

---

### 🎯 Bonus: GFG & Other Platforms

#### 29. **Count Triplets with Sum Smaller than X**
- **Platform:** GeeksforGeeks
- **Difficulty:** Medium
- **Pattern:** Opposite Direction
- **Key Learning:** Counting valid combinations
- **Link:** https://www.geeksforgeeks.org/problems/count-triplets-with-sum-smaller-than-x5549/1

#### 30. **Pair with Given Difference**
- **Platform:** GeeksforGeeks
- **Difficulty:** Easy
- **Pattern:** Same Direction
- **Key Learning:** Finding pairs with difference in sorted array
- **Link:** https://www.geeksforgeeks.org/problems/find-pair-given-difference1559/1

---

## Advanced Techniques

### 1. **Three Pointers Pattern**

Used in problems like Dutch National Flag (Sort Colors).

```java
int low = 0, mid = 0, high = arr.length - 1;
while (mid <= high) {
    if (arr[mid] == 0) {
        swap(arr, low, mid);
        low++;
        mid++;
    } else if (arr[mid] == 1) {
        mid++;
    } else {
        swap(arr, mid, high);
        high--;
    }
}
```

### 2. **Two Pointers on Two Arrays**

Used in merge operations or finding common elements.

```java
int i = 0, j = 0;
while (i < arr1.length && j < arr2.length) {
    if (arr1[i] == arr2[j]) {
        // Found common element
        i++;
        j++;
    } else if (arr1[i] < arr2[j]) {
        i++;
    } else {
        j++;
    }
}
```

### 3. **Sliding Window with Hash Map**

For complex window validation.

```java
Map<Character, Integer> map = new HashMap<>();
int left = 0, right = 0;
while (right < s.length()) {
    char c = s.charAt(right);
    map.put(c, map.getOrDefault(c, 0) + 1);

    while (windowInvalid(map)) {
        char leftChar = s.charAt(left);
        map.put(leftChar, map.get(leftChar) - 1);
        if (map.get(leftChar) == 0) {
            map.remove(leftChar);
        }
        left++;
    }

    updateResult();
    right++;
}
```

### 4. **Fast & Slow Pointer (Floyd's Algorithm)**

For cycle detection in linked lists.

```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow == fast) {
        // Cycle detected
        return true;
    }
}
return false;
```

---

## Common Pitfalls

### 1. **Off-by-One Errors**
```java
// ❌ Wrong: might miss last element
while (left < right - 1)

// ✅ Correct
while (left < right)
```

### 2. **Not Handling Duplicates**
```java
// In 3Sum, after finding a triplet:
while (left < right && nums[left] == nums[left + 1]) left++;
while (left < right && nums[right] == nums[right - 1]) right--;
```

### 3. **Incorrect Window Shrinking**
```java
// ❌ Wrong: shrink only once
if (windowInvalid()) {
    left++;
}

// ✅ Correct: shrink until valid
while (windowInvalid()) {
    left++;
}
```

### 4. **Forgetting to Sort**
Many two-pointer problems require sorted input. Always check if sorting is needed.

### 5. **Integer Overflow**
When dealing with sums or products, watch for overflow:
```java
// Use long for large sums
long sum = (long) nums[i] + nums[j] + nums[k];
```

---

## Practice Strategy

### Week 1: Fundamentals
- Solve problems 1-5
- Focus on understanding basic pointer movement
- Practice writing clean, bug-free code

### Week 2: Core Patterns
- Solve problems 6-12
- Learn to identify which pattern to use
- Practice handling edge cases

### Week 3: Advanced Applications
- Solve problems 13-20
- Combine two-pointer with other techniques
- Focus on optimization

### Week 4: Expert Level
- Solve problems 21-28
- Tackle hard problems
- Review and solidify understanding

### Week 5: Mixed Practice
- Solve GFG problems (29-30)
- Revisit difficult problems
- Time yourself to improve speed

---

## Time & Space Complexity

### Typical Complexities:
- **Time:** O(n) for single pass, O(n log n) if sorting required
- **Space:** O(1) for most in-place operations, O(n) if using hash maps

### Comparison with Brute Force:
- **Brute Force (nested loops):** O(n²)
- **Two Pointers:** O(n)
- **Improvement:** Significant for large inputs

---

## Key Takeaways

1. **Two pointers reduce time complexity** from O(n²) to O(n) in many cases
2. **Pattern recognition is crucial** - identify whether to use opposite direction, same direction, or sliding window
3. **Sorted arrays** often hint at two-pointer solutions
4. **In-place modifications** are efficiently done with fast & slow pointers
5. **Sliding window** is a specialized two-pointer technique for subarray/substring problems
6. **Practice is essential** - start with easy problems and gradually increase difficulty

---

## Additional Resources

- **Visualizations:** Use pen and paper to trace pointer movements
- **Debugging:** Print pointer positions at each step when stuck
- **Variations:** Try solving problems with different pointer strategies
- **Discussion Forums:** Read LeetCode discussions after solving

---

## Quick Reference Card

| Problem Type | Pattern | Template |
|--------------|---------|----------|
| Pair with target sum (sorted) | Opposite Direction | `left=0, right=n-1` |
| Remove duplicates | Fast & Slow | `slow=0, fast iterates` |
| Subarray sum | Sliding Window | `left=0, right=0, expand & shrink` |
| Cycle detection | Floyd's | `slow=1x, fast=2x` |
| Partition array | Three Pointers | `low, mid, high` |
| Merge arrays | Two Arrays | `i=0, j=0` |

---

**Happy Coding! 🚀**

*Remember: The key to mastering two pointers is understanding WHEN to move which pointer and WHY.*
