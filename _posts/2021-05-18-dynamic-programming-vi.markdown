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

[Number of Ways to Rearrange Sticks With K Sticks Visible][number-of-ways-to-rearrange-sticks-with-k-sticks-visible]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int rearrangeSticks(int n, int k) {
    long[][] dp = new long[n + 1][k + 1];
    dp[0][0] = 1;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= k; j++) {
            // f(n - 1, k - 1): rightmost is the longest
            // f(n - 1, k): rightmost is not the longest, and there are (n - 1) possibilities
            dp[i][j] = (dp[i - 1][j - 1] + (i - 1) * dp[i - 1][j] % MOD) % MOD;
        }
    }
    return (int)dp[n][k];
}
{% endhighlight %}

[Number of Music Playlists][number-of-music-playlists]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int numMusicPlaylists(int n, int goal, int k) {
    long[][] dp = new long[goal + 1][n + 1];
    dp[0][0] = 1;

    for (int i = 1; i <= goal; i++) {
        for (int j = 1; j <= n; j++) {
            // the last song is new
            dp[i][j] = (dp[i - 1][j - 1] * (n - (j - 1))) % MOD;

            // the last song is old
            // the songs from (j - k) to (j - 1) cannot be chosen
            if (j > k) {
                dp[i][j] = (dp[i][j] + (dp[i - 1][j] * (j - k)) % MOD) % MOD;
            }
        }
    }
    return (int)dp[goal][n];
}
{% endhighlight %}

[Frog Jump][frog-jump]

{% highlight java %}
public boolean canCross(int[] stones) {
    // stone : set of jump sizes which lead to the stone
    HashMap<Integer, Set<Integer>> map = new HashMap<>();
    for (int s : stones) {
        map.put(s, new HashSet<>());
    }

    map.get(0).add(0);
    for (int s : stones) {
        for (int k : map.get(s)) {
            // finds all stones that can be reached from the current stone
            for (int step = k - 1; step <= k + 1; step++) {
                if (step > 0 && map.containsKey(s + step)) {
                    map.get(s + step).add(step);
                }
            }
        }
    }
    return !map.get(stones[stones.length - 1]).isEmpty();
}
{% endhighlight %}

[Stone Game V][stone-game-v]

{% highlight java %}
// O(n ^ 3)
public int stoneGameV(int[] stoneValue) {
    int n = stoneValue.length;
    int[] p = new int[n + 1];
    for (int i = 0; i < n; i++) {
        p[i + 1] = p[i] + stoneValue[i];
    }

    int[][] dp = new int[n][n];
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i + len - 1 < n; i++) {
            int max = 0;
            for (int j = i; j + 1 <= i + len - 1; j++) {
                // [i, j] and [j + 1, i + len - 1]
                int left = p[j + 1] - p[i];
                int right = p[i + len] - p[j + 1];
                if (left == right) {
                    max = Math.max(max, left + dp[i][j]);
                    max = Math.max(max, right + dp[j + 1][i + len - 1]);
                } else if (left < right) {
                    max = Math.max(max, left + dp[i][j]);
                } else {
                    max = Math.max(max, right + dp[j + 1][i + len - 1]);
                }
            }
            dp[i][i + len - 1] = max;
        }
    }
    return dp[0][n - 1];
}
{% endhighlight %}

{% highlight java %}
// O(n ^ 2)
public int stoneGameV(int[] stoneValue) {
    int n = stoneValue.length;
    int[][] dp = new int[n][n], max = new int[n][n];

    // i <= j
    // max[i][j]: max(dp[i][i] + sum[i...i], dp[i][i + 1] + sum[i...(i + 1)], ..., dp[i][j] + sum[i...j]), i.e. left
    // max[j][i]: max(dp[i][j] + sum[i...j], dp[i + 1][j] + sum[(i + 1)...j], ..., dp[j][j] + sum[j...j]), i.e. right
    for (int i = 0; i < n; i++) {
        max[i][i] = stoneValue[i];
    }

    for (int j = 1; j < n; j++) {
        int mid = j, sum = stoneValue[j], rightHalf = 0;
        for (int i = j - 1; i >= 0; i--) {
            // sum(stoneValue[i, j])
            sum += stoneValue[i];

            // finds the index mid in the range [i, j]
            // if stoneValue[mid] is added to right half
            // then left half < right half
            while ((rightHalf + stoneValue[mid]) * 2 <= sum) {
                rightHalf += stoneValue[mid--];
            }

            // left remains
            // - if right half == left half, stoneValue[mid] is not added to right half
            //   so left half = max[i][mid]
            // - else, left half < right half, stoneValue[mid] is added to right half
            //   - mid == i means left is stoneValue[i], so Alice gets zero
            //   - else left half = max[i][mid - 1]
            dp[i][j] = rightHalf * 2 == sum ? max[i][mid] : (mid == i ? 0 : max[i][mid - 1]);

            // right remains
            // - if right half == left half, stoneValue[mid] is not added to right half
            //   so right half = max[j][mid + 1]
            // - else, left half > right half, stoneValue[mid] is not added to right half
            //   - mid == j means right is stoneValue[j], so Alice gets zero
            //   - else right half = max[j][mid + 1]
            dp[i][j] = Math.max(dp[i][j], mid == j ? 0 : max[j][mid + 1]);

            max[i][j] = Math.max(max[i][j - 1], dp[i][j] + sum);
            max[j][i] = Math.max(max[j][i + 1], dp[i][j] + sum);
        }
    }
    return dp[0][n - 1];
}
{% endhighlight %}

