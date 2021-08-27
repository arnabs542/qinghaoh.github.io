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

[Non-negative Integers without Consecutive Ones][non-negative-integers-without-consecutive-ones]

{% highlight java %}
public int findIntegers(int n) {
    // Fibonacci
    // f[i]: the number of integers whose binary representation is not more than i bits
    //   and do not contain cosecutive ones
    int[] f = new int[32];
    f[0] = 1;
    f[1] = 2;

    // e.g. i == 5
    // [00000 - 11111] = [00000 - 01111] and [10000 - 10111]
    //   (any integer >= 11000 is not allowed)
    //   [00000 - 01111] => [0000 - 1111] = f[4]
    //   [10000 - 10111] => [000 - 111] = f[3]
    // therefore, f[5] == f[4] + f[3]
    for (int i = 2; i < f.length; i++) {
        f[i] = f[i - 1] + f[i - 2];
    }

    // 2 ^ 30 > 10 ^ 9
    // scans from MSB to LSB
    int i = 30, sum = 0, prev = 0;
    while (i >= 0) {
        if ((n & (1 << i)) != 0) {
            // sets current bit to zero
            // so the following range is [000...0, 011...1] = f[i]
            sum += f[i];

            // two consecutive ones appears
            if (prev == 1) {
                // removes n itself
                sum--;
                break;
            }
            prev = 1;
        } else {
            prev = 0;
        }
        i--;
    }

    // adds extra 1 if there are no consecutive ones in n
    return sum + 1;
}
{% endhighlight %}

```
1011010

1st bit: 0000000 - 0111111 -> f[6]
3rd bit: 1000000 - 1001111 -> f[4]
4th bit: 1010000 - 1010111 -> f[3]

anything greater than 1010111 will not be allowed
```

# Median

[Best Meeting Point][best-meeting-point]

* Mean minimizes total distance for Euclidian distance
* Median minimizes total distance for absolute deviation: \\[D_{i}=\|x_{i}-m(X)\|\\]
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

[Allocate Mailboxes][allocate-mailboxes]

{% highlight java %}
private static final int MAX = 100 * 10000;

public int minDistance(int[] houses, int k) {
    int n = houses.length;

    Arrays.sort(houses);

    // finds the median of houses[i] to houses[j]
    // calculates the distance
    int[][] distance = new int[n + 1][n + 1];
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            for (int m = i; m <= j; m++) {
                distance[i][j] += Math.abs(houses[(i + j) / 2] - houses[m]);
            }
        }
    }

    // dp[m][i]: minimum total distance of m mailboxes starting from i-th house
    int[][] dp = new int[k + 1][n + 1];

    // initializes dp table boarders with max
    Arrays.fill(dp[0], MAX);
    for (int i = 1; i <= k; i++) {
        dp[i][n] = MAX;
    }

    // initializes dp[0][n] with 0
    dp[0][n] = 0;

    for (int i = n - 1; i >= 0; i--) {
        for (int m = 1; m <= k; m++) {
            dp[m][i] = Integer.MAX_VALUE;
            for (int j = i; j < n; j++) {
                // houses[i:] is split into two groups:
                // houses[i:j] and houses[(j + 1):]
                dp[m][i] = Math.min(dp[m][i], distance[i][j] + dp[m - 1][j + 1]);
            }
        }
    }

    return dp[k][0];
}
{% endhighlight %}

Another way to calculate the distance matrix:

