---
layout: default
title: Merge Sorted Array
---

**88. Merge Sorted Array**.

### 1. Common Mistake: The Naive Approach (Sorting)

This approach simply copies the second array into the first and uses the built-in sort function.

```java
import java.util.Arrays;

class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Step 1: Append all elements of nums2 into the empty space of nums1
        for (int i = 0; i < n; i++) {
            nums1[m + i] = nums2[i];
        }

        // Step 2: Sort the entire array
        // Time Complexity: O((m+n) log(m+n)) - Very Slow compared to O(m+n)
        Arrays.sort(nums1);
    }
}

```

**Why this is a mistake:**

* **Performance:** Sorting takes  time. The optimized Two Pointer approach takes  time. As the input size grows, this solution becomes significantly slower.
* **Interview Signal:** It tells the interviewer you rely on libraries rather than understanding the underlying algorithm (merging logic).

---

### 2. Optimized Solution (Java)

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Pointers for the end of valid elements in nums1 and nums2
        int p1 = m - 1;
        int p2 = n - 1;
        
        // Pointer for the end of the total array (where we place elements)
        int pMerge = m + n - 1;
        
        // While there are elements in nums2 to be processed
        while (p2 >= 0) {
            // If p1 is exhausted, or nums2 element is larger, take from nums2
            if (p1 >= 0 && nums1[p1] > nums2[p2]) {
                nums1[pMerge] = nums1[p1];
                p1--;
            } else {
                nums1[pMerge] = nums2[p2];
                p2--;
            }
            pMerge--;
        }
    }
}

```

---

### 3. Explanation, Pattern & Approach

**Pattern: Two Pointers (Reverse Iteration)**
Standard Two Pointer problems usually start from the beginning. However, when merging into an array that has empty space at the *end*, you almost always want to fill it from the **back** to avoid overwriting data you haven't processed yet.

**Approach Point-to-Point:**

1. **Pointers Setup:**
* `p1` points to the last valid element in `nums1`.
* `p2` points to the last element in `nums2`.
* `pMerge` points to the very last index of `nums1` (the "empty" zone).


2. **The Decision Logic:**
* We compare `nums1[p1]` and `nums2[p2]`.
* The **larger** of the two is placed at `pMerge`. This ensures the end of the array is sorted first.


3. **Handling Edge Cases:**
* The loop condition is `while (p2 >= 0)`. Why?
* If `p1` runs out first (e.g., `nums1` was `[4,5,6]` and `nums2` was `[1,2,3]`), the remaining elements in `nums1` are *already* in the correct place. We don't need to touch them.
* If `p2` still has elements (e.g., `nums2` has small numbers left), they must be copied into `nums1`. The loop ensures we don't miss them.


4. **Complexity:**
* **Time:**  — We iterate through each element at most once.
* **Space:**  — We modify `nums1` directly without using auxiliary data structures.


