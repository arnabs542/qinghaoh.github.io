---
layout: post
title:  "Sliding Window"
tags: array
---
## Index Map

[Longest Substring Without Repeating Characters][longest-substring-without-repeating-characters]

## At Most

### Max Length

#### Template

{% highlight java %}
/**
 * Finds the length of the longest subarray that contains at most k elements
 * that match a given condition.
 */
public int maxLength(int[] nums, int k) {
    int i = 0, j = 0;
    
    // the sliding window never shrinks
    // even if it doesn't meet the requirement at a certain moment
    while (j < nums.length) {
        if (updateCounters(nums, j++)) {
            k--;
        }

        // if k < 0, both i, j move forward together
        // i.e. right shifts
        if (k < 0 && updateCounters(nums, i++)) {
            k++;
        }
    }

    // [i, j) is a sliding window.
    // its span memorizes the max range so far
    return j - i;
}

/**
 * There can be multiple counters to track status.
 * Updates the counters based on the index.
 * @return true if nums[index] matches the given condition, otherwise false
 */
private boolean updateCounters(int[] nums, int index) {
    update();
    return condition(nums[index]);
}
{% endhighlight %}

[Max Consecutive Ones III][max-consecutive-ones-iii]

{% highlight java %}
public int longestOnes(int[] A, int K) {
    int i = 0, j = 0;
    while (j < A.length) {
        if (A[j++] == 0) {
            K--;
        }

        if (K < 0 && A[i++] == 0) {
            K++;
        }
    }
    return j - i;
}
{% endhighlight %}

[Get Equal Substrings Within Budget][get-equal-substrings-within-budget]

[Fruit Into Baskets][fruit-into-baskets]

{% highlight java %}
public int totalFruit(int[] tree) {
    int[] type = new int[tree.length];
    int k = 2;
    int i = 0, j = 0;
    while (j < tree.length) {
        if (type[tree[j++]]++ == 0) {
            k--;
        }

        if (k < 0 && --type[tree[i++]] == 0) {
            k++;
        }
    }
    return j - i;
}
{% endhighlight %}

[Longest Repeating Character Replacement][longest-repeating-character-replacement]

{% highlight java %}
public int characterReplacement(String s, int k) {
    int[] count = new int[26];
    int i = 0, j = 0, max = 0;
    while (j < s.length()) {
        // grows the window when the count of the new char exceeds the historical max count
        max = Math.max(max, ++count[s.charAt(j++) - 'A']);

        // count of other chars == j - i - max
        if (j - i - max > k) {
            count[s.charAt(i++) - 'A']--;
        }
    }
    return j - i;
}
{% endhighlight %}

[Longest Substring with At Most K Distinct Characters][longest-substring-with-at-most-k-distinct-characters]

[Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit][longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit]

{% highlight java %}
public int longestSubarray(int[] nums, int limit) {
    Deque<Integer> maxd = new ArrayDeque<>(), mind = new ArrayDeque<>();
    int i = 0, j = 0;
    while (j < nums.length) {
        while (!maxd.isEmpty() && maxd.peekLast() < nums[j]) {
            maxd.pollLast();
        }
        maxd.add(nums[j]);

        while (!mind.isEmpty() && mind.peekLast() > nums[j]) {
            mind.pollLast();
        }
        mind.add(nums[j]);

        j++;

        if (maxd.peek() - mind.peek() > limit) {
            if (maxd.peek() == nums[i]) {
                maxd.poll();
            }
            if (mind.peek() == nums[i]) {
                mind.poll();
            }
            i++;
        }
    }
    return j - i;
}
{% endhighlight %}

## At Most K Different Elements

### Count

#### Template

{% highlight java %}
/**
 * Finds the count of the subarrays that contain at most k elements
 * that match a given condition.
 */
public int atMost(int[] nums, int k) {
    int i = 0, j = 0, count = 0;

    // the sliding window never shrinks
    // even if it doesn't meet the requirement at a certain moment
    while (j < nums.length) {
        if (updateCounters(nums, j++)) {
            k--;
        }

        while (k < 0) {
            k += updateCounters(nums, i++));
        }

        // (j - i) is the length of each valid contiguous subarray with at most K different integers
        // Fomula: given an array of length n, it will produce (n * (n + 1)) / 2 total contiguous subarrays
        count += j - i;
    }

    // [i, j) is a sliding window.
    return count;
}

