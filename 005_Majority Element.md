---
layout: default
title: Majority Element
permalink: /problems/majority-element/
---

**169. Majority Element**.

### 1. The Question Explained (Simply)

You are given an array of size `n`. You need to find the "Majority Element."

* **Definition:** The Majority Element is the number that appears **more than** `n / 2` times.
* **The Guarantee:** The problem guarantees that a majority element *always* exists in the array.
* **The Challenge:** Find this number in  time and—crucially—** space**.
* **Analogy:** Imagine a voting booth. If one candidate has more than 50% of the total votes, they win. You need to identify the winner without storing every single ballot in a massive pile (memory).

---

### 2. Common Mistake: The HashMap Approach (Space Inefficient)

The most common instinct is to count the frequency of every number using a HashMap. While this is  in time, it requires  space to store the map. In a system-level interview, allocating memory linear to the input size when you don't have to is a "fail" on optimization.

**The "Memory Heavy" Code:**

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int majorityElement(int[] nums) {
        // Mistake: Allocating O(N) memory for frequency map
        Map<Integer, Integer> counts = new HashMap<>();
        int n = nums.length;
        
        for (int num : nums) {
            counts.put(num, counts.getOrDefault(num, 0) + 1);
            
            // Checking immediately if it crosses the threshold
            if (counts.get(num) > n / 2) {
                return num;
            }
        }
        return -1; // Should not happen based on problem constraints
    }
}

```

**Why this is a mistake:**

* **Space Complexity:** . If the input array is 1 million integers, you create a HashMap of significant size.
* **Overhead:** HashMaps have overhead (hashing function, collision handling, object wrapper classes like `Integer`).

---

### 3. Optimized Solution (Java)

We use the **Boyer-Moore Voting Algorithm**. This is the standard "Coder" way to solve this problem.

```java
class Solution {
    public int majorityElement(int[] nums) {
        // 'candidate' tracks the number we currently think is the majority
        int candidate = 0;
        // 'count' tracks the "strength" of that candidate
        int count = 0;
        
        for (int num : nums) {
            // 1. If the count is 0, the previous candidate has been completely "cancelled out".
            // We pick the current number as the new candidate.
            if (count == 0) {
                candidate = num;
            }
            
            // 2. If the current number matches our candidate, we strengthen it (vote up).
            if (num == candidate) {
                count++;
            } 
            // 3. If it's different, it "cancels out" one vote for the candidate (vote down).
            else {
                count--;
            }
        }
        
        return candidate;
    }
}

```

---

### 4. Explanation, Pattern & Approach

**Pattern: Boyer-Moore Voting Algorithm**
This algorithm relies on the logic of "cancellation."
Since the majority element appears **more than** `n / 2` times, it appears more times than *all other elements combined*.
If you pair up every majority element with a non-majority element and cancel them both out, the majority element will still remain standing at the end.

**Approach Point-to-Point:**

1. **The War of Attrition:** Think of this as a battlefield. The `candidate` is the army currently holding the hill. The `count` is the number of soldiers they have.
2. **Logic:**
* If you encounter a soldier from the **same** army (`num == candidate`), your army grows (`count++`).
* If you encounter a soldier from an **enemy** army (`num != candidate`), they kill one of your soldiers (`count--`).


3. **The Switch:**
* If `count` drops to 0, your army is wiped out. The *very next* number you see claims the empty hill and becomes the new `candidate`.


4. **Why it works:**
* Because the majority element has  occurrences, it is mathematically impossible to cancel all of them out. It will always be the last `candidate` standing with a `count > 0`.


5. **Complexity:**
* **Time:**  — Single pass.
* **Space:**  — Only two integer variables used.



**Next Problem:**
The next problem is **189. Rotate Array**.
This is a very famous problem that involves a "Magic Trick" involving reversing sections of the array.