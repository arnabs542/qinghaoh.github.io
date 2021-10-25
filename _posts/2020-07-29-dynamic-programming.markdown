---
layout: post
title:  "Dynamic Programming"
tag: dynamic programming
---
[Edit Distance][edit-distance]

{% highlight java %}
public int minDistance(String word1, String word2) {
    int n1 = word1.length(), n2 = word2.length();

    // dp[i][j]: word1.substring(0, i) -> word2.substring(0, j)
    int[][] dp = new int[n1 + 1][n2 + 1];

    // word1.substring(0, i) -> empty string
    for (int i = 1; i <= n1; i++) {
        dp[i][0] = i;
    }

    // empty string -> word2.substring(0, j)
    for (int j = 1; j <= n2; j++) {
        dp[0][j] = j;
    }

    for (int i = 1; i <= n1; i++) {
        for (int j = 1; j <= n2; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                // replace: dp[i - 1][j - 1] + 1
                // delete/insert: dp[i - 1][j] + 1, dp[i][j - 1] + 1
                dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
            }
        }
    }
    return dp[n1][n2];
}
{% endhighlight %}

For example, `word1 = "newton", word2 = "einstein"`, then `dp` is:
```
[0,1,2,3,4,5,6,7,8]
[1,1,2,2,3,4,5,6,7]
[2,1,2,3,3,4,4,5,6]
[3,2,2,3,4,4,5,5,6]
[4,3,3,3,4,4,5,6,6]
[5,4,4,4,4,5,5,6,7]
[6,5,5,4,5,5,6,6,6]
```

Notice `dp[i - 1][j - 1] <= dp[i][j - 1] + 1` and `dp[i - 1][j - 1] <= dp[i - 1][j] + 1`

Rolling array optimization:
* `dp[i - 1][j] -> pre[j]`
* `dp[i][j] -> cur[j]`

{% highlight java %}
public int minDistance(String word1, String word2) {
    int n1 = word1.length(), n2 = word2.length();
    int[] pre = new int[n2 + 1], cur = new int[n2 + 1];

    for (int j = 1; j <= n2; j++) {
        pre[j] = j;
    }

    for (int i = 1; i <= n1; i++) {
        cur[0] = i;
        for (int j = 1; j <= n2; j++) {
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
    return pre[n2];
}
{% endhighlight %}

* `pre[j - 1] -> pre`
* `pre[j] -> cur[j]`
{% highlight java %}
public int minDistance(String word1, String word2) {
    int pre = 0, n1 = word1.length(), n2 = word2.length();
    int[] cur = new int[n2 + 1];

    for (int j = 1; j <= n2; j++) {
        cur[j] = j;
    }

    for (int i = 1; i <= n1; i++) {
        pre = cur[0];
        cur[0] = i;
        for (int j = 1; j <= n2; j++) {
            int tmp = cur[j];
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                cur[j] = pre;
            } else {
                cur[j] = Math.min(pre, Math.min(cur[j], cur[j - 1])) + 1;
            }
            pre = tmp;
        }
    }
    return cur[n2];
}
{% endhighlight %}

[Minimum ASCII Delete Sum for Two Strings][minimum-ascii-delete-sum-for-two-strings]

{% highlight java %}
if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
    dp[i][j] = dp[i - 1][j - 1];
} else {
    dp[i][j] = Math.min(dp[i][j - 1], dp[i - 1][j]) + 1;
}
{% endhighlight %}

{% highlight java %}
char c1 = s1.charAt(i - 1), c2 = s2.charAt(j - 1);
if (c1 == c2) {
    dp[i][j] = dp[i - 1][j - 1];
} else {
    dp[i][j] = Math.min(dp[i][j - 1] + c2, dp[i - 1][j] + c1);
}
{% endhighlight %}

[Longest Common Subsequence][longest-common-subsequence]

{% highlight java %}
if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
    dp[i][j] = dp[i - 1][j - 1] + 1;
} else {
    dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
}
{% endhighlight %}

[Uncrossed Lines][uncrossed-lines]

{% highlight java %}
if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
    dp[i][j] = dp[i - 1][j - 1] + 1;
} else {
    dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
}
{% endhighlight %}

Look how it's transformed to [Longest Common Subsequence][longest-common-subsequence]!

[Distinct Subsequences][distinct-subsequences]

{% highlight java %}
// t == ""
for (int i = 0; i <= m; i++) {
    dp[i][0] = 1;
}

for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
        if (s.charAt(i) == t.charAt(j)) {
            dp[i + 1][j + 1] = dp[i][j];
        }
        dp[i + 1][j + 1] += dp[i][j + 1];
    }
}
{% endhighlight %}

[Maximum Length of Repeated Subarray][maximum-length-of-repeated-subarray]

