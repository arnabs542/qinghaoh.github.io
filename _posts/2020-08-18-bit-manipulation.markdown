---
layout: post
title:  "Bit Manipulation"
tags: bit
---
# Properties
```
n ^ 0 = n
n ^ n = 0
2k ^ (2k + 1) = 1

n & -n 		// clears all but rightmost set bit
n & (n - 1)		// zeros out rightmost set bit, Brian Kernighan's Algorithm
n & (n - 1) == 0  	// power of 2

// enumerate all subsets
for (int s = superset; s > 0; s = (s - 1) & superset)
```

## Brian Kernighan's Algorithm

[Counting Bits][counting-bits]

{% highlight java %}
public int[] countBits(int num) {
    int[] result = new int[num + 1];
    for (int i = 1; i <= num; i++) {
        result[i] = result[i & (i - 1)] + 1;
    }
    return result;
}
{% endhighlight %}

[Bitwise AND of Numbers Range][bitwise-and-of-numbers-range]

{% highlight java %}
public int rangeBitwiseAnd(int left, int right) {
    while (right > left) {
        right &= (right - 1);
    }

    return right;
}
{% endhighlight %}

[Concatenation of Consecutive Binary Numbers][concatenation-of-consecutive-binary-numbers]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int concatenatedBinary(int n) {
    long sum = 0;
    int length = 0;
    for (int i = 1; i <= n; i++) {
        // power of 2
        if ((i & (i - 1)) == 0) {
            length++;
        }
        sum = ((sum << length) | i) % MOD;
    }
    return (int)sum;
}
{% endhighlight %}

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

[Single Number II][single-number-ii]

{% highlight java %}
public int singleNumber(int[] nums) {
    int result = 0;
    // counts 1's in each bit
    for (int i = 0; i < 32; i++) {
        int sum = 0;
        for (int num: nums) {
            sum += (num >> i) & 1;
        }
        sum %= 3;
        result |= sum << i;
    }
    return result;
}
{% endhighlight %}

The above solution can be generalized to: every element appears `k (k > 1)` times except for one.

Generalization II:

{% highlight java %}
public int singleNumber(int[] nums) {
    // sets of numbers that appear once and twice respectively
    int ones = 0, twos = 0;
    for (int num : nums) {
        // if ones doesn't contain num, adds num to ones iff it's not in twos
        // otherwise removes num from ones
        ones = (ones ^ num) & ~twos;
        // if twos doesn't contain num, adds num to twos iff it's not in ones
        // othwerwise removes it from twos
        twos = (twos ^ num) & ~ones;

        // Effectively, any number that appears:
        // once: is added to ones but not to twos
        // twice: is removed from ones and added twos
        // thrice: is removed from twos and not in either set
    }
    return ones;
}
{% endhighlight %}

Another perspective is:

Since bitwise operations on each of the 32 bits are independent of each other, we can group, say the i-th bit of all counters, into one 32-bit number. All bits in this 32-bit number will follow the same bitwise operations. 

To cover `k` counts, we require `2 ^ n >= k`, where `n` is the total number of bits. Therefore, `n >= log(k)`. In this problem, `k == 3`, so the complete transition loop of the counter is `00 -> 01 -> 10 -> 00 -> ...`.

