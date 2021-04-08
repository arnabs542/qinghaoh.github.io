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

[Maximal Square][maximal-square]

{% highlight java %}
public int maximalSquare(char[][] matrix) {
    if (matrix.length == 0) {
        return 0;
    }

    int m = matrix.length, n = matrix[0].length;
    // dp[i][j]: side length of the max square whose bottom-right is at (i - 1, j - 1)
    int[][] dp = new int[m + 1][n + 1];
    int maxLen = 0;
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (matrix[i - 1][j - 1] == '1') {
                dp[i][j] = Math.min(Math.min(dp[i][j - 1], dp[i - 1][j]), dp[i - 1][j - 1]) + 1;
                maxLen = Math.max(maxLen, dp[i][j]);
            }
        }
    }
    return maxLen * maxLen;
}
{% endhighlight %}

{% highlight java %}
public int maximalSquare(char[][] matrix) {
    if (matrix.length == 0) {
        return 0;
    }

    int m = matrix.length, n = matrix[0].length;
    int[] dp = new int[n + 1];
    int maxLen = 0, prev = 0;
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            int tmp = dp[j];
            if (matrix[i - 1][j - 1] == '1') {
                dp[j] = Math.min(Math.min(dp[j - 1], dp[j]), prev) + 1;
                maxLen = Math.max(maxLen, dp[j]);
            } else {
                dp[j] = 0;
            }
            prev = tmp;
        }
    }
    return maxLen * maxLen;
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

[Lonely Pixel I][lonely-pixel-i]

# Traversal

[Diagonal Traverse II][diagonal-traverse-ii]

{% highlight java %}
public int[] findDiagonalOrder(List<List<Integer>> nums) {
    List<Deque<Integer>> diags = new ArrayList<>();
    int n = 0;
    for (int i = 0; i < nums.size(); i++) {
        List<Integer> row = nums.get(i);
        for (int j = 0; j < row.size(); j++, n++) {
            if (i + j == diags.size()) {
                diags.add(new ArrayDeque<>());
            }
            diags.get(i + j).push(row.get(j));
        }
    }

    int[] result = new int[n];
    int i = 0;
    for (Deque<Integer> d : diags) {
        for (int num : d) {
            result[i++] = num;
        }
    }
    return result;
}
{% endhighlight %}

## Flood Fill

[Flood fill](https://en.wikipedia.org/wiki/Flood_fill): also called seed fill, is an algorithm that determines and alters the area connected to a given node in a **multi-dimensional** array with some matching attribute.

[Number of Enclaves][number-of-enclaves]

{% highlight java %}
{% raw %}
private static final int[][] DIRECTIONS = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
{% endraw %}
private int[][] grid;
private int m, n;

public int numEnclaves(int[][] grid) {
    this.grid = grid;
    this.m = grid.length;
    this.n = grid[0].length;

    // flood-fills the land (1 -> 0) from the boundary
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            // boundary
            if (i == 0 || j == 0 || i == m - 1 || j == n - 1) {
                fill(i, j);
            }
        }
    }

    int count = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            count += grid[i][j];
        }
    }
    return count;
}

private void fill(int i, int j) {
    if (i < 0 || j < 0 || i == m || j == n || grid[i][j] == 0) {
        return;
    }

    grid[i][j] = 0;
    for (int[] d : DIRECTIONS) {
        fill(i + d[0], j + d[1]);
    }
}
{% endhighlight %}

Regular DFS/BFS would also work.

[diagonal-traverse-ii]: https://leetcode.com/problems/diagonal-traverse-ii/
[edit-distance]: https://leetcode.com/problems/edit-distance/
[find-positive-integer-solution-for-a-given-equation]: https://leetcode.com/problems/find-positive-integer-solution-for-a-given-equation/
[lonely-pixel-i]: https://leetcode.com/problems/lonely-pixel-i/
[matrix-block-sum]: https://leetcode.com/problems/matrix-block-sum/
[maximal-square]: https://leetcode.com/problems/maximal-square/
[number-of-enclaves]: https://leetcode.com/problems/number-of-enclaves/
[search-a-2d-matrix]: https://leetcode.com/problems/search-a-2d-matrix/
