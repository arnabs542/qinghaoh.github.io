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
    int n1 = word1.length(), n2 = word2.length();
    int[][] dp = new int[n1 + 1][n2 + 1];

    for (int i = 1; i <= n1; i++) {
        dp[i][0] = i;
    }

    for (int j = 1; j <= n2; j++) {
        dp[0][j] = j;
    }

    for (int i = 1; i <= n1; i++) {
        for (int j = 1; j <= n2; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
            }
        }
    }
    return dp[n1][n2];
}
{% endhighlight %}

Rolling array optimization:

![Rolling Array](/assets/dp_dimension_reduction_1.png)

{% highlight java %}
public int minDistance(String word1, String word2) {
    int prev = 0, n1 = word1.length(), n2 = word2.length();
    int[] dp = new int[word2.length() + 1];

    for (int j = 1; j <= n2; j++) {
        dp[j] = j;
    }

    for (int i = 1; i <= n1; i++) {
        prev = dp[0];
        dp[0] = i;
        for (int j = 1; j <= n2; j++) {
            int tmp = dp[j];
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[j] = prev;
            } else {
                dp[j] = Math.min(prev, Math.min(dp[j], dp[j - 1])) + 1;
            }
            prev = tmp;
        }
    }
    return dp[n2];
}
{% endhighlight %}

# Range Sum

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

[Maximum Side Length of a Square with Sum Less than or Equal to Threshold][maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold]

{% highlight java %}
public int maxSideLength(int[][] mat, int threshold) {
    int m = mat.length, n = mat[0].length;
    int[][] p = new int[m + 1][n + 1];  // prefix sum

    int max = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            p[i + 1][j + 1] = p[i + 1][j] + p[i][j + 1] - p[i][j] + mat[i][j];
            if (i - max >= 0 && j - max >= 0 && squareSum(p, i, j, max + 1) <= threshold) {
                max++;
            }
        }
    }
    return max;
}

private int squareSum(int[][] p, int i, int j, int k) {
    return p[i + 1][j + 1] - p[i + 1][j + 1 - k] - p[i + 1 - k][j + 1] + p[i + 1 - k][j + 1 - k];
}
{% endhighlight %}

# 2D Prefix Sum

2D -> 1D: Calculates prefix sum for each row, and then each column, or vice versa.

[Number of Submatrices That Sum to Target][number-of-submatrices-that-sum-to-target]

{% highlight java %}
int[][] p = new int[m + 1][n];

// calculates prefix sum for each column 
for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
        p[i + 1][j] = p[i][j] + matrix[i][j];
    }
}

// rows in range [r1, r2]
for (int r1 = 0; r1 < m; r1++) {
    for (int r2 = r1; r2 < m; r2++) {
        // 560. Subarray Sum Equals K
        count += subarraySum(p, r1, r2, target);
    }
}
{% endhighlight %}

Alternatively,

{% highlight java %}
// calculates prefix sum for row
for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
        p[i][j + 1] = p[i][j] + matrix[i][j];
    }
}
{% endhighlight %}

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

# Flood Fill

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

[Surrounded Regions][surrounded-regions]

# Sort

[Largest Submatrix With Rearrangements][largest-submatrix-with-rearrangements]

# Swap

[Transform to Chessboard][transform-to-chessboard]

