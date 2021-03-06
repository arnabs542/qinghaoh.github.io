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

# Fractional DP

[Minimum Skips to Arrive at Meeting On Time][minimum-skips-to-arrive-at-meeting-on-time]

{% highlight java %}
public int minSkips(int[] dist, int speed, int hoursBefore) {
    int n = dist.length;
    // dp[i][j]: minimum arriving time * speed when we have travelled i roads and skipped j rests
    long[][] dp = new long[n + 1][n + 1];

    for (int j = 0; j <= n; j++) {
        for (int i = 0; i < n; i++) {
            // no skip, ceil
            dp[i + 1][j] = (dp[i][j] + dist[i] + speed - 1) / speed * speed;

            // skips current rest at i-th road
            if (j > 0) {
                dp[i + 1][j] = Math.min(dp[i + 1][j], dist[i] + dp[i][j - 1]);
            }
        }

        // min skips to arrive with time <= hoursBefore
        if (dp[n][j] <= speed * hoursBefore) {
            return j;
        }
    }
    return -1;
}
{% endhighlight %}

# Stack

[Minimum Difficulty of a Job Schedule][minimum-difficulty-of-a-job-schedule]

{% highlight java %}
public int minDifficulty(int[] jobDifficulty, int d) {
    int n = jobDifficulty.length;
    if (n < d) {
        return -1;
    }

    // dp[i][j]: minimum difficulty of a job schedule with jobs[0...j] in (i + 1) days
    int[][] dp = new int[d][n];

    // one day
    dp[0][0] = jobDifficulty[0];
    for (int i = 1; i < n; i++) {
        dp[0][i] = Math.max(jobDifficulty[i], dp[0][i - 1]);
    }

    for (int i = 1; i < d; i++) {
        for (int j = i; j < n; j++) {
            // max of jobDifficulty[k...j]
            int max = 0;
            dp[i][j] = Integer.MAX_VALUE;
            for (int k = j; k >= i; k--) {
                max = Math.max(max, jobDifficulty[k]);
                dp[i][j] = Math.min(dp[i][j], dp[i - 1][k - 1] + max);
            }
        }
    }

    return dp[d - 1][n - 1];
}
{% endhighlight %}

Reduced to 1D:

{% highlight java %}
public int minDifficulty(int[] jobDifficulty, int d) {
    int n = jobDifficulty.length;
    if (n < d) {
        return -1;
    }

    int[] dp = new int[n];

    // one day
    dp[0] = jobDifficulty[0];
    for (int i = 1; i < n; i++) {
        dp[i] = Math.max(dp[i - 1], jobDifficulty[i]);
    }

    for (int i = 1; i < d; i++) {
        for (int j = n - 1; j >= i; j--) {
            int max = 0;
            dp[j] = Integer.MAX_VALUE;
            for (int k = j; k >= i; k--) {
                max = Math.max(max, jobDifficulty[k]);
                dp[j] = Math.min(dp[j], dp[k - 1] + max);
            }
        }
    }

    return dp[n - 1];
}
{% endhighlight %}

Stack:

{% highlight java %}
private static final int MAX_JOB_DIFFICULTY = 1000;

// O(nd)
public int minDifficulty(int[] jobDifficulty, int d) {
    int n = jobDifficulty.length;
    if (n < d) {
        return -1;
    }

    // rolling DP
    int[] dp = new int[n], dp2 = new int[n];
    // initializes dp with max jobDifficulty of each day
    Arrays.fill(dp, MAX_JOB_DIFFICULTY);

    // with stack, we don't have to try all cuts
    Deque<Integer> st = new ArrayDeque<>();
    for (int i = 0; i < d; i++) {
        // clears the stack
        st.clear();

        // dp[j]: for i days with j jobs
        for (int j = i; j < n; j++) {
            // dp is for the previous day
            // first, assigns today's job difficulty to dp2[j]
            // (we could possibly find better solutions with stack later)
            dp2[j] = jobDifficulty[j] + (j == 0 ? 0 : dp[j - 1]);

            // monotonically decreasing
            //
            // Case 1: difficulty of last day is jobDifficulty[top]
            //
            //   dp2[j] = dp2[top] + (jobDifficulty[j] - jobDifficulty[top])
            //   adds the extra difficulity introduced by jobDifficulty[j]
            //
            // Case 2: difficulty of last day is jobDifficulty[k],
            //   where k < top and jobDifficulty[top] <= jobDifficulty[k] <= jobDifficulty[j]
            //   so dp2[top] = dp2[k]
            //
            //   dp2[top] - jobDifficulty[top] + jobDifficulty[j]
            //   = dp2[k] - jobDifficulty[top] + jobDifficulty[j]
            //   >= dp2[k] - jobDifficulty[k] + jobDifficulty[j]
            //   which is already calculated when k was visited
            while (!st.isEmpty() && jobDifficulty[j] >= jobDifficulty[st.peek()]) {
                int top = st.pop();
                dp2[j] = Math.min(dp2[j], dp2[top] - jobDifficulty[top] + jobDifficulty[j]);
            }

            // Case 3: max of last day is jobDifficulty[k],
            //   where k < top and jobDifficulty[top] <= jobDifficulty[j] <= jobDifficulty[k] (because top is popped)
            //   so dp2[top] = dp2[k]
            //
            //  st.peek() == k
            //   dp2[top] - jobDifficulty[top] + jobDifficulty[j]
            //   = dp2[k] - jobDifficulty[top] + jobDifficulty[j]
            //   >= dp2[k]
            if (!st.isEmpty()) {
                dp2[j] = Math.min(dp2[j], dp2[st.peek()]);
            }

            st.push(j);
        }

        // swaps dp and dp2
        int[] tmp = dp;
        dp = dp2;
        dp2 = tmp;
    }
    return dp[n - 1];
}
{% endhighlight %}

