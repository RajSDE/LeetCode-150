**189. Rotate Array**.

### 1. The Question Explained (Simply)

You have an array of integers, and you need to shift every element to the right by `k` steps.

* **The "Wrap Around":** If an element is at the end of the array and gets pushed right, it circles back to the front (index 0).
* **Example:** `[1, 2, 3, 4, 5]` rotated by `k=2` becomes `[4, 5, 1, 2, 3]`.
* **Constraint:** You must do this **in-place** with  extra space.

---

### 2. Common Mistakes to Avoid

There are two common "trap" solutions here. One wastes time, the other wastes space.

**Mistake A: The "Brute Force" Shift ( Time)**
You shift the entire array by 1 step, and you repeat this process `k` times.

* **Why it fails:** If the array has 100,000 elements and `k` is 50,000, you are doing 5 billion operations. This will get a "Time Limit Exceeded" (TLE).

**Mistake B: The "Extra Array" ( Space)**
You create a new temporary array, calculate the new position for every element `(i + k) % n`, and copy them over.

* **Why it fails:** The problem strictly asks for ** space**. Allocating a whole new array violates this constraint.

```java
// Mistake B: Using Extra Space
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        int[] temp = new int[n]; // O(N) Space - Forbidden
        for (int i = 0; i < n; i++) {
            temp[(i + k) % n] = nums[i];
        }
        // Copy back
        for (int i = 0; i < n; i++) nums[i] = temp[i];
    }
}

```

---

### 3. Optimized Solution (Java)

We use the **Reversal Algorithm** (The "Magic Trick"). This solves it in  time and  space.

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        
        // CRITICAL STEP: Normalize k
        // If k is greater than array length (e.g., length 5, k=7), 
        // rotating 7 times is same as rotating 2 times (7 % 5 = 2).
        k = k % n;
        
        // Step 1: Reverse the entire array
        // [1, 2, 3, 4, 5] -> [5, 4, 3, 2, 1]
        reverse(nums, 0, n - 1);
        
        // Step 2: Reverse the first k elements
        // [5, 4, 3, 2, 1] -> [4, 5, 3, 2, 1]
        reverse(nums, 0, k - 1);
        
        // Step 3: Reverse the remaining elements (from k to end)
        // [4, 5, 3, 2, 1] -> [4, 5, 1, 2, 3]  <-- Result!
        reverse(nums, k, n - 1);
    }
    
    // Helper function to reverse a section of the array
    private void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}

```

---

### 4. Explanation, Pattern & Approach

**Pattern: Array Reversal**
This is a mathematical observation. If you reverse the whole list, the elements at the end move to the front (which is what we want). However, they are in the wrong order (reversed). By reversing the two separate sections (the part that wrapped around and the part that shifted), we restore the correct order.

**Approach Point-to-Point:**

1. **Normalize `k`:** Always do `k = k % n`. If `n=5` and `k=5`, rotation does nothing. If you don't do this, and `k > n`, your code will crash with an IndexOutOfBoundsException.
2. **The 3 Steps:**
* **Reverse All:** Brings the "tail" elements (which need to move to the front) to the front.
* **Reverse Head:** The elements we just brought to the front are backwards. Flip them to fix their order.
* **Reverse Tail:** The elements that were originally at the front (now at the back) are also backwards. Flip them to fix their order.


3. **Complexity:**
* **Time:** . We touch each element a constant number of times (2 times effectively).
* **Space:** . We just use a `temp` variable for swapping.



---

**Next Problem:**
The next problem is **121. Best Time to Buy and Sell Stock**.
This is the "Hello World" of Dynamic Programming (though we solve it greedily). It tests if you can track minimums and maximums efficiently in a single pass.
