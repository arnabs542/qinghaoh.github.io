---
layout: post
title:  "Two Pointers"
tags: array
---
[Remove All Adjacent Duplicates in String II][remove-all-adjacent-duplicates-in-string-ii]

{% highlight java %}
public String removeDuplicates(String s, int k) {
    char[] c = s.toCharArray();
    int[] count = new int[s.length()];  // DP
    int i = 0, j = 0;
    while (j < c.length) {
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

[][]

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

[Sort Transformed Array][sort-transformed-array]

[backspace-string-compare]: https://leetcode.com/problems/backspace-string-compare/
[container-with-most-water]: https://leetcode.com/problems/container-with-most-water/
[number-of-subsequences-that-satisfy-the-given-sum-condition]: https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/
[remove-all-adjacent-duplicates-in-string-ii]: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/
[shortest-word-distance-ii]: https://leetcode.com/problems/shortest-word-distance-ii/
[sort-transformed-array]: https://leetcode.com/problems/sort-transformed-array/
[trapping-rain-water]: https://leetcode.com/problems/trapping-rain-water/