{% highlight java %}
for (int i = n - 1; i >= 0; i--) {
    for (int j = i; j < n; j++) {
        distance[i][j] = houses[j] - houses[i];
        if (i + 1 < n - 1 && j > 0) {
            // from houses[(i + 1):(j - 1)] to houses[i:j]
            // the minimum distance added by the two new endpoints houses[i] and houses[j]
            // equals houses[j] - houses[i]
            // the mailbox can be at any point between the new endpoints
            distance[i][j] += distance[i + 1][j - 1];
        }
    }
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

$$
x^{n}=
  \begin{cases}
    x\,(x^{2})^{\frac {n-1}{2}},&{\mbox{if }}n{\mbox{ is odd}} \\
    (x^{2})^{\frac {n}{2}},&{\mbox{if }}n{\mbox{ is even}}
  \end{cases}
$$

If we write \\(n\\) in binary as \\(b_{k}\cdots b_{0}\\), then this is equivalent to defining a sequence \\(r_{k+1}, \ldots, r_{0}\\) by letting \\(r_{k+1} = 1\\) and then defining \\(r_{i}=r_{i+1}^{2}x^{b_{i}} \\) for \\(i = k, \ldots, 0\\), where \\(r_{0}\\) will equal \\(x^{n}\\).

[Pow(x, n)][powx-n]

![Exponentiation by squaring](/assets/powx_n.png)

Iterative:

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

Recursive:

{% highlight java %}
public double myPow(double x, int n) {
    return fastPow(x, (long)n);
}

// fast power algorithm
private double fastPow(double x, long n) {
    if (n < 0) {
        return fastPow(1 / x, -n);
    }

    if (n == 0) {
        return 1;
    }

    if (n == 1) {
        return x;
    }

    return n % 2 == 0 ? fastPow(x * x, n / 2) : x * fastPow(x * x, (n - 1) / 2);
}
{% endhighlight %}

Another way of the last return is:

{% highlight java %}
double half = fastPow(x, n / 2);
return half * half * (n % 2 == 0 ? 1 : x);
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

# Logarithm

[Power of Three][power-of-three]

{% highlight java %}
public boolean isPowerOfThree(int n) {
    // takes the decimal part
    return (Math.log10(n) / Math.log10(3)) % 1 == 0;
}
{% endhighlight %}

# Absolute Value

[Maximum of Absolute Value Expression][maximum-of-absolute-value-expression]

{% highlight java %}
public int maxAbsValExpr(int[] arr1, int[] arr2) {
    int n = arr1.length;
    // linear combinations
    int[] c1 = new int[n], c2 = new int[n], c3 = new int[n], c4 = new int[n];

    for (int i = 0; i < n; i++) {
        c1[i] = arr1[i] + arr2[i] + i;
        c2[i] = arr1[i] + arr2[i] - i;
        c3[i] = arr1[i] - arr2[i] + i;
        c4[i] = arr1[i] - arr2[i] - i;
    }

    int max = Integer.MIN_VALUE;
    for (int[] c : new int[][]{c1, c2, c3, c4}) {
        max = Math.max(max, maxDiff(c));
    }
    return max;
}

private int maxDiff(int[] c) {
    Arrays.sort(c);
    return c[c.length - 1] - c[0];
}
{% endhighlight %}

# Minimax

[Egg Drop With 2 Eggs and N Floors][egg-drop-with-2-eggs-and-n-floors]

{% highlight java %}
public int twoEggDrop(int n) {
    // minimax
    //
    // suppose the optimal answer is x,
    // then the first number chosen cannot exceed x.
    // the second guess cannot exceed (x - 1)
    // ...
    // 1 + 2 + ... + x >= n
    return (int) Math.ceil((Math.sqrt(1 + 8 * n) - 1) / 2);
}
{% endhighlight %}

Another solution is recursion:

{% highlight java %}
private int[][] memo;

public int twoEggDrop(int n) {
    int eggs = 2;
    this.memo = new int[n + 1][eggs + 1];

    return drop(n, eggs);
}

private int drop(int n, int eggs) {
    if (eggs == 1 || n <= 1) {
        return n;
    }

    if (memo[n][eggs] > 0) {
        return memo[n][eggs];
    }

    int min = n;
    for (int i = 1; i <= n; i++) {
        // break, not break
        min = Math.min(min, 1 + Math.max(drop(i - 1, eggs - 1), drop(n - i, eggs)));
    }

    return memo[n][eggs] = min;
}
{% endhighlight %}

# Sequence

[Reach a Number][reach-a-number]

{% highlight java %}
public int reachNumber(int target) {
    // puts + and - signs on 1, 2, ..., k so that the sum == target
    // symmetry
    target = Math.abs(target);

    int step = 0, sum = 0;
    while (sum < target) {
        step++;
        sum += step;
    }

    // switching the sign of i will introduce a 2 * i delta
    // so the delta must be even
    while ((sum - target) % 2 != 0) {
        step++;
        sum += step;
    }

    return step;
}
{% endhighlight %}

[4-keys-keyboard]: https://leetcode.com/problems/4-keys-keyboard/
[allocate-mailboxes]: https://leetcode.com/problems/allocate-mailboxes/
[best-meeting-point]: https://leetcode.com/problems/best-meeting-point/
[check-if-number-is-a-sum-of-powers-of-three]: https://leetcode.com/problems/check-if-number-is-a-sum-of-powers-of-three/
[egg-drop-with-2-eggs-and-n-floors]: https://leetcode.com/problems/egg-drop-with-2-eggs-and-n-floors/
[handshakes-that-dont-cross]: https://leetcode.com/problems/handshakes-that-dont-cross/
[maximum-of-absolute-value-expression]: https://leetcode.com/problems/maximum-of-absolute-value-expression/
[non-negative-integers-without-consecutive-ones]: https://leetcode.com/problems/non-negative-integers-without-consecutive-ones/
[power-of-three]: https://leetcode.com/problems/power-of-three/
[powx-n]: https://leetcode.com/problems/powx-n/
[reach-a-number]: https://leetcode.com/problems/reach-a-number/
[robot-bounded-in-circle]: https://leetcode.com/problems/robot-bounded-in-circle/
[sparse-matrix-multiplication]: https://leetcode.com/problems/sparse-matrix-multiplication/
