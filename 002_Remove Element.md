---
layout: default
title: Remove Element
permalink: /problems/remove-element/
order: 2
---

**27. Remove Element**.

### 1. Common Mistakes to Avoid (Naive Code)

The most common mistake is finding the element to remove and then manually **shifting all subsequent elements to the left** one by one. This is functionally correct but computationally expensive.

**The Mistake: Brute Force Shifting ()**

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] == val) {
                // Mistake: Shifting every element left one by one
                // This nested loop makes it O(N^2)
                for (int j = i; j < n - 1; j++) {
                    nums[j] = nums[j + 1];
                }
                // Reduce array size and decrement i to check the new value at this index
                n--;
                i--; 
            }
        }
        return n;
    }
}

```

**Why this is a mistake:**

* **Time Complexity:** In the worst case (e.g., removing all elements), you shift the array  times for  elements, resulting in .
* **Inefficient:** You are moving data that you might move again later.

---

### 2. Optimized Solution (Java)

We use the **Two Pointer (Reader/Writer)** pattern to solve this in a single pass ().

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        // 'writer' pointer tracks the position for the next valid element
        int writer = 0;

        // 'reader' pointer iterates through the entire array
        for (int reader = 0; reader < nums.length; reader++) {
            
            // If the current element is NOT the value we want to remove...
            if (nums[reader] != val) {
                // ...we write it to the 'writer' position
                nums[writer] = nums[reader];
                
                // Advance the writer pointer
                writer++;
            }
            // If it IS the value, we basically ignore it (skip it)
        }

        // 'writer' now sits at the index just after the valid elements
        // which represents the new length of the array
        return writer;
    }
}

```

---

### 3. Explanation, Pattern & Approach

**Pattern: Two Pointers (Reader / Writer)**
This is a specific variation of the Two Pointer technique. Instead of pointers moving toward each other, both pointers move forward, but at different speeds.

* **Reader (`i` or `reader`):** Scans every element to check its value.
* **Writer (`k` or `writer`):** Only moves when we find a value we want to *keep*.

**Approach Point-to-Point:**

1. **Logic:** We don't actually "delete" items (which is hard in arrays). Instead, we **overwrite** the bad items with the good items.
2. **Execution:**
* As the `reader` scans, if it sees a "good" number (not `val`), it copies it to the `writer`'s location.
* If the `reader` sees a "bad" number (equal to `val`), it does nothing. The `writer` stays put.
* The next time the `reader` finds a "good" number, it overwrites the "bad" number sitting at the `writer`'s index.


3. **Efficiency:**
* **Time:**  — We iterate through the array exactly once.
* **Space:**  — We modify the array in-place.



**Next Step:**
The next problem in the plan is **26. Remove Duplicates from Sorted Array**. This uses a very similar pattern but with a slight twist because the array is sorted.
