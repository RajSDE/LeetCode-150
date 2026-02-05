**122. Best Time to Buy and Sell Stock II**.

### 1. The Question Explained (Simply)

This is the sequel to the previous problem.

* **The Change:** You can now buy and sell **as many times as you want**.
* **The Constraint:** You can still only hold **one** share at a time (you must sell before you buy again).
* **The Nuance:** You can buy and sell on the *same day*.
* *Example:* If prices are `[1, 5, 30]`.
* Stock I Strategy: Buy 1, Sell 30. Profit = 29.
* Stock II Strategy: Buy 1, Sell 5 (Profit 4). Buy 5, Sell 30 (Profit 25). Total = 29.
* *Note:* The total profit is the same in a straight line up, but if there are dips, "Stock II" allows you to sell before the dip and buy back lower.



---

### 2. Common Mistake: The "Valley-Peak" Approach (Over-Complicated)

A common mistake is trying to explicitly find the "local minimum" (valley) and "local maximum" (peak) using `while` loops. While this is logically correct, the code becomes messy, hard to read, and prone to "Index Out of Bounds" errors.

**The Messy Code:**

```java
class Solution {
    public int maxProfit(int[] prices) {
        // Mistake: Writing complex logic to find exact valleys and peaks
        int i = 0;
        int profit = 0;
        int n = prices.length;
        
        while (i < n - 1) {
            // Find next valley (dip)
            while (i < n - 1 && prices[i] >= prices[i + 1]) i++;
            int valley = prices[i];
            
            // Find next peak (high)
            while (i < n - 1 && prices[i] <= prices[i + 1]) i++;
            int peak = prices[i];
            
            profit += peak - valley;
        }
        return profit;
    }
}

```

**Why this is a mistake:**

* **Readability:** It's hard to follow the `i` pointer inside multiple loops.
* **Bug Prone:** Easy to mess up the `i < n - 1` checks.
* **Unnecessary:** You don't need to find the *entire* slope. You just need the daily difference.

---

### 3. Optimized Solution (Java)

We use the **Simple Greedy (Slope Accumulation)** approach.

```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        
        // Start from day 1 (compare with day 0)
        for (int i = 1; i < prices.length; i++) {
            
            // IF today's price is higher than yesterday's...
            if (prices[i] > prices[i - 1]) {
                
                // ...we assume we bought yesterday and sold today.
                // We add this "daily profit" to our total.
                maxProfit += prices[i] - prices[i - 1];
            }
        }
        
        return maxProfit;
    }
}

```

---

### 4. Explanation, Pattern & Approach

**Pattern: Greedy / Cumulative Sum**
The logic here is simpler than it looks. Instead of looking for "Buy Low, Sell High" over a long period, we look for **"Is today better than yesterday?"**

**Approach Point-to-Point:**

1. **The Math Trick:**
* Suppose prices are `[1, 5, 10]`.
* One transaction: Buy 1, Sell 10. Profit = 9.
* Multiple transactions: (5 - 1) + (10 - 5) = 4 + 5 = 9.
* **Mathematical Conclusion:** The sum of segments equals the total distance.


2. **The Logic:**
* If the price curve goes **UP** (e.g., 1 -> 5), we capture that profit immediately.
* If the price curve goes **DOWN** (e.g., 5 -> 3), we do nothing (we simply don't buy yesterday).
* By adding up *every single upward slope*, we automatically capture the maximum possible profit from all trends.


3. **Complexity:**
* **Time:**  — Single pass.
* **Space:**  — No extra space.



---

**Next Problem:**
The next problem is **55. Jump Game**.
This is a very popular "Medium" problem that tests if you can track your "maximum reach" dynamically.