---
layout: post
title:  "Multi-dimension"
tags: array
---
# Search

## Reduce to One-dimension
[Search a 2D Matrix][search-a-2d-matrix]

## Monotonic in Each Dimenstion
[Find Positive Integer Solution for a Given Equation][find-positive-integer-solution-for-a-given-equation]

{% highlight java %}
public List<List<Integer>> findSolution(CustomFunction customfunction, int z) {
    List<List<Integer>> result = new ArrayList<>();
    // starts from bottom-right
    int x = 1, y = 1000;
    while (x <= 1000 && y > 0) {
        int v = customfunction.f(x, y);
        if (v < z) {
            x++;
        } else if (v > z) {
            y--;
        } else {
            result.add(Arrays.asList(x++, y--));
        }
    }
    return result;
}
{% endhighlight %}

# Dimension Reduction

[Edit Distance][edit-distance]

{% highlight java %}
public int minDistance(String word1, String word2) {
    int[][] dp = new int[word1.length() + 1][word2.length() + 1];

    for (int i = 1; i <= word1.length(); i++) {
        dp[i][0] = i;
    }

    for (int j = 1; j <= word2.length(); j++) {
        dp[0][j] = j;
    }

    for (int i = 1; i <= word1.length(); i++) {
        for (int j = 1; j <= word2.length(); j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
            }
        }
    }
    return dp[word1.length()][word2.length()];
}
{% endhighlight %}

Rolling array optimization:
* `dp[i - 1][j] -> pre[j]`
* `dp[i][j] -> cur[j]`

{% highlight java %}
public int minDistance(String word1, String word2) {
    int[] pre = new int[word2.length() + 1];
    int[] cur = new int[word2.length() + 1];

    for (int j = 1; j <= word2.length(); j++) {
        pre[j] = j;
    }

    for (int i = 1; i <= word1.length(); i++) {
        cur[0] = i;
        for (int j = 1; j <= word2.length(); j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                cur[j] = pre[j - 1];
            } else {
                cur[j] = Math.min(pre[j - 1], Math.min(pre[j], cur[j - 1])) + 1;
            }
        }
        int[] tmp = pre;
        pre = cur;
        cur = tmp;
    }
    return pre[word2.length()];
}
{% endhighlight %}

* `pre[j - 1] -> pre`
* `pre[j] -> cur[j]`
{% highlight java %}
public int minDistance(String word1, String word2) {
    int pre = 0;
    int[] cur = new int[word2.length() + 1];

    for (int j = 1; j <= word2.length(); j++) {
        cur[j] = j;
    }

    for (int i = 1; i <= word1.length(); i++) {
        pre = cur[0];
        cur[0] = i;
        for (int j = 1; j <= word2.length(); j++) {
            int tmp = cur[j];
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                cur[j] = pre;
            } else {
                cur[j] = Math.min(pre, Math.min(cur[j], cur[j - 1])) + 1;
            }
            pre = tmp;
        }
    }
    return cur[word2.length()];
}
{% endhighlight %}

![Example](/assets/rotate_array.png)

[edit-distance]: https://leetcode.com/problems/edit-distance/
[find-positive-integer-solution-for-a-given-equation]: https://leetcode.com/problems/find-positive-integer-solution-for-a-given-equation/
[search-a-2d-matrix]: https://leetcode.com/problems/search-a-2d-matrix/