{% highlight java %}
// dp[i][j]: max length of repeated subarray ending with nums1[i - 1] and nums2[j - 1]
int[][] dp = new int[n1 + 1][n2 + 1];
for (int i = 0; i <= n1; i++) {
    for (int j = 0;j <= n2; j++) {
        if (i == 0 || j == 0){
            dp[i][j] = 0;
        } else if (nums1[i - 1] == nums2[j - 1]) {
            max = Math.max(max, dp[i][j] = dp[i - 1][j - 1] + 1);
        }
    }
}
{% endhighlight %}

[Interleaving String][interleaving-string]

{% highlight java %}
public boolean isInterleave(String s1, String s2, String s3) {
    int n1 = s1.length(), n2 = s2.length();
    if (s3.length() != n1 + n2) {
        return false;
    }

    // dp[i][j]: s1.substring(0, i) and s2.substring(0, j)
    boolean dp[][] = new boolean[n1 + 1][n2 + 1];
    dp[0][0] = true;

    for (int j = 0; j < n2; j++) {
        dp[0][j + 1] = dp[0][j] && s2.charAt(j) == s3.charAt(j);
    }

    for (int i = 0; i < n1; i++) {
        dp[i + 1][0] = dp[i][0] && s1.charAt(i) == s3.charAt(i);
    }

    for (int i = 0; i < n1; i++) {
        for (int j = 0; j < n2; j++) {
            dp[i + 1][j + 1] = (dp[i][j + 1] && s1.charAt(i) == s3.charAt(i + j + 1)) || (dp[i + 1][j] && s2.charAt(j) == s3.charAt(i + j + 1));
        }
    }

    return dp[n1][n2];
}
{% endhighlight %}

Reduced to 1D:

{% highlight java %}
boolean dp[] = new boolean[n2 + 1];
dp[0] = true;

// initializes first row
for (int j = 0; j < n2; j++) {
    dp[j + 1] = dp[j] && s2.charAt(j) == s3.charAt(j);
}

for (int i = 0; i < n1; i++) {
    // initializes first column in this row
    dp[0] = dp[0] && s1.charAt(i) == s3.charAt(i);
    for (int j = 0; j < n2; j++) {
        dp[j + 1] = (dp[j + 1] && s1.charAt(i) == s3.charAt(i + j + 1)) || (dp[j] && s2.charAt(j) == s3.charAt(i + j + 1));
    }
}

return dp[n2];
{% endhighlight %}

[Min Cost Climbing Stairs][min-cost-climbing-stairs]

{% highlight java %}
public int minCostClimbingStairs(int[] cost) {
    int[] dp = new int[cost.length];
    dp[0] = cost[0];
    dp[1] = cost[1];

    for (int i = 2; i < cost.length; i++) {
        dp[i] = Math.min(dp[i - 1], dp[i - 2]) + cost[i];
    }
    return Math.min(dp[cost.length - 1], dp[cost.length - 2]);
}
{% endhighlight %}

{% highlight java %}
public int minCostClimbingStairs(int[] cost) {
    int dp0 = cost[0], dp1 = cost[1];

    for (int i = 2; i < cost.length; i++) {
        int tmp = Math.min(dp0, dp1) + cost[i];
        dp0 = dp1;
        dp1 = tmp;
    }
    return Math.min(dp0, dp1);
}
{% endhighlight %}

[Greatest Sum Divisible by Three][greatest-sum-divisible-by-three]

-> Divisible by K

{% highlight java %}
public int maxSumDivThree(int[] nums) {
    int k = 3;
    int[] dp = new int[k];
    for (int num : nums) {
        int[] tmp = Arrays.copyOf(dp, k);
        for (int i = 0; i < k; i++) {
            int r = (dp[i] + num) % k;
            tmp[r] = Math.max(tmp[r], dp[i] + num);
        }
        dp = tmp;
    }
    return dp[0];
}
{% endhighlight %}

[Longest String Chain][longest-string-chain]

{% highlight java %}
public int minimumTotal(List<List<Integer>> triangle) {
    int n = triangle.size();
    // bottom level
    Integer[] dp = triangle.get(n - 1).toArray(new Integer[0]);

    // from bottom to top
    for (int i = n - 2; i >= 0; i--) {
        for (int j = 0; j <= i; j++) {
            dp[j] = Math.min(dp[j], dp[j + 1]) + triangle.get(i).get(j);
        }
    }

    return dp[0];
}
{% endhighlight %}

[Make Array Strictly Increasing][make-array-strictly-increasing]

