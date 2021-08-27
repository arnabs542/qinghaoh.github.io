---
layout: post
title:  "Dynamic Programming VI"
tag: dynamic programming
---
[Best Team With No Conflicts][best-team-with-no-conflicts]

{% highlight java %}
public int bestTeamScore(int[] scores, int[] ages) {
    int n = ages.length;
    Integer[] index = new Integer[n];
    for (int i = 0; i < n; i++) {
        index[i] = i;
    }

    Arrays.sort(index, (i, j) -> ages[i] == ages[j] ? scores[i] - scores[j] : ages[i] - ages[j]);

    // dp[i]: max score if the i-th player is chosen and all the other players are between 0 and (i - 1)
    int[] dp = new int[n];
    int max = dp[0] = scores[index[0]];
    for (int i = 1; i < n; i++) {
       dp[i] = scores[index[i]];
       for (int j = 0; j < i; j++) {
           // age[index[j]] <= age[index[i]],
           // so we can always choose the younger player
           // if s/he has a lower score
           if (scores[index[j]] <= scores[index[i]]) {
               dp[i] = Math.max(dp[i], scores[index[i]] + dp[j]);
           }  
       }
       max = Math.max(dp[i], max);
    }
    return max;
}
{% endhighlight %}

Alternative representation:

{% highlight java %}
int[][] candidate = new int[n][2];
       
for (int i = 0; i < n; i++) {
    candidate[i][0] = ages[i];
    candidate[i][1] = scores[i];
}

Arrays.sort(candidate, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
{% endhighlight %}

[Maximum Height by Stacking Cuboids][maximum-height-by-stacking-cuboids]

{% highlight java %}
nt n = cuboids.length, max = 0;
int[] dp = new int[n];
for (int j = 0; j < n; j++) {
    dp[j] = cuboids[j][2];
    for (int i = 0; i < j; i++) {
        if (cuboids[i][0] >= cuboids[j][0] && cuboids[i][1] >= cuboids[j][1] && cuboids[i][2] >= cuboids[j][2]) {
            dp[j] = Math.max(dp[j], dp[i] + cuboids[j][2]);
        }
    }
    max = Math.max(max, dp[j]);
}
{% endhighlight %}

[Build Array Where You Can Find The Maximum Exactly K Comparisons][build-array-where-you-can-find-the-maximum-exactly-k-comparisons]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int numOfArrays(int n, int m, int k) {
    // dp[a][b][c]: max element == b
    long[][][] dp = new long[n + 1][m + 1][k + 1];

    // all one's
    for (int b = 1; b <= m; b++) {
        dp[1][b][1] = 1;
    }

    for (int a = 1; a <= n; a++) {
        for (int b = 1; b <= m; b++) {
            for (int c = 1; c <= k; c++) {
                long sum = 0;

                // dp[a][b][c] += b * dp[a - 1][b][c]
                // appends any element from [1, b] to the end of every array
                sum = (sum + b * dp[a - 1][b][c]) % MOD;

                // dp[a][b][c] += dp[a - 1][1][c - 1] + dp[a - 1][2][c - 1] + ... + dp[a - 1][b - 1][c - 1]
                // appends the element "b" to the end of every array
                for (int j = 1; j < b; j++) {
                    sum = (sum + dp[a - 1][j][c - 1]) % MOD;
                }

                dp[a][b][c] = (dp[a][b][c] + sum) % MOD;
            }
        }
    }

    long count = 0;
    for (int b = 1; b <= m; b++) {
        count = (count + dp[n][b][k]) % MOD;
    }

    return (int)count;
}
{% endhighlight %}

Prefix sum:

{% highlight java %}
// dp[a][b][c]: max element == b
long[][][] dp = new long[n + 1][m + 1][k + 1];
// prefix sum
long[][][] p = new long[n + 1][m + 1][k + 1];

// all one's
for (int b = 1; b <= m; b++) {
    dp[1][b][1] = 1;
    p[1][b][1] = p[1][b - 1][1] + 1;
}

for (int a = 1; a <= n; a++) {
    for (int b = 1; b <= m; b++) {
        for (int c = 1; c <= k; c++) {
            long sum = 0;

            // dp[a][b][c] += b * dp[a - 1][b][c]
            // appends any element from [1, b] to the end of every array
            sum = (sum + b * dp[a - 1][b][c]) % MOD;

            // dp[a][b][c] += dp[a - 1][1][c - 1] + dp[a - 1][2][c - 1] + ... + dp[a - 1][b - 1][c - 1]
            // appends the element "b" to the end of every array
            sum = (sum + p[a - 1][b - 1][c - 1]) % MOD;

            dp[a][b][c] = (dp[a][b][c] + sum) % MOD;
            p[a][b][c] = (dp[a][b][c] + p[a][b - 1][c]) % MOD;
        }
    }
}
{% endhighlight %}

[best-team-with-no-conflicts]: https://leetcode.com/problems/best-team-with-no-conflicts/
[build-array-where-you-can-find-the-maximum-exactly-k-comparisons]: https://leetcode.com/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons/
[maximum-height-by-stacking-cuboids]: https://leetcode.com/problems/maximum-height-by-stacking-cuboids/
