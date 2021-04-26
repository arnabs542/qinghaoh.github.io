---
layout: post
title:  "Mathematics"
tags: math
usemathjax: true
---
# Theorem

[Lagrange's four-square theorem](https://en.wikipedia.org/wiki/Lagrange%27s_four-square_theorem)

Lagrange's four-square theorem, also known as Bachet's conjecture, states that every natural number can be represented as the sum of four integer squares. That is, the squares form an additive basis of order four.

\\[p=a_{0}^{2}+a_{1}^{2}+a_{2}^{2}+a_{3}^{2}\\]

[Legendre's three-square theorem](https://en.wikipedia.org/wiki/Legendre%27s_three-square_theorem)

Legendre's three-square theorem states that a natural number can be represented as the sum of three squares of integers

\\[n=x^{2}+y^{2}+z^{2}\\]

if and only if \\(n\\) is not of the form \\(n = 4^a(8b + 7)\\) for nonnegative integers \\(a\\) and \\(b\\).

[Sum of two squares theorem](https://en.wikipedia.org/wiki/Sum_of_two_squares_theorem)

An integer greater than one can be written as a sum of two squares if and only if its prime decomposition contains no prime congruent to 3 modulo 4 raised to an odd power.

[Zeller's congruence](https://en.wikipedia.org/wiki/Zeller%27s_congruence)

Zeller's congruence is an algorithm devised by Christian Zeller to calculate the day of the week for any Julian or Gregorian calendar date.

Gregorian calendar:
\\[h=\left(q+\left\lfloor {\frac {13(m+1)}{5}}\right\rfloor +K+\left\lfloor {\frac {K}{4}}\right\rfloor +\left\lfloor {\frac {J}{4}}\right\rfloor -2J\right){\bmod {7}}\\]

[Floor and celing functions](https://en.wikipedia.org/wiki/Floor_and_ceiling_functions)

\\[\left\lceil {\frac {n}{m}}\right\rceil =\left\lfloor {\frac {n+m-1}{m}}\right\rfloor =\left\lfloor {\frac {n-1}{m}}\right\rfloor +1\\]

\\[\left\lfloor {\frac {n}{m}}\right\rfloor =\left\lceil {\frac {n-m+1}{m}}\right\rceil =\left\lceil {\frac {n+1}{m}}\right\rceil -1\\]

[Inclusion–exclusion principle](https://en.wikipedia.org/wiki/Inclusion%E2%80%93exclusion_principle)

\\[
|A\cup B|=|A|+|B|-|A\cap B|
\\]

[4 Keys Keyboard][4-keys-keyboard]

{% highlight java %}
public int maxA(int N) {
    // https://oeis.org/A178715
    int[] dp = new int[N + 1];
    for (int i = 0; i <= N; i++) {
        dp[i] = i;
        // j steps to reach maxA(j)
        // then uses the remaining n - j steps to reach n - j - 1 copies of maxA(j)
        for (int j = 1; j <= i - 3; j++) {
            dp[i] = Math.max(dp[i], dp[j] * (i - j - 1));
        }
    }
    return dp[N];
}
{% endhighlight %}

[Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number#Sequence_properties)

\\[\sum_{i=0}^{n-1}F_{2i+1}=F_{2n}\\]

\\[\sum_{i=1}^{n}F_{2i}=F_{2n+1}-1\\]

[Sparse Matrix Multiplication][sparse-matrix-multiplication]

{% highlight java %}
public int[][] multiply(int[][] A, int[][] B) {
    int m = A.length, n = B[0].length;
    int[][] result = new int[m][n];
    for (int i = 0; i < m; i++) {
        for (int k = 0; k < B.length; k++) {
            if (A[i][k] != 0) {
                for (int j = 0; j < n; j++) {
                    if (B[k][j] != 0) {
                        result[i][j] += A[i][k] * B[k][j];
                    }
                }
            }
        }
    }
    return result;
}
{% endhighlight %}

# Median

[Best Meeting Point][best-meeting-point]

* Mean minimizes total distance for Euclidian distance
* Median minimzes total distance for absolute deviation
* Mode minimizes distance for indicator function

{% highlight java %}
public int minTotalDistance(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    List<Integer> x = new ArrayList<>(), y = new ArrayList<>();    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 1) {
                x.add(i);
            }
        }
    }
    for (int j = 0; j < n; j++) {
        for (int i = 0; i < m; i ++) {
            if (grid[i][j] == 1) {  
                y.add(j);
            }
        }
    }
    return minDistance1D(x) + minDistance1D(y);
}

public int minDistance1D(List<Integer> points) {
    int d = 0, median = points.get(points.size() / 2);
    for (int p : points) {
        d += Math.abs(p - median);
    }
    return d;
}
{% endhighlight %}

# Permutation

[Permutations of multisets](https://en.wikipedia.org/wiki/Permutation#Permutations_of_multisets)

\\[{n \choose m_{1},m_{2},\ldots ,m_{l}}={\frac {n!}{m_{1}!\,m_{2}!\,\cdots \,m_{l}!}}=\frac {\left(\sum_{i=1}^{l}{m_{i}}\right)!}{\prod_{i=1}^{l}{m_{i}!}}\\]

# Combination

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

## Catalan Number

The nth Catalan number is given directly in terms of binomial coefficients by

\\[
C_{n}={\frac {1}{n+1}}{2n \choose n}={\frac {(2n)!}{(n+1)!\,n!}}=\prod \limits _{k=2}^{n}{\frac {n+k}{k}}\qquad {\text{for }}n\geq 0
\\]

\\[
C_{0}=1\quad {\text{and}}\quad C_{n+1}={\frac {2(2n+1)}{n+2}}C_{n}
\\]

[Handshakes That Don't Cross][handshakes-that-dont-cross]

```
dp[n + 1] = dp[0] * dp[n] + dp[1] * dp[n - 1] + ... + dp[n] * dp[0]
```

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int numberOfWays(int num_people) {
    int n = num_people / 2;  // pairs
    long[] dp = new long[n + 1];
    dp[0] = 1;

    // splits pairs
    for (int k = 1; k <= n; k++) {
        for (int i = 0; i < k; i++) {
            dp[k] = (dp[k] + dp[i] * dp[k - 1 - i]) % MOD;
        }
    }
    return (int)dp[n];
}
{% endhighlight %}

# Exponentiation

[Exponentiation by squaring](https://en.wikipedia.org/wiki/Exponentiation_by_squaring): square-and-multiply/binary exponentiation/double-and-add

\\[
\begin{cases}
x^{n}={\begin{cases}x\,(x^{2})^{\frac {n-1}{2}},&{\mbox{if }}n{\mbox{ is odd}}
(x^{2})^{\frac {n}{2}},&{\mbox{if }}n{\mbox{ is even}}.\end{cases}}
\end{cases}
\\]

If we write \\(n\\) in binary as \\(b_{k}\cdots b_{0}\\), then this is equivalent to defining a sequence \\(r_{k+1}, \ldots, r_{0}\\) by letting \\(r_{k+1} = 1\\) and then defining \\(r_{i}=r_{i+1}^{2}x^{b_{i}} \\) for \\(i = k, \ldots, 0\\), where \\(r_{0}\\) will equal \\(x^{n}\\).

[Pow(x, n)][powx-n]

![Exponentiation by squaring](/assets/powx_n.png)

{% highlight java %}
public double myPow(double x, int n) {
    // if n == Integer.MIN_VALUE, -n would overflow
    // so n is converted to long
    long nl = n;
    if (nl < 0) {
        x = 1 / x;
        nl = -nl;
    }

    // fast power algorithm
    // r = x ^ 0
    double r = 1, pow = x;
    for (long i = nl; i > 0; i /= 2) {
        if (i % 2 == 1) {
            r *= pow;
        }
        pow *= pow;
    }
    return r;
}
{% endhighlight %}

# Radix

[Check if Number is a Sum of Powers of Three][check-if-number-is-a-sum-of-powers-of-three]

{% highlight java %}
public boolean checkPowersOfThree(int n) {
    while (n > 0) {
        if (n % 3 == 2) {
            return false;
        }

        // right shifts ternary bits by 1
        n /= 3;
    }
    return true;
}
{% endhighlight %}

# Dyanmical Systems

[Attractor](https://en.wikipedia.org/wiki/Attractor): a set of numerical values toward which a system tends to evolve, for a wide variety of starting conditions of the system.

[Robot Bounded In Circle][robot-bounded-in-circle]

{% highlight java %}
public boolean isRobotBounded(String instructions) {
    // N, E, S, W
    {% raw %}
    int[][] directions = new int[][]{{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    {% endraw %}
    int x = 0, y = 0, d = 0;
    for (char c : instructions.toCharArray()) {
        if (c == 'L') {
            d = (d + 3) % 4;
        } else if (c == 'R') {
            d = (d + 1) % 4;
        } else {
            x += directions[d][0];
            y += directions[d][1];
        }
    }

    // after at most 4 cycles, the limit cycle trajectory returns to the initial point
    return (x == 0 && y == 0) || d != 0;
}
{% endhighlight %}

[4-keys-keyboard]: https://leetcode.com/problems/4-keys-keyboard/
[best-meeting-point]: https://leetcode.com/problems/best-meeting-point/
[check-if-number-is-a-sum-of-powers-of-three]: https://leetcode.com/problems/check-if-number-is-a-sum-of-powers-of-three/
[count-sorted-vowel-strings]: https://leetcode.com/problems/count-sorted-vowel-strings/
[handshakes-that-dont-cross]: https://leetcode.com/problems/handshakes-that-dont-cross/
[number-of-sets-of-k-non-overlapping-line-segments]: https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/
[powx-n]: https://leetcode.com/problems/powx-n/
[robot-bounded-in-circle]: https://leetcode.com/problems/robot-bounded-in-circle/
[sparse-matrix-multiplication]: https://leetcode.com/problems/sparse-matrix-multiplication/