{% highlight java %}
public int makeArrayIncreasing(int[] arr1, int[] arr2) {
    Arrays.sort(arr2);

    // rolling dp
    // dp[i]: i is the element we choose for the current position.
    // this element can be from either arr1 or arr2.
    Map<Integer, Integer> dp = new HashMap<>();
    dp.put(-1, 0);

    for (int a1: arr1) {
        // builds temporary dp map for i-th element of arr1
        Map<Integer, Integer> tmp = new HashMap<>();
        for (int key : dp.keySet()) {
            int val = dp.get(key);
            // path one
            // no assignment for key -> a1
            if (a1 > key) {
                tmp.put(a1, Math.min(tmp.getOrDefault(a1, Integer.MAX_VALUE), val));
            }

            int index = Arrays.binarySearch(arr2, key + 1);
            if (index < 0) {
                index = ~index;
            }

            // path two
            // one assignment for key -> arr2[index]
            if (index < arr2.length) {
                tmp.put(arr2[index], Math.min(tmp.getOrDefault(arr2[index], Integer.MAX_VALUE), val + 1));
            }
        }
        dp = tmp;
    }

    return dp.isEmpty() ? - 1: Collections.min(dp.values());
}
{% endhighlight %}

# Reverse

[Freedom Trail][freedom-trail]

{% highlight java %}
// O(mn^2)
public int findRotateSteps(String ring, String key) {
    // dp[i][j]: key.substring(i) and ring.substring(j)
    int n = ring.length(), m = key.length();
    int[][] dp = new int[m + 1][n];

    // reversely scans key
    for (int i = m - 1; i >= 0; i--) {
        // ring points to j
        for (int j = 0; j < n; j++) {
            dp[i][j] = Integer.MAX_VALUE;
            for (int k = 0; k < n; k++) {
                if (key.charAt(i) == ring.charAt(k)) {
                    int diff = Math.abs(j - k);
                    int step = Math.min(diff, n - diff);
                    dp[i][j] = Math.min(dp[i][j], step + dp[i + 1][k]);
                }
            }
        }
    }

    // if we process from 0 to (m - 1)
    // there could be multiple indefinite final states dp[i][j]
    return dp[0][0] + m;
}
{% endhighlight %}

Precomputation:

{% highlight java %}
// O(mn)
public int findRotateSteps(String ring, String key) {
    int n = ring.length(), m = key.length();
    // dp[i][j]: key.substring(i) and ring.substring(j)
    int[][] dp = new int[m + 1][n];

    int[][] clock = preProcess(ring, 1), anti = preProcess(ring, -1);
    for (int i = m - 1; i >= 0; i--) {
        int index = key.charAt(i) - 'a';
        for (int j = 0; j < n; j++) {
            int p = clock[j][index], q = anti[j][index];
            dp[i][j] = Math.min(dp[i + 1][p] + (j + n - p) % n, dp[i + 1][q] + (q + n - j) % n);
        }
    }
    return dp[0][0] + m;
}

/**
 * Precomputes the last index memo array.
 * @param r ring
 * @param orientation: clockwise (1) or anticlockwise (-1)
 * @return last index memo array
 */
private int[][] preProcess(String r, int orientation) {
    int n = r.length();
    // lastIndex[i][j]: last index of character (j + 'a') appears before (or at) the i-th position of r (wrapped)
    int[][] lastIndex = new int[n][26];

    // map[i]: last index of character (i + 'a')
    int[] map = new int[26];

    // "abc" -> "abcab"
    for (int i = 0, j = 0; j < n * 2 - 1; j++) {
        map[r.charAt(i) - 'a'] = i;

        // all the indexes before (n - 1) will be written twice
        // i.e. i and i + n
        // so the map copied at i will be overwritten at (i + n)
        // therefore, we can skip the first copy and start from (n - 1) directly
        if (j >= n - 1) {
            System.arraycopy(map, 0, lastIndex[i], 0, 26);
        }

        i = (i + orientation + n) % n;
    }
    return lastIndex;
}
{% endhighlight %}

[delete-operation-for-two-strings]: https://leetcode.com/problems/delete-operation-for-two-strings/
[distinct-subsequences]: https://leetcode.com/problems/distinct-subsequences/
[edit-distance]: https://leetcode.com/problems/edit-distance/
[freedom-trail]: https://leetcode.com/problems/freedom-trail/
[greatest-sum-divisible-by-three]: https://leetcode.com/problems/greatest-sum-divisible-by-three/
[interleaving-string]: https://leetcode.com/problems/interleaving-string/
[longest-common-subsequence]: https://leetcode.com/problems/longest-common-subsequence/
[longest-string-chain]: https://leetcode.com/problems/longest-string-chain/
[make-array-strictly-increasing]: https://leetcode.com/problems/make-array-strictly-increasing/
[maximum-length-of-repeated-subarray]: https://leetcode.com/problems/maximum-length-of-repeated-subarray/
[min-cost-climbing-stairs]: https://leetcode.com/problems/min-cost-climbing-stairs/
[minimum-ascii-delete-sum-for-two-strings]: https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/
[uncrossed-lines]: https://leetcode.com/problems/uncrossed-lines/
