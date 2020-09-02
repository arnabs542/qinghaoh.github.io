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

[binary-number-with-alternating-bits]: https://leetcode.com/problems/binary-number-with-alternating-bits/
[xor-operation-in-an-array]: https://leetcode.com/problems/xor-operation-in-an-array/
