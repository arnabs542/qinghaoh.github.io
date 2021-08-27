---
layout: post
title:  "Combinatorics"
tags: math
usemathjax: true
---
# Permutations

[Permutations of multisets](https://en.wikipedia.org/wiki/Permutation#Permutations_of_multisets)

\\[{n \choose m_{1},m_{2},\ldots ,m_{l}}={\frac {n!}{m_{1}!\,m_{2}!\,\cdots \,m_{l}!}}=\frac {\left(\sum_{i=1}^{l}{m_{i}}\right)!}{\prod_{i=1}^{l}{m_{i}!}}\\]

[Probability of a Two Boxes Having The Same Number of Distinct Balls][probability-of-a-two-boxes-having-the-same-number-of-distinct-balls]

{% highlight java %}
private int n;
private long[] f;

public double getProbability(int[] balls) {
    int k = balls.length;

    this.n = Arrays.stream(balls).sum() / 2;

    this.f = new long[n + 1];
    f[0] = 1;
    for (int i = 1; i <= n; i++) {
        f[i] = f[i - 1] * i;
    }

    double[] good = new double[]{0}, all = new double[]{0};
    backtrack(balls, 0, new int[k], new int[k], good, all);

    return good[0] / all[0];
}

private void backtrack(int[] balls, int index, int[] box1, int[] box2, double[] good, double[] all) {
    if (index == balls.length) {
        // sum1 == sum2
        if (Arrays.stream(box1).sum() == n) {
            double p = (double)permutation(box1) * (double)permutation(box2);
            all[0] += p;

            long count1 = Arrays.stream(box1).filter(b -> b > 0).count();
            long count2 = Arrays.stream(box2).filter(b -> b > 0).count();
            good[0] += count1 == count2 ? p : 0;
        }
        return;
    }

    for (int i = 0; i <= balls[index]; i++) {
        box1[index] = i;
        box2[index] = balls[index] - i;
        backtrack(balls, index + 1, box1, box2, good, all);
        box1[index] = box2[index] = 0;
    }
}

private long permutation(int[] arr) {
    // permutations of multisets
    long prod = Arrays.stream(arr)
        .mapToLong(i -> f[i])
        .reduce(1, (a, b) -> a * b);
    return f[n] / prod;
}
{% endhighlight %}

# Combinations

[Count Sorted Vowel Strings][count-sorted-vowel-strings]

{% highlight java %}
public int countVowelStrings(int n) {
    // comb(n + 4, 4)
    return (n + 4) * (n + 3) * (n + 2) * (n + 1) / 24;
}
{% endhighlight %}

[Number of Sets of K Non-Overlapping Line Segments][number-of-sets-of-k-non-overlapping-line-segments]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int numberOfSets(int n, int k) {
    // equivalent to:
    // n + k - 1 points, k segments, not allowed to share endpoints.
    // C(n + k - 1, 2 * k)
    // (n + k - 1)! / ((n - k - 1)! * (2 * k)!)
    BigInteger count = BigInteger.valueOf(1);
    for (int i = 1; i < k * 2 + 1; i++) {
        count = count.multiply(BigInteger.valueOf(n + k - i));
        count = count.divide(BigInteger.valueOf(i));
    }
    count = count.mod(BigInteger.valueOf(MOD));
    return count.intValue();
}
{% endhighlight %}

## Number of k-combinations

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

## Indistinguishable Objects, Distinguishable Bins

### Stars and Bars

[Stars and bars](https://en.wikipedia.org/wiki/Stars_and_bars_(combinatorics))

#### Theorem one

Positivity: Place \\(n\\) objects into \\(k\\) bins, such that all bins contain at least one object. 

\\[\binom {n-1}{k-1}\\]

#### Theorem two

[Number of combinations with repetition](https://en.wikipedia.org/wiki/Combination#Number_of_combinations_with_repetition)

Non-negativity: Place \\(n\\) objects into \\(k\\) bins. Some bins can be empty.

\\[\left({\binom {k}{n}}\right)={\binom {n+k-1}{n}}\\]

## Distinguishable Objects, Indistinguishable Bins

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

[count-sorted-vowel-strings]: https://leetcode.com/problems/count-sorted-vowel-strings/
[count-ways-to-distribute-candies]: https://leetcode.com/problems/count-ways-to-distribute-candies/
[number-of-sets-of-k-non-overlapping-line-segments]: https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/
[probability-of-a-two-boxes-having-the-same-number-of-distinct-balls]: https://leetcode.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls/
