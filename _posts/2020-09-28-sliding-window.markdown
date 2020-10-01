---
layout: post
title:  "Sliding Window"
tags: array
---
## Index Map

[Longest Substring Without Repeating Characters][longest-substring-without-repeating-characters]

[Max Consecutive Ones III][max-consecutive-ones-iii]

{% highlight java %}
public int longestOnes(int[] A, int K) {
    int i = 0, j = 0, zero = 0;
    while (j < A.length) {
        if (A[j++] == 0) {
            zero++;
        }

        if (zero > K && A[i++] == 0) {
            zero--;
        }
    }
    return j - i;
}
{% endhighlight %}

From @lee215:

{% highlight java %}
public int longestOnes(int[] A, int K) {
    // finds the longest subarray with at most K zeros
    int i = 0, j = 0;
    while (j < A.length) {
        if (A[j] == 0) {
            K--;
        }

        // if K < 0, both i, j move forward together
        if (K < 0 && A[i++] == 0) {
            K++;
        }
        j++;
    }

    // i, j is a sliding window.
    // its span memorizes the max range so far
    return j - i;
}
{% endhighlight %}

[Fruit Into Baskets][fruit-into-baskets]

{% highlight java %}
public int totalFruit(int[] tree) {
    int[] type = new int[tree.length];
    int k = 2;
    int i = 0, j = 0, result = 0;
    while (j < tree.length) {
        if (type[tree[j]]++ == 0) {
            k--;
        }

        if (k < 0 && --type[tree[i++]] == 0) {
            k++;
        }
        j++;
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
        // the sliding window never shrinks, even if it covers an invalid substring
        // grows the window when the count of the new char exceeds the historical max count
        max = Math.max(max, ++count[s.charAt(j++) - 'A']);
        // window size is j - i + 1
        // right shifts the whole window by one
        if (j - i - max > k) {
            count[s.charAt(i++) - 'A']--;
        }
    }
    return j - i;
}
{% endhighlight %}

[Count Number of Nice Subarrays][count-number-of-nice-subarrays]

If we apply `nums[i] -> nums[i] % 2`, the problem becomes [Subarray Sum Equals K][subarray-sum-equals-k]

Another solution is by "three" pointers:

{% highlight java %}
public int numberOfSubarrays(int[] nums, int k) {
    int i = 0, j = 0, count = 0, result = 0;
    while (j < nums.length) {
        if (nums[j] % 2 == 1) {
            k--;
            count = 0;
        }

        while (k == 0) {
            k += nums[i++] % 2;
            count++;
        }

        // i moves forward by `count` steps in the last move
        result += count;
        j++;
    }

    return result;
}
{% endhighlight %}

## Minimum Length

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

## At Most K Different Elements

[Subarrays with K Different Integers][subarrays-with-k-different-integers]

{% highlight java %}
public int subarraysWithKDistinct(int[] A, int K) {
    return atMost(A, K) - atMost(A, K - 1);
}

private int atMost(int[] A, int K) {
    int[] count = new int[A.length + 1];
    int i = 0, j = 0, result = 0;
    while (j < A.length) {
        if (count[A[j]] == 0) {
            K--;
        }
        count[A[j]]++;

        while (K < 0) {
            if (--count[A[i++]] == 0) {
                K++;
            }
        }

        // (j - i + 1) is the length of each valid contiguous subarray with at most K different integers
        // Fomula: given an array of length n, it will produce (n * (n + 1)) / 2 total contiguous subarrays
        result += j - i + 1;
        j++;
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
        k -= nums[j] % 2;

        while (k < 0) {
            k += nums[i++] % 2;
        }

        result += j - i + 1;
        j++;
    }

    return result;
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

[count-number-of-nice-subarrays]: https://leetcode.com/problems/count-number-of-nice-subarrays/
[find-k-th-smallest-pair-distance]: https://leetcode.com/problems/find-k-th-smallest-pair-distance/
[fruit-into-baskets]: https://leetcode.com/problems/fruit-into-baskets/
[longest-repeating-character-replacement]: https://leetcode.com/problems/longest-repeating-character-replacement/
[longest-substring-without-repeating-characters]: https://leetcode.com/problems/longest-substring-without-repeating-characters/
[max-consecutive-ones-iii]: https://leetcode.com/problems/max-consecutive-ones-iii/
[minimum-size-subarray-sum]: https://leetcode.com/problems/minimum-size-subarray-sum/
[number-of-substrings-containing-all-three-characters]: https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/
[replace-the-substring-for-balanced-string]: https://leetcode.com/problems/replace-the-substring-for-balanced-string/
[subarray-sum-equals-k]: https://leetcode.com/problems/subarray-sum-equals-k/
[subarrays-with-k-different-integers]: https://leetcode.com/problems/subarrays-with-k-different-integers/