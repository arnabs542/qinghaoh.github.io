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

![Rolling Array](/assets/dp_dimension_reduction_1.png)

{% highlight java %}
public int minDistance(String word1, String word2) {
    int prev = 0;
    int[] dp = new int[word2.length() + 1];

    for (int j = 1; j <= word2.length(); j++) {
        dp[j] = j;
    }

    for (int i = 1; i <= word1.length(); i++) {
        prev = dp[0];
        dp[0] = i;
        for (int j = 1; j <= word2.length(); j++) {
            int tmp = dp[j];
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[j] = prev;
            } else {
                dp[j] = Math.min(prev, Math.min(dp[j], dp[j - 1])) + 1;
            }
            prev = tmp;
        }
    }
    return cur[word2.length()];
}
{% endhighlight %}

# Block Sum
[Matrix Block Sum][matrix-block-sum]

{% highlight java %}
public int[][] matrixBlockSum(int[][] mat, int K) {
    int m = mat.length, n = mat[0].length;
    int[][] rangeSum = new int[m + 1][n + 1];

    // similar to prefix sum
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            rangeSum[i + 1][j + 1] = rangeSum[i + 1][j] + rangeSum[i][j + 1] - rangeSum[i][j] + mat[i][j];
        }
    }

    int[][] sum = new int[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            int r1 = Math.max(0, i - K), c1 = Math.max(0, j - K), r2 = Math.min(m, i + K + 1), c2 = Math.min(n, j + K + 1);
            sum[i][j] = rangeSum[r2][c2] - rangeSum[r2][c1] - rangeSum[r1][c2] + rangeSum[r1][c1];
        }   
    }  
    return sum;
}
{% endhighlight %}

[edit-distance]: https://leetcode.com/problems/edit-distance/
[find-positive-integer-solution-for-a-given-equation]: https://leetcode.com/problems/find-positive-integer-solution-for-a-given-equation/
[matrix-block-sum]: https://leetcode.com/problems/matrix-block-sum/
[search-a-2d-matrix]: https://leetcode.com/problems/search-a-2d-matrix/
