---
layout: post
title:  "Two Pointers"
tags: array
---
[Remove All Adjacent Duplicates in String II][remove-all-adjacent-duplicates-in-string-ii]

{% highlight java %}
public String removeDuplicates(String s, int k) {
    int n = s.length();
    // count[i]: number of duplicates ending at i
    int[] count = new int[n];

    char[] c = s.toCharArray();
    int i = 0, j = 0;
    while (j < n) {
        // copies the j-th char to the i-th position
        c[i] = c[j];

        count[i] = i > 0 && c[i - 1] == c[i] ? count[i - 1] + 1 : 1;
        if (count[i] == k) {
            i -= k;
        }

        i++;
        j++;
    }

    return new String(c, 0, i);
}
{% endhighlight %}

Another solution is to use stack.

[Backspace String Compare][backspace-string-compare]

Scan from the end of the Strings.

{% highlight java %}
public boolean backspaceCompare(String S, String T) {
    int i = S.length() - 1, j = T.length() - 1;
    int back = 0;
    while (true) {
        back = 0;
        while (i >= 0 && (back > 0 || S.charAt(i) == '#')) {
            back += S.charAt(i) == '#' ? 1 : -1;
            i--;
        }
        back = 0;
        while (j >= 0 && (back > 0 || T.charAt(j) == '#')) {
            back += T.charAt(j) == '#' ? 1 : -1;
            j--;
        }
        if (i >= 0 && j >= 0 && S.charAt(i) == T.charAt(j)) {
            i--;
            j--;
        } else {
            break;
        }
    }
    return i == -1 && j == -1;
}
{% endhighlight %}

[Trapping Rain Water][trapping-rain-water]

{% highlight java %}
public int trap(int[] height) {
    int water = 0, leftMax = 0, rightMax = 0;
    int left = 0, right = height.length - 1;
    while (left < right) {
        // moves pointer of one side if its height is lower
        // this ensures left and right meet at max height
        // in this way, we avoid one pass to find the max height
        if (height[left] < height[right]) {
            leftMax = Math.max(leftMax, height[left]);
            water += leftMax - height[left++];
        } else {
            rightMax = Math.max(rightMax, height[right]);
            water += rightMax - height[right--];
        }
    }

    return water;
}
{% endhighlight %}

[Container With Most Water][container-with-most-water]