# Map

[Tallest Billboard][tallest-billboard]

{% highlight java %}
public int tallestBillboard(int[] rods) {
    // dp[i]: pair (a, b) with max a and b - a == i > 0
    Map<Integer, Integer> dp = new HashMap<>(), tmp;
    dp.put(0, 0);

    for (int r : rods) {
        tmp = new HashMap<>(dp);
        for (int d : tmp.keySet()) {
            // Case 1: put r to the long side
            // ---- v ----|-- d --|--- r ---|
            // ---- v ----|
            // dp[d + r] = max(dp[d + r], v)
            dp.put(d + r, Math.max(dp.getOrDefault(r + d, 0), tmp.get(d)));

            // Case 2: put r to the short side
            // ---- v ----|-- d --|
            // ---- v ----|--- r ---|
            // dp[r - d] = max(dp[r - d], v + d)
            // or
            // ---- v ----|-- d --|
            // ---- v ----|- r -|
            // dp[d - r] = max(dp[d - r], v + r)
            //
            // in summary,
            // dp[abs(d - r)] = max(dp[abs[d - r]], v + min(d, r))
            dp.put(Math.abs(d - r), Math.max(dp.getOrDefault(Math.abs(d - r), 0), tmp.get(d) + Math.min(d, r)));
        }
    }
    return dp.get(0);
}
{% endhighlight %}

[Stickers to Spell Word][stickers-to-spell-word]

{% highlight java %}
private Map<String, Integer> memo = new HashMap<>();
private int[][] countMap;

public int minStickers(String[] stickers, String target) {
    int n = stickers.length;
    this.countMap = new int[n][26];

    for (int i = 0; i < n; i++) {
        for (char c : stickers[i].toCharArray()) {
            countMap[i][c - 'a']++;
        }
    }

    memo.put("", 0);
    return dfs(target);
}

private int dfs(String target) {
    if (memo.containsKey(target)) {
        return memo.get(target);
    }

    int[] t = new int[26];
    for (char c : target.toCharArray()) {
        t[c - 'a']++;
    }

    int min = Integer.MAX_VALUE;
    StringBuilder sb = new StringBuilder();
    for (int[] s : countMap) {
        // the sticker has to contain the first character of target
        if (s[target.charAt(0) - 'a'] > 0) {
            // builds the string = (target - sticker)
            // the string is sorted
            for (int i = 0; i < 26; i++) {
                sb.append(String.valueOf((char)('a' + i)).repeat(Math.max(0, t[i] - s[i])));
            }

            int tmp = dfs(sb.toString());
            if (tmp != -1) {
                min = Math.min(min, tmp + 1);
            }
        }
        sb.setLength(0);
    }

    if (min == Integer.MAX_VALUE) {
        min = -1;
    }
    memo.put(target, min);
    return min;
}
{% endhighlight %}

[Minimum Distance to Type a Word Using Two Fingers][minimum-distance-to-type-a-word-using-two-fingers]

{% highlight java %}
public int minimumDistance(String word) {
    // distance is the total distance we get with right finger
    int distance = 0, save = 0;
    // dp[i]: the max distance that can be saved if left finger ends at character i
    int[] dp = new int[26];
    for (int i = 0; i < word.length() - 1; i++) {
        int curr = word.charAt(i) - 'A', next = word.charAt(i + 1) - 'A';
        for (int prev = 0; prev < 26; prev++) {
            // moves right finger from curr to next
            // or moves left finger from prev to next
            dp[curr] = Math.max(dp[curr], dp[prev] + cost(curr, next) - cost(prev, next));
        }
        save = Math.max(save, dp[curr]);

        // now right finger is at next, left finger is at curr
        distance += cost(curr, next);
    }
    return distance - save;
}

private int cost(int a, int b) {
    return Math.abs(a / 6 - b / 6) + Math.abs(a % 6 - b % 6);
}
{% endhighlight %}