{% highlight java %}
public int movesToChessboard(int[][] board) {
    int n = board.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            // two types of rows if it can transform to a chessboard
            // e.g., if there's a row 01010011
            // then any other row must be either 01010011 or 10101100 (inverse)
            //
            // corollary: the 4 corners of any rectangle inside the board must be one of the following:
            // - 4 zeros
            // - 2 zeros 2 ones
            // - 4 ones
            //
            // checks the top left corner rectangle
            if ((board[0][0] ^ board[i][0] ^ board[0][j] ^ board[i][j]) == 1) {
                return -1;
            }
        }
    }

    // first row and column
    // sum, i.e. count of one's
    int rowSum = 0, colSum = 0;
    // count of misplaced element
    int rowMisplaced = 0, colMisplaced = 0;
    for (int i = 0; i < n; i++) {
        rowSum += board[0][i];
        colSum += board[i][0];

        // if the final pattern is "1010..."
        if (board[i][0] == i % 2) {
            rowMisplaced++;
        }
        if (board[0][i] == i % 2) {
            colMisplaced++ ;
        }
    }

    // - if n == 2 * k, then count(0) == count(1) == k
    // - if n == 2 * k + 1, then count(0) == k, count(1) == k + 1
    //   or count(0) == k + 1, count(1) == k
    //
    // checking the first row and column is sufficient,
    // because the top left corner rectangle is verified
    if (rowSum != n / 2 && rowSum != (n + 1) / 2) {
        return -1;
    }
    if (colSum != n / 2 && colSum != (n + 1) / 2) {
        return -1;
    }

    if (n % 2 == 1) {
        // when n is odd
        // only one final pattern is possible
        //
        // if misplaced is even
        // then the final pattern is "1010..."
        //
        // if misplaced is odd,
        // then the final pattern is the inverse of "1010..."
        // i.e. "0101..."
        // and the actual count of misplaced elements is (n - misplace)
        //
        // e.g. "001", misplaced == 1
        // the final pattern should be "010",
        // and the actual count of misplaced elements is 2 == n - misplaced
        //
        // in either case, the actual count of misplaced elements is even
        if (colMisplaced % 2 == 1) {
            colMisplaced = n - colMisplaced;
        }
        if (rowMisplaced % 2 == 1) {
            rowMisplaced = n - rowMisplaced;
        }
    } else {
        // when n is even
        // the final pattern can be either "1010..." or "0101..."
        //
        // if the final pattern is "0101..." (inverse)
        // the misplaced counts for row and col will be
        // (n - rowMisplaced) and (n - colMisplaced) respectively
        //
        // misplaced and (n - misplaced) are both even
        // picks the minimum of them
        colMisplaced = Math.min(n - colMisplaced, colMisplaced);
        rowMisplaced = Math.min(n - rowMisplaced, rowMisplaced);
    }

    // one swap fixes two misplaced elements
    return (colMisplaced + rowMisplaced) / 2;
}
{% endhighlight %}

# Linear Transformation

[Image Overlap][image-overlap]

{% highlight java %}
public int largestOverlap(int[][] img1, int[][] img2) {
    int n = img1.length;
    int count = 0;
    // linear transformation
    int[][] vectors = new int[2 * n][2 * n];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            // focuses on cells with ones
            if (img1[i][j] == 0) {
                continue;
            }

            for (int r = 0; r < n; r++) {
                for (int c = 0; c < n; c++) {
                    // focuses on cells with ones
                    if (img2[r][c] == 1) {
                        count = Math.max(count, ++vectors[n + i - r][n + j - c]);
                    }
                }
            }
        }
    }
    return count;
}
{% endhighlight %}

# Greedy

[Find Valid Matrix Given Row and Column Sums][find-valid-matrix-given-row-and-column-sums]

{% highlight java %}
public int[][] restoreMatrix(int[] rowSum, int[] colSum) {
    int m = rowSum.length, n = colSum.length;
    int[][] matrix = new int[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i][j] = Math.min(rowSum[i], colSum[j]);
            rowSum[i] -= matrix[i][j];
            colSum[j] -= matrix[i][j];
        }
    }
    return matrix;
}
{% endhighlight %}

[diagonal-traverse-ii]: https://leetcode.com/problems/diagonal-traverse-ii/
[edit-distance]: https://leetcode.com/problems/edit-distance/
[find-positive-integer-solution-for-a-given-equation]: https://leetcode.com/problems/find-positive-integer-solution-for-a-given-equation/
[find-valid-matrix-given-row-and-column-sums]: https://leetcode.com/problems/find-valid-matrix-given-row-and-column-sums/
[image-overlap]: https://leetcode.com/problems/image-overlap/
[largest-submatrix-with-rearrangements]: https://leetcode.com/problems/largest-submatrix-with-rearrangements/
[lonely-pixel-i]: https://leetcode.com/problems/lonely-pixel-i/
[matrix-block-sum]: https://leetcode.com/problems/matrix-block-sum/
[maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold]: https://leetcode.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/
[number-of-enclaves]: https://leetcode.com/problems/number-of-enclaves/
[number-of-submatrices-that-sum-to-target]: https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/
[search-a-2d-matrix]: https://leetcode.com/problems/search-a-2d-matrix/
[surrounded-regions]: https://leetcode.com/problems/surrounded-regions/
[transform-to-chessboard]: https://leetcode.com/problems/transform-to-chessboard/