/**
 * There can be multiple counters to track status.
 * Updates the counters based on the index.
 * @return
 */
private int updateCounters(int[] nums, int index) {
}
{% endhighlight %}

[Subarrays with K Different Integers][subarrays-with-k-different-integers]

{% highlight java %}
public int subarraysWithKDistinct(int[] A, int K) {
    return atMost(A, K) - atMost(A, K - 1);
}

private int atMost(int[] A, int K) {
    int[] count = new int[A.length + 1];
    int i = 0, j = 0, result = 0;
    while (j < A.length) {
        if (count[A[j++]]++ == 0) {
            K--;
        }

        while (K < 0) {
            if (--count[A[i++]] == 0) {
                K++;
            }
        }

        result += j - i;
    }
    return result;
}
{% endhighlight %}

[Count Number of Nice Subarrays][count-number-of-nice-subarrays]

{% highlight java %}
public int numberOfSubarrays(int[] nums, int k) {
    return atMost(nums, k) - atMost(nums, k - 1);
}

private int atMost(int[] nums, int k) {
    int i = 0, j = 0, result = 0;
    while (j < nums.length) {
        k -= nums[j++] % 2;

        while (k < 0) {
            k += nums[i++] % 2;
        }

        result += j - i;
    }

    return result;
}
{% endhighlight %}

If we apply `nums[i] -> nums[i] % 2`, the problem becomes [Subarray Sum Equals K][subarray-sum-equals-k]

Another solution is by "three" pointers:

{% highlight java %}
public int numberOfSubarrays(int[] nums, int k) {
    int i = 0, j = 0, count = 0, result = 0;
    while (j < nums.length) {
        if (nums[j++] % 2 == 1) {
            k--;
            count = 0;
        }

        while (k == 0) {
            k += nums[i++] % 2;
            count++;
        }

        // i moves forward by `count` steps in the last move
        result += count;
    }

    return result;
}
{% endhighlight %}

# At Most

[Subarray Product Less Than K][subarray-product-less-than-k]

{% highlight java %}
public int numSubarrayProductLessThanK(int[] nums, int k) {
    if (k <= 1) {
        return 0;
    }

    int i = 0, j = 0, prod = 1, count = 0;
    while (j < nums.length) {
        prod *= nums[j++];
        while (prod >= k) {
            prod /= nums[i++];
        }
        count += j - i;
    }
    return count;
}
{% endhighlight %}

[Find K-th Smallest Pair Distance][find-k-th-smallest-pair-distance]

{% highlight java %}
public int smallestDistancePair(int[] nums, int k) {
    Arrays.sort(nums);

    int low = 0, high = nums[nums.length - 1] - nums[0];
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (condition(nums, mid, k)) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return low;
}

private boolean condition(int[] nums, int distance, int k) {            
    int i = 0, j = 0, count = 0;
    while (j < nums.length) {
        while (nums[j] - nums[i] > distance) {
            i++;
        }
        count += j - i;
        j++;
    }
    return count >= k;
}
{% endhighlight %}

## Min Length

[Minimum Size Subarray Sum][minimum-size-subarray-sum]

{% highlight java %}
public int minSubArrayLen(int s, int[] nums) {
    int i = 0, j = 0, min = nums.length + 1;
    while (j < nums.length) {
        s -= nums[j++];

        while (s <= 0) {
            min = Math.min(min, j - i);
            s += nums[i++];
        }
    }

    return min == nums.length + 1 ? 0 : min;
}
{% endhighlight %}

[Replace the Substring for Balanced String][replace-the-substring-for-balanced-string]

{% highlight java %}
public int balancedString(String s) {
    int[] count = new int[26];
    for (char c : s.toCharArray()) {
        count[c - 'A']++;
    }

    int i = 0, j = 0, min = s.length(), k = s.length() / 4;
    while (j < s.length()) {
        count[s.charAt(j++) - 'A']--;

        while (i < s.length() && condition(count, k)) {
            min = Math.min(min, j - i);
            count[s.charAt(i++) - 'A']++;
        }
    }
    return min;
}

