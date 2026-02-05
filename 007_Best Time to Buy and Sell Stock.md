---
layout: default
title: Best Time to Buy and Sell Stock
permalink: /problems/best-time-to-buy-and-sell-stock/
---

**121. Best Time to Buy and Sell Stock**.

### 1. The Question Explained (Simply)

You are a time traveler looking at a chart of stock prices for the next few days.

* **Input:** An array `prices` where `prices[i]` is the price of the stock on day `i`.
* **Goal:** You want to pick **one day** to buy and a **different day in the future** to sell.
* **Constraint:** You must maximize your profit. You cannot sell before you buy. If no profit is possible (prices only go down), return 0.

---

### 2. Common Mistake: The "Brute Force" Time Traveler ()

The most intuitive (but wrong) way to think is: "For every single day, let's check every future day to see how much I would make."

**The Slow Code:**

```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        // Outer loop: Buy Day
        for (int i = 0; i < prices.length; i++) {
            // Inner loop: Sell Day (must be after Buy Day)
            for (int j = i + 1; j < prices.length; j++) {
                int profit = prices[j] - prices[i];
                if (profit > maxProfit) {
                    maxProfit = profit;
                }
            }
        }
        return maxProfit;
    }
}

```

**Why this is a mistake:**

* **Time Complexity:** . If the stock history is 100,000 days long (common in test cases), this nested loop will execute billions of operations and time out.
* **Inefficiency:** You are recalculating comparisons you don't need. You don't need to check *every* pair; you only care about the *lowest* price relative to the current day.

---

### 3. Optimized Solution (Java)

We use a **Single Pass (Greedy)** approach.

```java
class Solution {
    public int maxProfit(int[] prices) {
        // Track the lowest price we have seen SO FAR. 
        // Start with a very high number so the first price becomes the minimum.
        int minPrice = Integer.MAX_VALUE;
        
        // Track the maximum profit we have found SO FAR.
        int maxProfit = 0;
        
        for (int price : prices) {
            // Case 1: Is this the cheapest price I've seen yet?
            if (price < minPrice) {
                minPrice = price;
            } 
            // Case 2: If I sold TODAY (using the minPrice I found earlier), 
            // is this the best profit so far?
            else if (price - minPrice > maxProfit) {
                maxProfit = price - minPrice;
            }
        }
        
        return maxProfit;
    }
}

```

---

### 4. Explanation, Pattern & Approach

**Pattern: Greedy / One Pass**
This is a classic "Greedy" algorithm. As we iterate through the array, we greedily make the best choice available at that moment based on the history we've seen.

**Approach Point-to-Point:**

1. **The Logic:** To make the most money, you want to buy at the **absolute lowest price** and sell at the highest price *after* that low.
2. **Visualizing the Loop:** Imagine walking through the days one by one. You only need to carry two pieces of information in your head:
* "What is the lowest price I have seen in the past?" (`minPrice`)
* "What is the most money I could have made up to today?" (`maxProfit`)


3. **Execution:**
* On Day 1: Price is 7. `minPrice` becomes 7. Profit is 0.
* On Day 2: Price is 1. `minPrice` becomes 1. (New low!). Profit is still 0 (can't sell yet).
* On Day 3: Price is 5. `minPrice` is still 1. Selling today gives `5 - 1 = 4` profit. `maxProfit` becomes 4.
* On Day 4: Price is 3. `minPrice` is 1. Selling today gives `3 - 1 = 2` profit. 2 is less than 4, so ignore it.


4. **Complexity:**
* **Time:**  — We iterate through the list exactly once.
* **Space:**  — Only two variables used.



---

**Next Problem:**
The next problem is **122. Best Time to Buy and Sell Stock II**.
This sounds similar, but the rules change significantly: You can buy and sell **as many times as you want**.