[Karnaugh map](https://en.wikipedia.org/wiki/Karnaugh_map)

![Karnaugh map](/assets/karnaugh_map.png)

[Karnaugh map tool](https://www.charlie-coleman.com/experiments/kmap/)

{% highlight java %}
public int singleNumber(int[] nums) {
    int n0 = 0, n1 = 0;
    for (int num : nums) {
        int tmp = n0;
        n0 = (n0 & ~num) | (~n0 & ~n1 & num);
        n1 = (tmp & num) | (n1 & ~num);
    }
    return n0;
}
{% endhighlight %}

[Single Number III][single-number-iii]

{% highlight java %}
public int[] singleNumber(int[] nums) {
    int lsb = 0;
    for (int num : nums) {
        lsb ^= num;
    }

    // the two elements are distinct, so lsb != 0
    lsb &= -lsb;

    // partitions the numbers based on lsb
    int[] result = {0, 0};
    for (int num : nums) {
        if ((num & lsb) == 0) {
            result[0] ^= num;
        } else {
            result[1] ^= num;
        }
    }
    return result;
}
{% endhighlight %}

# Exclusive Or

Distributive Property:

```
(a + b) * (c + d) = (a * c) + (a * d) + (b * c) + (b * d)
(a ^ b) & (c ^ d) = (a & c) ^ (a & d) ^ (b & c) ^ (b & d)
```

[Maximum XOR of Two Numbers in an Array][maximum-xor-of-two-numbers-in-an-array]

{% highlight java %}
public int findMaximumXOR(int[] nums) {
    int max = 0, mask = 0;
    for (int i = 0; i < 32; i++) {
        // most significant (i + 1) bits
        mask = mask | (1 << (31 - i));

        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num & mask);
        }

        // so far, max contains the most significant i bits
        // tpm stands for the potential max we can get if we consider the current bit 
        int tmp = max | (1 << (31 - i));
        // finds a and b in the set so that a ^ b == tmp
        // a ^ b == tmp => a ^ b ^ b == tmp ^ b => a == tmp ^ b
        for (int prefix : set) {
            if (set.contains(tmp ^ prefix)) {
                max = tmp;
                break;
            }
        }
    }
    return max;
}
{% endhighlight %}

Another solution is by Trie.

# Gray Code