{% highlight java %}
// matrix[i][j] can be replaced by two arrays instead
left[i][j] = max(sum[i][k] + dp[i][k])
right[i][j] = max(sum[k][j] + dp[k][j])

left[i][j] = max(left[i][j-1], sum[i][j] + dp[i][j])
right[i][j] = max(right[i+1][j], sum[i][j] + dp[i][j])

dp[i][j] = max(left[i][mid], right[mid + 1][j])
{% endhighlight %}

[Paint House III][paint-house-iii]

{% highlight java %}
private static final int MAX = (int)1e6 + 1;

public int minCost(int[] houses, int[][] cost, int m, int n, int target) {
    // dp[i][j][k]: min cost where we have j neighborhood in the first i houses
    //   and the i-th house is painted with the color k
    int[][][] dp = new int[m + 1][target + 1][n];

    for (int i = 0; i <= m; i++) {
        for (int j = 0; j <= target; j++) {
            Arrays.fill(dp[i][j], MAX);
        }
    }

    for (int k = 0; k < n; k++) {
        dp[0][0][k] = 0;
    }

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= Math.min(target, i); j++) {
            for (int k = 0; k < n; k++) {
                // the current house is houses[i - 1]
                // skips if it's painted and the painted color is not (k + 1)
                if (houses[i - 1] != 0 && k != houses[i - 1] - 1) {
                    continue;
                }

                // compares the current house with previous house
                int sameNeighborhood = dp[i - 1][j][k];

                int diffNeighborhood = MAX;
                for (int prevK = 0; prevK < n; prevK++) {
                    if (prevK != k) {
                        diffNeighborhood = Math.min(diffNeighborhood, dp[i - 1][j - 1][prevK]);
                    }
                }

                // paints the current house only if it's not pained yet
                int paintCost = cost[i - 1][k] * (houses[i - 1] == 0 ? 1 : 0);
                dp[i][j][k] = Math.min(sameNeighborhood, diffNeighborhood) + paintCost;
            }
        }
    }

    int min = MAX;
    for (int k = 0; k < n; k++) {
        min = Math.min(min, dp[m][target][k]);
    }

    return min == MAX ? -1 : min;
}
{% endhighlight %}

[Make the XOR of All Segments Equal to Zero][make-the-xor-of-all-segments-equal-to-zero]

{% highlight java %}
private static final int MAX = (int)1e6 + 1;

public int minCost(int[] houses, int[][] cost, int m, int n, int target) {
    // dp[i][j][k]: min cost where we have j neighborhood in the first i houses
    //   and the i-th house is painted with the color k
    int[][][] dp = new int[m + 1][target + 1][n];

    for (int i = 0; i <= m; i++) {
        for (int j = 0; j <= target; j++) {
            Arrays.fill(dp[i][j], MAX);
        }
    }

    for (int k = 0; k < n; k++) {
        dp[0][0][k] = 0;
    }

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= Math.min(target, i); j++) {
            for (int k = 0; k < n; k++) {
                // the current house is houses[i - 1]
                // skips if it's painted and the painted color is not (k + 1)
                if (houses[i - 1] != 0 && k != houses[i - 1] - 1) {
                    continue;
                }

                // compares the current house with previous house
                int sameNeighborhood = dp[i - 1][j][k];

                int diffNeighborhood = MAX;
                for (int prevK = 0; prevK < n; prevK++) {
                    if (prevK != k) {
                        diffNeighborhood = Math.min(diffNeighborhood, dp[i - 1][j - 1][prevK]);
                    }
                }

                // paints the current house only if it's not pained yet
                int paintCost = cost[i - 1][k] * (houses[i - 1] == 0 ? 1 : 0);
                dp[i][j][k] = Math.min(sameNeighborhood, diffNeighborhood) + paintCost;
            }
        }
    }

    int min = MAX;
    for (int k = 0; k < n; k++) {
        min = Math.min(min, dp[m][target][k]);
    }

    return min == MAX ? -1 : min;
}
{% endhighlight %}

[best-team-with-no-conflicts]: https://leetcode.com/problems/best-team-with-no-conflicts/
[build-array-where-you-can-find-the-maximum-exactly-k-comparisons]: https://leetcode.com/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons/
[frog-jump]: https://leetcode.com/problems/frog-jump/
[make-the-xor-of-all-segments-equal-to-zero]: https://leetcode.com/problems/make-the-xor-of-all-segments-equal-to-zero/
[maximum-height-by-stacking-cuboids]: https://leetcode.com/problems/maximum-height-by-stacking-cuboids/
[number-of-music-playlists]: https://leetcode.com/problems/number-of-music-playlists/
[number-of-ways-to-rearrange-sticks-with-k-sticks-visible]: https://leetcode.com/problems/number-of-ways-to-rearrange-sticks-with-k-sticks-visible/
[paint-house-iii]: https://leetcode.com/problems/paint-house-iii/
[stone-game-v]: https://leetcode.com/problems/stone-game-v/
