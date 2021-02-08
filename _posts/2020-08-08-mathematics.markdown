---
layout: post
title:  "Mathematics"
tags: math
usemathjax: true
---
[Lagrange's four-square theorem](https://en.wikipedia.org/wiki/Lagrange%27s_four-square_theorem)

Lagrange's four-square theorem, also known as Bachet's conjecture, states that every natural number can be represented as the sum of four integer squares. That is, the squares form an additive basis of order four.

$$ p=a_{0}^{2}+a_{1}^{2}+a_{2}^{2}+a_{3}^{2} $$

[Legendre's three-square theorem](https://en.wikipedia.org/wiki/Legendre%27s_three-square_theorem)

Legendre's three-square theorem states that a natural number can be represented as the sum of three squares of integers

$$ n=x^{2}+y^{2}+z^{2} $$

if and only if $$ n $$ is not of the form $$ n = 4^a(8b + 7) $$ for nonnegative integers $$ a $$ and $$ b $$.

[Sum of two squares theorem](https://en.wikipedia.org/wiki/Sum_of_two_squares_theorem)

An integer greater than one can be written as a sum of two squares if and only if its prime decomposition contains no prime congruent to 3 modulo 4 raised to an odd power.

[Zeller's congruence](https://en.wikipedia.org/wiki/Zeller%27s_congruence)

Zeller's congruence is an algorithm devised by Christian Zeller to calculate the day of the week for any Julian or Gregorian calendar date.

Gregorian calendar:
$$ h=\left(q+\left\lfloor {\frac {13(m+1)}{5}}\right\rfloor +K+\left\lfloor {\frac {K}{4}}\right\rfloor +\left\lfloor {\frac {J}{4}}\right\rfloor -2J\right){\bmod {7}} $$

[Triangle area using coordinates](https://en.wikipedia.org/wiki/Triangle#Using_coordinates)

$$ T={\frac {1}{2}}{\big |}(x_{A}-x_{C})(y_{B}-y_{A})-(x_{A}-x_{B})(y_{C}-y_{A}) $$

[Euclid-Euler theorem](https://en.wikipedia.org/wiki/Euclid%E2%80%93Euler_theorem)

The Euclid–Euler theorem is a theorem in mathematics that relates perfect numbers to Mersenne primes. It states that an even number is perfect if and only if it has the form $$ 2p−1(2p − 1) $$, where $$ 2p − 1 $$ is a prime number. 

[Floor and celing functions](https://en.wikipedia.org/wiki/Floor_and_ceiling_functions)

$$ \left\lceil {\frac {n}{m}}\right\rceil =\left\lfloor {\frac {n+m-1}{m}}\right\rfloor =\left\lfloor {\frac {n-1}{m}}\right\rfloor +1 $$

$$ \left\lfloor {\frac {n}{m}}\right\rfloor =\left\lceil {\frac {n-m+1}{m}}\right\rceil =\left\lceil {\frac {n+1}{m}}\right\rceil -1 $$

[Inclusion–exclusion principle](https://en.wikipedia.org/wiki/Inclusion%E2%80%93exclusion_principle)

$$ |A\cup B|=|A|+|B|-|A\cap B| $$

[2 Keys Keyboard][2-keys-keyboard]

Prime Factorization

{% highlight java %}
public int minSteps(int n) {
    int s = 0;
    for (int d = 2; d <= n; d++) {
        while (n % d == 0) {
            s += d;
            n /= d;
        }
    }
    return s;
}
{% endhighlight %}

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

Taxicab metric = $$l_1$$ distance = $$l_1$$ norm = $Manhattan distance

[Triangle inequality](https://en.wikipedia.org/wiki/Triangle_inequality#Normed_vector_space)

In a normed vector space $$ V $$, one of the defining properties of the norm is the triangle inequality:

$$ \|x+y\|\leq \|x\|+\|y\|\quad \forall \,x,y\in V $$

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

$$\sum _{i=0}^{n-1}F_{2i+1}=F_{2n}$$

$$\sum _{i=1}^{n}F_{2i}=F_{2n+1}-1$$

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

For three points $$ P_{1}=(x_{1},y_{1})$$, $$P_{2}=(x_{2},y_{2}$$ and $$P_{3}=(x_{3},y_{3})$$, compute the z-coordinate of the cross product of the two vectors $$\overrightarrow {P_{1}P_{2}}$$ and $$\overrightarrow {P_{1}P_{3}}$$, which is given by the expression $$(x_{2}-x_{1})(y_{3}-y_{1})-(y_{2}-y_{1})(x_{3}-x_{1})$$.

[Convex Polygon][convex-polygon]

[Count Sorted Vowel Strings][count-sorted-vowel-strings]

{% highlight java %}
public int countVowelStrings(int n) {
    // comb(n + 4, 4)
    return (n + 4) * (n + 3) * (n + 2) * (n + 1) / 24;
}
{% endhighlight %}

[2-keys-keyboard]: https://leetcode.com/problems/2-keys-keyboard/
[4-keys-keyboard]: https://leetcode.com/problems/4-keys-keyboard/
[convex-polygon]: https://leetcode.com/problems/convex-polygon/
[count-sorted-vowel-strings]: https://leetcode.com/problems/count-sorted-vowel-strings/
[escape-the-ghosts]: https://leetcode.com/problems/escape-the-ghosts/
[implement-rand10-using-rand7]: https://leetcode.com/problems/implement-rand10-using-rand7/
[sparse-matrix-multiplication]: https://leetcode.com/problems/sparse-matrix-multiplication/

