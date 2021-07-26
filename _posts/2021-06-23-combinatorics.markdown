---
layout: post
title:  "Combinatorics"
tags: math
usemathjax: true
---
# Number of k-combinations

\\[{\binom {n}{k}}={\binom {n-1}{k-1}}+{\binom {n-1}{k}}\\]

{% highlight java %}
long[][] choose = new long[n][k];
for (int i = 0; i < n; i++) {
    choose[i][0] = 1;
}

for (int i = 1; i < n; i++) {
    for (int j = 1; j < k; j++) {
        choose[i][j] = (choose[i - 1][j - 1] + choose[i - 1][j]) % mod;
    }
}
{% endhighlight %}

# Indistinguishable Objects, Distinguishable Bins

## Stars and Bars

[Stars and bars](https://en.wikipedia.org/wiki/Stars_and_bars_(combinatorics))

### Theorem one

Positivity: Place \\(n\\) objects into \\(k\\) bins, such that all bins contain at least one object. 

\\[\binom {n-1}{k-1}\\]

### Theorem two

[Number of combinations with repetition](https://en.wikipedia.org/wiki/Combination#Number_of_combinations_with_repetition)

Non-negativity: Place \\(n\\) objects into \\(k\\) bins. Some bins can be empty.

\\[\left({\binom {k}{n}}\right)={\binom {n+k-1}{n}}\\]

# Distinguishable Objects, Indistinguishable Bins

[Stirling numbers of the second kind](https://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind): the number of ways to partition a set of n objects into k non-empty subsets.

\\[S(n,k)={\frac {1}{k!}}\sum _{i=0}^{k}(-1)^{i}{\binom {k}{i}}(k-i)^{n}\\]

[Count Ways to Distribute Candies][count-ways-to-distribute-candies]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int waysToDistribute(int n, int k) {
    long[][] dp = new long[k + 1][n + 1];

    for (int i = 1; i <= k; i++) {
        dp[i][i] = 1;
        for (int j = i + 1; j <= n; j++) {
            dp[i][j] = (dp[i - 1][j - 1] + (dp[i][j - 1] * i) % MOD) % MOD;
        }
    }

    return (int)dp[k][n];
}
{% endhighlight %}

[count-ways-to-distribute-candies]: https://leetcode.com/problems/count-ways-to-distribute-candies/
