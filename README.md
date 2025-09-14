# Article-The-Sliding-Window-Algorithm-and-When-We-Should-Use-It
When trying to solve problems optimally with the best time and space complexity possible, we may face challenges.

![Alt text](https://miro.medium.com/v2/resize:fit:720/format:webp/0*8kOBsduOiqDHVnfx.gif)

````markdown
# Sliding Window Algorithm Explained

Many coding problems involve manipulating **arrays** and **strings** — for example:  
- Finding a subarray inside an array that meets a condition  
- Finding a subsequence of letters inside a string  

At first, these problems may seem simple, but solving them **optimally** (with the best time and space complexity) can be challenging.  

Naive approaches often use nested loops or extra data structures, resulting in **O(n²)** time and extra space. This is inefficient for large inputs.  

---

## Solution: Sliding Window Technique

The **Sliding Window Algorithm** helps us:

✅ Reduce time complexity to **O(n)**  
✅ Avoid nested loops  
✅ Process subarrays and substrings more efficiently  

---

## What is a Sliding Window?

As the name suggests, the technique uses two pointers (often called `left` and `right`) to create a **window** that covers part of an array or string.  

Instead of checking all possible subarrays/substrings with nested loops, we **slide the window** across the data, adjusting its size or position depending on the problem’s requirements.

There are two main types:  
1. **Fixed Sliding Window**  
2. **Dynamic Sliding Window**

---

## Fixed Sliding Window

A fixed sliding window maintains a **constant length**.  
Example: find the maximum sum of any subarray of size `k`.

### Problem
```js
nums = [2, 1, 5, 1, 3, 2]
k = 3
// Maximum sum is 9, from subarray [5, 1, 3].
````

### Naive Solution (O(n²))

```js
function maxSum(nums, k) {
    let maxSum = -Infinity;

    for (let i = 0; i <= nums.length - k; i++) {
        let sum = 0;
        for (let j = i; j < i + k; j++) {
            sum += nums[j];
        }
        maxSum = Math.max(maxSum, sum);
    }

    return maxSum;
}

console.log(maxSum([2, 1, 5, 1, 3, 2], 3)); // Output: 9
```

Inefficient because it recalculates sums repeatedly.

---

### Optimized Solution (O(n))

Steps:

1. Compute the sum of the first window of size `k`.
2. Slide the window one step at a time.
3. Add the next element, subtract the element leaving the window.

```js
function maxSum(nums, k) {
    let maxSum = 0;
    let windowSum = 0;

    // Sum first window
    for (let i = 0; i < k; i++) {
        windowSum += nums[i];
    }
    maxSum = windowSum;

    // Slide window across array
    for (let i = k; i < nums.length; i++) {
        windowSum += nums[i] - nums[i - k];  // Add new, remove old
        maxSum = Math.max(maxSum, windowSum);
    }

    return maxSum;
}

console.log(maxSum([2, 1, 5, 1, 3, 2], 3)); // Output: 9
```

✔️ Runs in **O(n)** time
✔️ Uses **O(1)** extra space

---

## Dynamic Sliding Window

A dynamic sliding window’s size is **not fixed**.
It expands or shrinks depending on problem constraints (e.g., no duplicates, sum ≤ target, etc.).

### Example Problem

**Find the length of the longest substring without repeating characters.**

```js
s = "abcabcbb"
// Answer = 3 ("abc")
```

---

### Naive Solution (O(n²))

```js
function lengthOfLongestSubstring(s) {
    let max = 0;

    for (let i = 0; i < s.length; i++) {
        let seen = new Set();
        for (let j = i; j < s.length; j++) {
            if (seen.has(s[j])) break;
            seen.add(s[j]);
            max = Math.max(max, j - i + 1);
        }
    }

    return max;
}

console.log(lengthOfLongestSubstring("abcabcbb")); // Output: 3
```

---

### Optimized Solution (O(n))

Steps:

1. Use two pointers `left` and `right` to define the window.
2. Expand `right`, adding characters to a set.
3. If a duplicate is found, move `left` forward until valid again.
4. Track the maximum window length.

```js
function lengthOfLongestSubstring(s) {
    let set = new Set();
    let left = 0, maxLen = 0;

    for (let right = 0; right < s.length; right++) {
        while (set.has(s[right])) {
            set.delete(s[left]);
            left++;
        }
        set.add(s[right]);
        maxLen = Math.max(maxLen, right - left + 1);
    }

    return maxLen;
}

console.log(lengthOfLongestSubstring("abcabcbb")); // Output: 3
```

✔️ Each character is added/removed at most once → **O(n)**
✔️ Space complexity: **O(min(n, charset))**

---

## Why Sliding Window Works

* The window **expands** as long as constraints are satisfied.
* When constraints are violated, the window **shrinks** until valid.
* Each element is processed at most twice → efficient linear runtime.

---

## Conclusion

The Sliding Window is a **powerful optimization technique** that replaces nested loops with linear-time solutions.

* **Fixed Sliding Window**: constant-size problems (e.g., maximum sum of subarray size `k`).
* **Dynamic Sliding Window**: variable-size problems (e.g., longest substring without duplicates).

By mastering both, you can solve a wide variety of **array and string problems** efficiently:

* Maximum sums, averages → **Fixed Window**
* Longest substrings, smallest subarrays with constraints → **Dynamic Window**

👉 Both approaches typically achieve **O(n)** time with minimal extra space.

---

## Final Thoughts

Thanks for reading! 🚀
I hope you learned something new today, or at least refreshed your knowledge.
Let’s continue learning and growing together! ✨
