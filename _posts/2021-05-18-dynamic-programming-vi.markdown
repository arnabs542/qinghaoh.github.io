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
       
for(int i = 0; i < n; i++) {
    candidate[i][0] = ages[i];
    candidate[i][1] = scores[i];
}

Arrays.sort(candidate, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
{% endhighlight %}

[best-team-with-no-conflicts]: https://leetcode.com/problems/best-team-with-no-conflicts/
