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

[Triangle area using coordinates](https://en.wikipedia.org/wiki/Triangle#Using_coordinates)

\\[
T={\frac {1}{2}}{\big |}(x_{A}-x_{C})(y_{B}-y_{A})-(x_{A}-x_{B})(y_{C}-y_{A}){\big |}
\\]

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

[Taxicab geometry](https://en.wikipedia.org/wiki/Taxicab_geometry)

Taxicab metric = \\(l_1\\) distance = \\(l_1\\) norm = $Manhattan distance

[Triangle inequality](https://en.wikipedia.org/wiki/Triangle_inequality#Normed_vector_space)

In a normed vector space $$ V $$, one of the defining properties of the norm is the triangle inequality:

\\[\|x+y\|\leq \|x\|+\|y\|\quad \forall \,x,y\in V\\]

[Escape The Ghosts][escape-the-ghosts]

{% highlight java %}
public boolean escapeGhosts(int[][] ghosts, int[] target) {
    // Manhattan distance
    int d = Math.abs(target[0]) + Math.abs(target[1]);
    for (int[] g : ghosts) {
        if (Math.abs(g[0] - target[0]) + Math.abs(g[1] - target[1]) <= d) {
            return false;
        }
    }
    return true;
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

[Graham scan](https://en.wikipedia.org/wiki/Graham_scan)

For three points \\(P_{1}=(x_{1},y_{1})\\), \\(P_{2}=(x_{2},y_{2}\\) and \\(P_{3}=(x_{3},y_{3})\\), compute the z-coordinate of the cross product of the two vectors \\(\overrightarrow {P_{1}P_{2}}\\) and \\(\overrightarrow {P_{1}P_{3}}\\), which is given by the expression \\((x_{2}-x_{1})(y_{3}-y_{1})-(y_{2}-y_{1})(x_{3}-x_{1})\\).

[Convex Polygon][convex-polygon]

[Count Sorted Vowel Strings][count-sorted-vowel-strings]

{% highlight java %}
public int countVowelStrings(int n) {
    // comb(n + 4, 4)
    return (n + 4) * (n + 3) * (n + 2) * (n + 1) / 24;
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

# Combination

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

# Dearrangement

\\[!n=(n-1)({!(n-1)}+{!(n-2)})\\]

\\[!n=n!\sum _{i=0}^{n}{\frac {(-1)^{i}}{i!}}, \quad n\geq 0\\]

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

[4-keys-keyboard]: https://leetcode.com/problems/4-keys-keyboard/
[best-meeting-point]: https://leetcode.com/problems/best-meeting-point/
[convex-polygon]: https://leetcode.com/problems/convex-polygon/
[count-sorted-vowel-strings]: https://leetcode.com/problems/count-sorted-vowel-strings/
[escape-the-ghosts]: https://leetcode.com/problems/escape-the-ghosts/
[implement-rand10-using-rand7]: https://leetcode.com/problems/implement-rand10-using-rand7/
[number-of-sets-of-k-non-overlapping-line-segments]: https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/
[powx-n]: https://leetcode.com/problems/powx-n/
[sparse-matrix-multiplication]: https://leetcode.com/problems/sparse-matrix-multiplication/