private boolean condition(int[] count, int k) {
    for (char c : "QWER".toCharArray()) {
        if (count[c - 'A'] > k) {
            return false;
        }
    }
    return true;
}
{% endhighlight %}

[Minimum Window Substring][minimum-window-substring]

{% highlight java %}
public String minWindow(String s, String t) {
    int[] count = new int[256];
    for (char c : t.toCharArray()) {
        count[c]++;
    }

    int i = 0, j = 0, k = t.length(), min = s.length();
    String window = "";
    while (j < s.length()) {
        if (count[s.charAt(j++)]-- > 0) {
            k--;
        }

        while (k == 0) {
            if (j - i <= min) {
                window = s.substring(i, j);
                min = j - i;
            }
            if (count[s.charAt(i++)]++ == 0) {
                k++;
            }
        }
    }
    return window;
}
{% endhighlight %}

## At Least

[Number of Substrings Containing All Three Characters][number-of-substrings-containing-all-three-characters]

{% highlight java %}
public int numberOfSubstrings(String s) {
    int k = 3;
    int[] count = new int[k];
    int i = 0, j = 0, result = 0;
    while (j < s.length()) {
        if (count[s.charAt(j++) - 'a']++ == 0) {
            k--;
        }

        while (k == 0) {
            if (--count[s.charAt(i++) - 'a'] == 0) {
                k++;
            }
        }
        result += i;
    }
    return result;
}
{% endhighlight %}

# Fixed Size

[Maximum Points You Can Obtain from Cards][maximum-points-you-can-obtain-from-cards]

[Minimum Difference Between Largest and Smallest Value in Three Moves][minimum-difference-between-largest-and-smallest-value-in-three-moves]

[Minimum Swaps to Group All 1's Together][minimum-swaps-to-group-all-1s-together]

{% highlight java %}
public int minSwaps(int[] data) {
    int sum = 0;
    for (int d : data) {
        sum += d;
    }

    int i = 0, j = 0, count = 0, min = data.length;
    while (j < data.length) {
        count += data[j++];
        if (j - i >= sum) {
            min = Math.min(min, sum - count);
            count -= data[i++];
        }
    }
    return min;
}
{% endhighlight %}

[Find All Anagrams in a String][find-all-anagrams-in-a-string]

{% highlight java %}
public List<Integer> findAnagrams(String s, String p) {
    int[] count = new int[26];
    for (char c : p.toCharArray()) {
        count[c - 'a']++;
    }

    List<Integer> list = new ArrayList<>();
    int i = 0, j = 0, k = p.length();
    while (j < s.length()) {
        if (count[s.charAt(j++) - 'a']-- > 0) {
            k--; 
        }

        if (k == 0) {
            list.add(i);
        }

        // count of chars in p won't go below 0
        if (j - i == p.length() && count[s.charAt(i++) - 'a']++ >= 0) {
            k++;
        }
    }
    return list;
}
{% endhighlight %}

[K Empty Slots][k-empty-slots]

{% highlight java %}
public int kEmptySlots(int[] bulbs, int k) {
    int n = bulbs.length;
    // days[i]: the day when bulbs[i] is turned on (1-indexed)
    int[] days =  new int[n];
    for (int i = 0; i < n; i++) {
        days[bulbs[i] - 1] = i + 1;
    }

    // sliding window
    // the goal is find a fixed window in days, whose length is (k + 2)
    // and for all indexes in between (exclusively), days[index] > endpoints
    int min = Integer.MAX_VALUE;
    int left = 0, right = k + 1;
    for (int i = 0; right < n; i++) {
        // current days[i] is valid, continue
        if (days[i] > days[left] && days[i] > days[right]) {
            continue;
        }

        // reaches the right endpoint
        // since all previous number are valid, this is a candidate minimum
        if (i == right) {
            min = Math.min(min, Math.max(days[left], days[right]));
        }

        // not valid, slides the window
        left = i;
        right = k + 1 + i;
    }
    return min == Integer.MAX_VALUE ? -1 : min;
}
{% endhighlight %}

[Minimum Number of Flips to Make the Binary String Alternating][minimum-number-of-flips-to-make-the-binary-string-alternating]

{% highlight java %}
public int minFlips(String s) {
    // sliding window
    // cyclic problem: s += s
    int n = s.length();
    // flips needed to become "0101..." and "1010..." respectively
    int flips0 = 0, flips1 = 0;
    int flips = n;

    for (int i = 0; i < 2 * n; i++) {
        // the expected char at i-th index of "0101..."
        char c = (char)('0' + i % 2);

        if (c != s.charAt(i % n)) {
            flips0++;
        } else {
            flips1++;
        }

        // i is the end of the window
        if (i >= n) {
            // i % n is outside of the window
            // decrements if it was flipped before
            c = (char)('0' + (i % n) % 2);

            if (c != s.charAt(i % n)) {
                flips0--;
            } else {
                flips1--;
            }

            flips = Math.min(flips, Math.min(flips0, flips1));
        }
    }
    return flips;
}
{% endhighlight %}

## Exact

[Minimum Operations to Reduce X to Zero][minimum-operations-to-reduce-x-to-zero]

{% highlight java %}
public int minOperations(int[] nums, int x) {
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }

    int i = 0, j = 0, min = Integer.MAX_VALUE;
    while (j < nums.length) {
        // sum([0...i) + (j...n - 1]) == x
        sum -= nums[j++];

        while (sum < x && i < j) {
            sum += nums[i++];
        }

        if (sum == x) {
            min = Math.min(min, nums.length - j + i);
        }
    }
    return min == Integer.MAX_VALUE ? -1 : min;
}
{% endhighlight %}

