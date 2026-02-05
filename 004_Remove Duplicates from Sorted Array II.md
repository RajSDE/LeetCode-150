---
layout: default
title: Remove Duplicates from Sorted Array II
permalink: /problems/remove-duplicates-sorted-array-II/
---

**80. Remove Duplicates from Sorted Array II** (Medium).

### 1. The Question Explained (Simply)

This is an extension of the previous problem. You still have a sorted deck of cards (e.g., `1, 1, 1, 2, 2, 3`), but the rules have changed.

* **New Rule:** You are allowed to keep duplicates, but **at most two** of each number.
* **Example:** If you have `[1, 1, 1, 2, 2, 3]`, you must remove the third `1`. The result should be `[1, 1, 2, 2, 3]`.
* **Constraint:** Still **in-place** ( space) and **sorted**.

---

### 2. Common Mistake: The "Counter Spaghetti" Code

A common mistake is trying to adapt the previous solution by adding a `count` variable that resets every time the number changes. While this logic *can* work, it often leads to messy, bug-prone code ("Spaghetti Code") where you lose track of which index you are writing to vs. reading from.

**The Messy/Error-Prone Approach:**

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        // Mistake: Over-engineering with complex state management
        int i = 0;
        int count = 1; // trying to track count of current number
        
        for (int j = 1; j < nums.length; j++) {
            if (nums[j] == nums[i]) {
                if (count < 2) { // logic nested inside logic
                    i++;
                    nums[i] = nums[j];
                    count++;
                }
                // else skip
            } else {
                i++;
                nums[i] = nums[j];
                count = 1; // reset count
            }
        }
        return i + 1;
    }
}

```

**Why this is a risk:**

* It is hard to read and debug during an interview.
* You have to carefully manage the `count` reset and the `i` increment separately.
* There is a much cleaner "Generic Pattern" that solves this in 3 lines.

---

### 3. Optimized Solution (Java)

We use the **Two Pointer** pattern again, but with a slight modification to the "Look Back" logic.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        // Any array with 2 or fewer elements is already valid.
        if (nums.length <= 2) return nums.length;
        
        // 'insertPos' is our Writer. 
        // We start at 2 because the first two elements (indices 0 and 1) 
        // are always allowed to stay, regardless of what they are.
        int insertPos = 2;

        // 'i' is our Reader. Start scanning from index 2.
        for (int i = 2; i < nums.length; i++) {
            
            // THE CORE LOGIC:
            // Instead of checking the immediate neighbor (insertPos - 1),
            // we check the element TWO spots back (insertPos - 2).
            if (nums[i] != nums[insertPos - 2]) {
                
                // If nums[i] is NOT the same as the number two spots back,
                // it means we haven't used this number twice yet. It's valid.
                nums[insertPos] = nums[i];
                insertPos++;
            }
            // If it IS the same as nums[insertPos - 2], then we already have
            // two of these numbers (at insertPos-1 and insertPos-2). 
            // So we skip this one.
        }

        return insertPos;
    }
}

```

---

### 4. Explanation, Pattern & Approach

**Pattern: Two Pointers (Look Back K)**
This is a powerful generalization.

* If you want at most **1** duplicate, you check `nums[i] != nums[insertPos - 1]`.
* If you want at most **2** duplicates, you check `nums[i] != nums[insertPos - 2]`.
* If you want at most **K** duplicates, you check `nums[i] != nums[insertPos - K]`.

**Approach Point-to-Point:**

1. **Initialization:** We know `nums[0]` and `nums[1]` are safe. Even if they are `1, 1`, that's allowed. So we start our work at index `2`.
2. **The Check:** `nums[insertPos - 2]` represents the value that sits at the "start" of our allowed pair.
* If we are building `[1, 1, ...]` and our next number is `1`.
* `insertPos` is at index 2. `insertPos - 2` is index 0 (value `1`).
* Is the new `1` different from the `1` at index 0? No. So we skip it.


3. **Efficiency:**
* **Time:**  — Single pass.
* **Space:**  — In-place.



**Next Problem:**
The next problem is **169. Majority Element**. This is a classic interview question that has a very specific, famous algorithm (Boyer-Moore Voting Algorithm) to solve it efficiently.

Ready to learn the Voting Algorithm?
