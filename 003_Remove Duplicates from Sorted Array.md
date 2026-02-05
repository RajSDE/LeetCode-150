---
layout: default
title: Remove Duplicates from Sorted Array
permalink: /problems/remove-duplicates-sorted-array/
---

**26. Remove Duplicates from Sorted Array**.

### 1. The Question Explained (Simply)

Imagine you have a deck of cards that is already sorted (e.g., 1, 1, 2, 2, 3, 4, 4), but you want to remove the duplicates so you only have unique cards (e.g., 1, 2, 3, 4).

* **Constraint 1:** You must do this **"in-place."** You cannot buy a new table (create a new array) to lay out the unique cards. You have to stack the unique ones at the front of the original pile.
* **Constraint 2:** The array is **sorted**. This is a huge hint. It means all duplicate numbers are sitting right next to each other.
* **Goal:** Return the count of unique numbers (`k`). The first `k` slots of your array must contain these unique numbers in order. We don't care what is left in the slots after `k`.

---

### 2. Common Mistake: The Naive Approach (Using Extra Space)

The most common "lazy" way to solve this is to throw everything into a `HashSet` (which automatically deletes duplicates) and then put them back into the array.

**The Mistake Code:**

```java
import java.util.HashSet;

class Solution {
    public int removeDuplicates(int[] nums) {
        // Mistake: Using O(N) extra space
        HashSet<Integer> set = new HashSet<>();
        
        for (int num : nums) {
            set.add(num);
        }
        
        // This fails the "O(1) extra memory" requirement of the problem
        int i = 0;
        for (int num : set) { 
            nums[i++] = num; 
        }
        
        // Note: Sets also don't guarantee order (unless LinkedHashSet is used), 
        // so this might even break the sorted order requirement!
        return set.size();
    }
}

```

**Why this is a mistake:**

* **Space Complexity:** It uses  space. The interviewer explicitly asks for .
* **Ignoring the "Sorted" Hint:** You aren't utilizing the fact that the array is sorted, which allows you to solve it much faster without a Set.

---

### 3. Optimized Solution (Java)

We use the **Two Pointer (Reader/Writer)** pattern again.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        // Edge case: empty array
        if (nums.length == 0) return 0;
        
        // 'insertPos' is our Writer pointer.
        // We start at index 1 because the first element (index 0) is always unique.
        int insertPos = 1;

        // 'i' is our Reader pointer. Start scanning from the second element.
        for (int i = 1; i < nums.length; i++) {
            
            // Compare current element with the previous element
            // If they are different, we found a new unique number.
            if (nums[i] != nums[i - 1]) {
                
                // Copy the unique number to the insert position
                nums[insertPos] = nums[i];
                
                // Move the insert position forward
                insertPos++;
            }
            // If they are the same, we simply skip 'i' and do nothing.
        }

        return insertPos;
    }
}

```

---

### 4. Explanation, Pattern & Approach

**Pattern: Two Pointers (Fast/Slow or Reader/Writer)**
Just like the previous problem, we have one pointer (`i`) scanning the array fast, and one pointer (`insertPos`) moving slowly, only advancing when we find valid data (a unique number).

**Approach Point-to-Point:**

1. **The "Sorted" Advantage:** Since the array is sorted, we don't need to check the whole array to see if a number is a duplicate. We only need to check the **neighbor immediately to the left**.
2. **Logic:**
* If `nums[i]` is exactly the same as `nums[i-1]`, it's a duplicate. Ignore it.
* If `nums[i]` is different from `nums[i-1]`, it is a **New Unique Number**.


3. **Execution:**
* We capture that new unique number and put it at our `insertPos`.
* We increment `insertPos` to get ready for the *next* unique number.


4. **Complexity:**
* **Time:**  — Single pass through the array.
* **Space:**  — No extra data structures used.



**Next Problem:**
The next one is **80. Remove Duplicates from Sorted Array II**.
This is a "Medium" level problem. It builds on exactly this logic but adds a tricky constraint: you are allowed duplicates, but **at most twice**.