[First Day Where You Have Been in All the Rooms][first-day-where-you-have-been-in-all-the-rooms]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int firstDayBeenInAllRooms(int[] nextVisit) {
    int n = nextVisit.length;
    long[] dp = new long[n];
    for (int i = 1; i < n; i++) {
        // 0 -> (i - 1): dp[i - 1]
        // (i - 1) -> nextVisit[i - 1]: 1
        // nextVisit[i - 1] -> (i - 1): dp[i - 1] - dp[nextVisit[i - 1]]
        // (i - 1) -> i: 1
        dp[i] = (2 * dp[i - 1] - dp[nextVisit[i - 1]] + 2 + MOD) % MOD;
    }
    return (int)dp[n - 1];
}
{% endhighlight %}

[Choose Numbers From Two Arrays in Range][choose-numbers-from-two-arrays-in-range]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int countSubranges(int[] nums1, int[] nums2) {
    // dp[i]: number of ways to sum to i
    Map<Integer, Integer> dp = new HashMap<>(), dp2;

    int count = 0;
    for (int i = 0; i < nums1.length; i++) {
        dp2 = new HashMap<>();
        dp2.put(nums1[i], 1);
        // negates nums2 elements
        // the goal is to find the number of different ranges that sum to 0
        dp2.put(-nums2[i], dp2.getOrDefault(-nums2[i], 0) + 1);

        for (var e : dp.entrySet()) {
            int k = e.getKey(), v = e.getValue();
            // picks nums1[i]
            dp2.put(k + nums1[i], (dp2.getOrDefault(k + nums1[i], 0) + v) % MOD);
            // picks -nums2[i]
            dp2.put(k - nums2[i], (dp2.getOrDefault(k - nums2[i], 0) + v) % MOD);
        }

        count = (count + dp2.getOrDefault(0, 0)) % MOD;
        dp = dp2;
    }
    return count;
}
{% endhighlight %}

[Minimum Total Space Wasted With K Resizing Operations][minimum-total-space-wasted-with-k-resizing-operations]

{% highlight java %}
public int minSpaceWastedKResizing(int[] nums, int k) {
    int n = nums.length, max = 0, sum = 0;
    int[][] dp = new int[n][k + 1];
    for (int i = n - 1; i >= 0; i--) {
        max = Math.max(max, nums[i]);
        sum += nums[i];
        dp[i][0] = max * (n - i) - sum;
    }

    for (int m = 1; m <= k; m++) {
        for (int i = n - 1; i >= 0; i--) {
            dp[i][m] = dp[i][m - 1];
            max = sum = 0;

            // resizes at i
            // finds the wasted space in [i, j)
            for (int j = i + 1; j < n; j++) {
                max = Math.max(max, nums[j - 1]);
                sum += nums[j - 1];
                dp[i][m] = Math.min(dp[i][m], dp[j][m - 1] + max * (j - i) - sum);
            }
        }
    }
    return dp[0][k];
}
{% endhighlight %}

[best-team-with-no-conflicts]: https://leetcode.com/problems/best-team-with-no-conflicts/
[build-array-where-you-can-find-the-maximum-exactly-k-comparisons]: https://leetcode.com/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons/
[choose-numbers-from-two-arrays-in-range]: https://leetcode.com/problems/choose-numbers-from-two-arrays-in-range/
[first-day-where-you-have-been-in-all-the-rooms]: https://leetcode.com/problems/first-day-where-you-have-been-in-all-the-rooms/
[frog-jump]: https://leetcode.com/problems/frog-jump/
[make-the-xor-of-all-segments-equal-to-zero]: https://leetcode.com/problems/make-the-xor-of-all-segments-equal-to-zero/
[maximum-height-by-stacking-cuboids]: https://leetcode.com/problems/maximum-height-by-stacking-cuboids/
[minimum-difficulty-of-a-job-schedule]: https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/
[minimum-distance-to-type-a-word-using-two-fingers]: https://leetcode.com/problems/minimum-distance-to-type-a-word-using-two-fingers/
[minimum-skips-to-arrive-at-meeting-on-time]: https://leetcode.com/problems/minimum-skips-to-arrive-at-meeting-on-time/
[minimum-total-space-wasted-with-k-resizing-operations]: https://leetcode.com/problems/minimum-total-space-wasted-with-k-resizing-operations/
[number-of-music-playlists]: https://leetcode.com/problems/number-of-music-playlists/
[number-of-ways-to-rearrange-sticks-with-k-sticks-visible]: https://leetcode.com/problems/number-of-ways-to-rearrange-sticks-with-k-sticks-visible/
[paint-house-iii]: https://leetcode.com/problems/paint-house-iii/
[stickers-to-spell-word]: https://leetcode.com/problems/stickers-to-spell-word/
[stone-game-v]: https://leetcode.com/problems/stone-game-v/
[tallest-billboard]: https://leetcode.com/problems/tallest-billboard/