{% highlight java %}
public int maxArea(int[] height) {
    int area = 0, left = 0, right = height.length - 1;
    while (left < right) {
        area = Math.max(area, Math.min(height[left], height[right]) * (right - left));
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    return area;
}
{% endhighlight %}

[Number of Subsequences That Satisfy the Given Sum Condition][number-of-subsequences-that-satisfy-the-given-sum-condition]

[Modular arithmetic properties](https://en.wikipedia.org/wiki/Modular_arithmetic#Properties)

{% highlight java %}
private int MOD = (int)1e9 + 7;

public int numSubseq(int[] nums, int target) {
    Arrays.sort(nums);

    // (A * B) mod C = (A mod C * B mod C) mod C
    int[] pows = new int[nums.length];
    pows[0] = 1;
    for (int k = 1; k < nums.length; k++) {
        pows[k] = pows[k - 1] * 2 % MOD;
    }

    int i = 0, j = nums.length - 1, count = 0;
    while (i <= j) {
        if (nums[i] + nums[j] > target) {
            j--;
        } else {
            count = (count + pows[j - i]) % MOD;
            i++;
        }
    }
    return count;
}
{% endhighlight %}

[Shortest Word Distance II][shortest-word-distance-ii]

{% highlight java %}
private Map<String, List<Integer>> map;

public WordDistance(String[] words) {
    map = new HashMap<>();
    for (int i = 0; i < words.length; i++) {
        map.computeIfAbsent(words[i], k -> new ArrayList<>()).add(i);
    }
}

public int shortest(String word1, String word2) {
    int i = 0, j = 0, min = Integer.MAX_VALUE;
    List<Integer> list1 = map.get(word1), list2 = map.get(word2);
    while (i < list1.size() && j < list2.size()) {
        int index1 = list1.get(i), index2 = list2.get(j);
        min = Math.min(min, Math.abs(index1 - index2));
        if (index1 < index2) {
            i++;
        } else {
            j++;
        }
    }
    return min;
}
{% endhighlight %}

# Unique Characters

[Count Unique Characters of All Substrings of a Given String][count-unique-characters-of-all-substrings-of-a-given-string]

{% highlight java %}
private final int MOD = (int)1e9 + 7;

public int uniqueLetterString(String s) {
    int[][] index = new int[26][2];  // last two indexes of each character
    for (int i = 0; i < index.length; i++) {
        Arrays.fill(index[i], -1);
    }

    int sum = 0;
    for (int i = 0; i < s.length(); i++) {
        int c = s.charAt(i) - 'A';
        // e.g. AxxAxxxA: 2 * 3
        // index[c][0] --> index[c][1] --> i
        sum = (sum + (i - index[c][1]) * (index[c][1] - index[c][0]) % MOD) % MOD;
        index[c][0] = index[c][1];
        index[c][1] = i;
    }

    // calculates index[c][1] --> s.length()
    for (int c = 0; c < index.length; c++) {
        sum = (sum + (s.length() - index[c][1]) * (index[c][1] - index[c][0]) % MOD) % MOD;
    }
    return sum;
}
{% endhighlight %}

[Count Substrings That Differ by One Character][count-substrings-that-differ-by-one-character]

{% highlight java %}
public int countSubstrings(String s, String t) {
    int count = 0 ;
    // each unmatched pair (i, j) is counted once and only once
    // e.g.
    //  - (s[2], t[2]) is in the first loop
    //  - (s[2], t[5]) is in the first loop
    //  - (s[2], t[1]) is in the second loop
    for (int i = 0; i < s.length(); i++) {
        count += helper(s, t, i, 0);
    }

    for (int j = 1; j < t.length(); j++) {
        count += helper(s, t, 0, j);
    }
    return count;
}

// i and j are starting indexes of s and t respectively
private int helper(String s, String t, int i, int j) {
    int count = 0, prev = 0, curr = 0;
    while (i < s.length() && j < t.length()) {
        curr++;
        // e.g. UMMUMMMMU
        // the number of substrings which contains the middle 'U' as the only unmatched char
        // == (2 + 1) * (4 + 1) == 15
        if (s.charAt(i++) != t.charAt(j++)) {
            prev = curr;
            curr = 0;
        }
        count += prev;
    }
    return count;
}
{% endhighlight %}

[Sort Transformed Array][sort-transformed-array]

[One Edit Distance][one-edit-distance]

{% highlight java %}
public boolean isOneEditDistance(String s, String t) {
    if (s.equals(t) || Math.abs(s.length() - t.length()) > 1) {
        return false;
    }

    int i = 0, j = 0;
    boolean hasDiff = false;
    while (i < s.length() && j < t.length()) {
        if (s.charAt(i) != t.charAt(j)) {
            if (hasDiff) {
                return false;
            }
            hasDiff = true;
            if (s.length() > t.length()) {
                j--;
            } else if (s.length() < t.length()) {
                i--;
            }
        }
        i++;
        j++;
    }
    return true;
}
{% endhighlight %}

[Maximum Score of a Good Subarray][maximum-score-of-a-good-subarray]

{% highlight java %}
public int maximumScore(int[] nums, int k) {
    int n = nums.length, i = k, j = k;
    int score = nums[k], min = nums[k];
    while (i > 0 || j < n - 1) {
        if (i == 0) {
            j++;
        } else if (j == n - 1) {
            i--;
        } else if (nums[i - 1] < nums[j + 1]) {
            // invariant:
            // the current subarray always has the highest score
            // among all subarrays of the same size
            j++;
        } else {
            i--;
        }
        min = Math.min(min, Math.min(nums[i], nums[j]));
        score = Math.max(score, min * (j - i + 1));
    }
    return score;
}
{% endhighlight %}

# Three Pointers

[Intersection of Three Sorted Arrays][intersection-of-three-sorted-arrays]

{% highlight java %}
if (arr1[p1] == arr2[p2] && arr2[p2] == arr3[p3]) {
    list.add(arr1[p1]);
    p1++;
    p2++;
    p3++;
} else {
    if (arr1[p1] < arr2[p2]) {
        p1++;
    } else if (arr2[p2] < arr3[p3]) {
        p2++;
    } else {  // arr1[p1] >= arr2[p2] && arr2[p2] >= arr3[p3]
        p3++;
    }
}
{% endhighlight %}

# Two Passes

[Push Dominoes][push-dominoes]

{% highlight java %}
public String pushDominoes(String dominoes) {
    char[] chars = dominoes.toCharArray();
    int n = chars.length;

    int[] forces = new int[n];
    int force = 0;

    // -> right
    for (int i = 0; i < n; i++) {
        if (chars[i] == 'R'){
            force = n;
        } else if (chars[i] == 'L') {
            force = 0;
        } else {
            force = Math.max(force - 1, 0);
        }
        forces[i] += force;
    }

    // <- left
    force = 0;
    for (int i = n - 1; i >= 0; i--) {
        if (chars[i] == 'L') {
            force = n;
        } else if (chars[i] == 'R') {
            force = 0;
        } else {
            force = Math.max(force - 1, 0);
        }
        forces[i] -= force;

        chars[i] = forces[i] < 0 ? 'L' : (forces[i] > 0 ? 'R' : '.');
    }
    return new String(chars);
}
{% endhighlight %}

```
.L.R...LR..L..

[  0,  0,0,14, 13, 12, 11,  0,14, 13, 12,  0,0,0]
[-13,-14,0, 0,-11,-12,-13,-14, 0,-12,-13,-14,0,0]

[-13,-14,0,14,  2,  0, -2,-14,14,  1, -1,-14,0,0]

LL.RR.LLRRLL..
```

[Get the Maximum Score][get-the-maximum-score]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int maxSum(int[] nums1, int[] nums2) {
    int i = 0, j = 0, n = nums1.length, m = nums2.length;
    // sum of elements between the knots (comment elements) on each path
    long dp1 = 0, dp2 = 0;
    while (i < n || j < m) {
        if (i < n && (j == m || nums1[i] < nums2[j])) {
            dp1 += nums1[i++];
        } else if (j < m && (i == n || nums1[i] > nums2[j])) {
            dp2 += nums2[j++];
        } else {
            // common elements
            // resets both sums to the same value
            dp1 = dp2 = Math.max(dp1, dp2) + nums1[i];
            i++;
            j++;
        }
    }
    return (int)(Math.max(dp1, dp2) % MOD);
}
{% endhighlight %}

[backspace-string-compare]: https://leetcode.com/problems/backspace-string-compare/
[container-with-most-water]: https://leetcode.com/problems/container-with-most-water/
[count-substrings-that-differ-by-one-character]: https://leetcode.com/problems/count-substrings-that-differ-by-one-character/
[count-unique-characters-of-all-substrings-of-a-given-string]: https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/
[get-the-maximum-score]: https://leetcode.com/problems/get-the-maximum-score/
[intersection-of-three-sorted-arrays]: https://leetcode.com/problems/intersection-of-three-sorted-arrays/
[maximum-score-of-a-good-subarray]: https://leetcode.com/problems/maximum-score-of-a-good-subarray/
[number-of-subsequences-that-satisfy-the-given-sum-condition]: https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/
[one-edit-distance]: https://leetcode.com/problems/one-edit-distance/
[push-dominoes]: https://leetcode.com/problems/push-dominoes/
[remove-all-adjacent-duplicates-in-string-ii]: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/
[shortest-word-distance-ii]: https://leetcode.com/problems/shortest-word-distance-ii/
[sort-transformed-array]: https://leetcode.com/problems/sort-transformed-array/
[trapping-rain-water]: https://leetcode.com/problems/trapping-rain-water/