[Gray code](https://en.wikipedia.org/wiki/Gray_code): an ordering of the binary numeral system such that two successive values differ in only one bit (binary digit).

Formula:

{% highlight java %}
int g(int n) {
    return n ^ (n >> 1);
}
{% endhighlight %}

[Circular Permutation in Binary Representation][circular-permutation-in-binary-representation]

{% highlight java %}
public List<Integer> circularPermutation(int n, int start) {
    List<Integer> list = new ArrayList<>();
    for (int i = 0; i < (1 << n); i++) {
        list.add(start ^ i ^ (i >> 1));
    }
    return list;
}
{% endhighlight %}

## Inverse Gray Code

[Minimum One Bit Operations to Make Integers Zero][minimum-one-bit-operations-to-make-integers-zero]

{% highlight java %}
public int minimumOneBitOperations(int n) {
    // oeis A006068
    // inverse Gray code
    int count = 0;
    while (n > 0) {
        count ^= n;
        n >>= 1;
    }
    return count;
}
{% endhighlight %}

[Flip Columns For Maximum Number of Equal Rows][flip-columns-for-maximum-number-of-equal-rows]

{% highlight java %}
public int maxEqualRowsAfterFlips(int[][] matrix) {
    Map<String, Integer> map = new HashMap<>();
    for (int[] row : matrix) {
        // Flipping a subset of columns is like doing a bitwise XOR of some number k onto each row
        // if row ^ k == 0 or row ^ k == 1
        // then k == row or k == row ^ 1
        StringBuilder sb1 = new StringBuilder(), sb2 = new StringBuilder();
        for (int e : row) {
            sb1.append(e);
            sb2.append(e ^ 1);
        }
        map.compute(sb1.toString(), (k, v) -> v == null ? 1 : v + 1);
        map.compute(sb2.toString(), (k, v) -> v == null ? 1 : v + 1);
    }
    return Collections.max(map.values());
}
{% endhighlight %}

[Find Root of N-Ary Tree][find-root-of-n-ary-tree]

{% highlight java %}
public Node findRoot(List<Node> tree) {
    // visits all nodes
    // the root node would be the only node that is visited once
    // the rest of the nodes would be visited twice.
    int xor = 0;
    for (Node node : tree) {
        xor = xor ^ node.val;
        for (Node child : node.children) {
            xor = xor ^ child.val;
        }
    }

    for (Node node : tree) {
        if (node.val == xor) {
            return node;
        }
    }
    return null;
}
{% endhighlight %}

[Integer Replacement][integer-replacement]

{% highlight java %}
public int integerReplacement(int n) {
    int count = 0;
    // not n > 1, because of Integer.MAX_VALUE
    while (n != 1) {
        if (n % 2 == 0) {
            n >>>= 1;
        } else {
            // checks the last two digits
            if (n == 3 || (n & 3) == 1) {
                n--;
            } else {
                n++;
            }
        }
        count++;
    }
    return count;
}
{% endhighlight %}

[Divide Two Integers][divide-two-integers]

{% highlight java %}
public int divide(int dividend, int divisor) {
    if (dividend == Integer.MIN_VALUE && divisor == -1) {
        return Integer.MAX_VALUE;
    }

    int a = Math.abs(dividend), b = Math.abs(divisor), result = 0;
    for (int i = 31; i >= 0; i--) {
        if ((a >>> i) - b >= 0) {
            result += 1 << i;
            a -= b << i;
        }
    }

    return (dividend > 0) == (divisor > 0) ? result : -result;
}
{% endhighlight %}

[Missing Number][missing-number]

{% highlight java %}
    int missing = nums.length;
    for (int i = 0; i < nums.length; i++) {
        missing ^= i ^ nums[i];
    }
    return missing;
{% endhighlight %}

[UTF-8 Validation][utf-8-validation]

{% highlight java %}
public boolean validUtf8(int[] data) {
    int count = 0;
    for (int d : data) {
        d = d & 255;
        if (count == 0) {
            if ((d >> 5) == 0b110) {
                count = 1;
            } else if ((d >> 4) == 0b1110) {
                count = 2;
            } else if ((d >> 3) == 0b11110) {
                count = 3;
            } else if ((d >> 7) != 0) {
                return false;
            }
        } else {
            if ((d >> 6) != 0b10) {
                return false;
            }
            count--;
        }
    }
    return count == 0;
}
{% endhighlight %}

[binary-number-with-alternating-bits]: https://leetcode.com/problems/binary-number-with-alternating-bits/
[bitwise-and-of-numbers-range]: https://leetcode.com/problems/bitwise-and-of-numbers-range/
[concatenation-of-consecutive-binary-numbers]: https://leetcode.com/problems/concatenation-of-consecutive-binary-numbers/
[circular-permutation-in-binary-representation]: https://leetcode.com/problems/circular-permutation-in-binary-representation/
[counting-bits]: https://leetcode.com/problems/counting-bits/
[divide-two-integers]: https://leetcode.com/problems/divide-two-integers/
[find-root-of-n-ary-tree]: https://leetcode.com/problems/find-root-of-n-ary-tree/
[flip-columns-for-maximum-number-of-equal-rows]: https://leetcode.com/problems/flip-columns-for-maximum-number-of-equal-rows/
[integer-replacement]: https://leetcode.com/problems/integer-replacement/
[k-th-symbol-in-grammar]: https://leetcode.com/problems/k-th-symbol-in-grammar/
[maximum-xor-of-two-numbers-in-an-array]: https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/
[minimum-one-bit-operations-to-make-integers-zero]: https://leetcode.com/problems/minimum-one-bit-operations-to-make-integers-zero/
[missing-number]: https://leetcode.com/problems/missing-number/
[single-number]: https://leetcode.com/problems/single-number/
[single-number-ii]: https://leetcode.com/problems/single-number-ii/
[single-number-iii]: https://leetcode.com/problems/single-number-iii/
[total-hamming-distance]: https://leetcode.com/problems/total-hamming-distance/
[utf-8-validation]: https://leetcode.com/problems/utf-8-validation/
[xor-operation-in-an-array]: https://leetcode.com/problems/xor-operation-in-an-array/
