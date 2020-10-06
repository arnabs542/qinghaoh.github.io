---
layout: post
title:  "Bitwise Operations"
tags: bit
---
## Properties
```
n ^ 0 = n
n ^ n = 0
2k ^ (2k + 1) = 1
```

[Binary Number with Alternating Bits][binary-number-with-alternating-bits]

{% highlight java %}
public boolean hasAlternatingBits(int n) {
    return (n & (n >> 1)) == 0 && (n | (n >> 2)) == n;
}
{% endhighlight %}

`(n & (n >> 1)) == 0` ensures no consecutive 1's.

[XOR Operation in an Array][xor-operation-in-an-array]

{% highlight java %}
public int xorOperation(int n, int start) {
    // nums[i] = start + 2 * i
    //  right shift each element
    //  nums[i] = start / 2 + i
    //
    // Igore LBS for now
    int xor = 2 * xorRightShift(n, start / 2);

    // If start is odd, then all elements are odd
    //  and if hte number of odd elements is odd
    //  then LSB == 1
    if (n % 2 == 1 && start % 2 == 1) {
        xor++;
    }

    return xor;
}

// nums[i] = start + i
private int xorRightShift(int n, int start) {
    // Let a_0 = start, a_i = nums[i]
    //  if n is even
    //   xor == a_0 ^ a_1 ^ a_2 ^ ... ^ a_(n - 1)
    //       == ((a_0 - 1) ^ (a_0 - 1)) ^ a_0 ^ a_1 ^ a_2 ^ ... ^ a_(n - 1)
    return start % 2 == 0 ? xorEvenStart(n, start) : (start - 1) ^ xorEvenStart(n + 1, start - 1);
}

/**
 * nums[i] = start + i
 * start % 2 == 0
 * We take use of the property: start ^ (start + 1) == 1
 */
private int xorEvenStart(int n, int start) {
    // Let a_0 = start, a_i = nums[i]
    //  if n is even
    //   xor == a_0 ^ a_1 ^ a_2 ^ ... ^ a_(n - 1)
    //       == (a_0 ^ a_1) ^ (a_2 ^ a_3) ^ ... ^ (a_(n - 2), a_(n - 1))
    //       == 1 ^ 1 ^ ... ^ 1
    //   where the number of 1's is n / 2
    //     if n / 2 is even, xor == 0; else xor == 1
    //
    // if n is odd
    //   xor == a_0 ^ a_1 ^ a_2 ^ ... ^ a_(n - 1)
    //       == (a_0 ^ a_1) ^ (a_2 ^ a_3) ^ ... ^ (a_(n - 3), a_(n - 2)) ^ a_(n - 1)
    //       == 1 ^ 1 ^ ... ^ 1 ^ a_(n - 1)
    //   where the number of 1's is n / 2
    //     if n / 2 is even, xor == 0 ^ a_(n - 1); else xor == 1 ^ a_(n - 1)
    //      a_(n - 1) == nums[i] == start + n - 1
    return n % 2 == 0 ? (n / 2) & 1 : ((n / 2) & 1) ^ (start + n - 1);
}
{% endhighlight %}

[Total Hamming Distance][total-hamming-distance]

{% highlight java %}
public int totalHammingDistance(int[] nums) {
    int count = 0;
    for (int i = 0; i < 31; i++) {
        int ones = 0;
        for (int num : nums) {
            if ((num & (1 << i)) != 0) {
                ones++;
            }
        }
        count += ones * (nums.length - ones);
    }
    return count;
}
{% endhighlight %}

[K-th Symbol in Grammar][k-th-symbol-in-grammar]

{% highlight java %}
public int kthGrammar(int N, int K) {
    // K is in [1, 2 ^ (N - 1)], so we can ignore N
    //
    // if k is 0 indexed
    // if f(k) == 0
    //   then f(2 * k) == 0, f(2 * k + 1) == 1
    // else if f(k) == 1
    //   then f(2 * k) == 1, f(2 * k + 1) == 0
    //
    // so f(2 * k) == f(k) ^ 0, f(2 * k + 1) == f(k) ^ 1
    //
    // f(10110)
    //   = f(1011) ^ 0
    //   = f(101) ^ 1 ^ 0
    //   = f(10) ^ 1 ^ 1 ^ 0
    //   = f(1) ^ 0 ^ 1 ^ 1 ^ 0
    //   = f(0) ^ 1 ^ 0 ^ 1 ^ 1 ^ 0
    //   = 1 ^ 0 ^ 1 ^ 1 ^ 0
    return Integer.bitCount(K - 1) & 1;
}
{% endhighlight %}

[Single Number][single-number]

{% highlight java %}
public int singleNumber(int[] nums) {
    int a = 0;
    for (int num : nums) {
      a ^= num;
    }
    return a;
}
{% endhighlight %}

[binary-number-with-alternating-bits]: https://leetcode.com/problems/binary-number-with-alternating-bits/
[k-th-symbol-in-grammar]: https://leetcode.com/problems/k-th-symbol-in-grammar/
[single-number]: https://leetcode.com/problems/single-number/
[total-hamming-distance]: https://leetcode.com/problems/total-hamming-distance/
[xor-operation-in-an-array]: https://leetcode.com/problems/xor-operation-in-an-array/