Similar to: [Maximum Size Subarray Sum Equals k][maximum-size-subarray-sum-equals-k]

[count-number-of-nice-subarrays]: https://leetcode.com/problems/count-number-of-nice-subarrays/
[find-all-anagrams-in-a-string]: https://leetcode.com/problems/find-all-anagrams-in-a-string/
[find-k-th-smallest-pair-distance]: https://leetcode.com/problems/find-k-th-smallest-pair-distance/
[fruit-into-baskets]: https://leetcode.com/problems/fruit-into-baskets/
[get-equal-substrings-within-budget]: https://leetcode.com/problems/get-equal-substrings-within-budget/
[k-empty-slots]: https://leetcode.com/problems/k-empty-slots/
[longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit]: https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/
[longest-repeating-character-replacement]: https://leetcode.com/problems/longest-repeating-character-replacement/
[longest-substring-with-at-most-k-distinct-characters]: https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/
[longest-substring-without-repeating-characters]: https://leetcode.com/problems/longest-substring-without-repeating-characters/
[max-consecutive-ones-iii]: https://leetcode.com/problems/max-consecutive-ones-iii/
[maximum-points-you-can-obtain-from-cards]: https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/
[maximum-size-subarray-sum-equals-k]: https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/
[minimum-difference-between-largest-and-smallest-value-in-three-moves]: https://leetcode.com/problems/minimum-difference-between-largest-and-smallest-value-in-three-moves/
[minimum-number-of-flips-to-make-the-binary-string-alternating]: https://leetcode.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating/
[minimum-operations-to-reduce-x-to-zero]: https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/
[minimum-size-subarray-sum]: https://leetcode.com/problems/minimum-size-subarray-sum/
[minimum-swaps-to-group-all-1s-together]: https://leetcode.com/problems/minimum-swaps-to-group-all-1s-together/
[minimum-window-substring]: https://leetcode.com/problems/minimum-window-substring/
[number-of-substrings-containing-all-three-characters]: https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/
[replace-the-substring-for-balanced-string]: https://leetcode.com/problems/replace-the-substring-for-balanced-string/
[subarray-product-less-than-k]: https://leetcode.com/problems/subarray-product-less-than-k/submissions/
[subarray-sum-equals-k]: https://leetcode.com/problems/subarray-sum-equals-k/
[subarrays-with-k-different-integers]: https://leetcode.com/problems/subarrays-with-k-different-integers/
