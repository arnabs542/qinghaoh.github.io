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

// enumerate all submasks
for (int s = mask; s > 0; s = (s - 1) & mask)
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

# And

[Find a Value of a Mysterious Function Closest to Target][find-a-value-of-a-mysterious-function-closest-to-target]

{% highlight java %}
public int closestToTarget(int[] arr, int target) {
    int n = arr.length, min = Integer.MAX_VALUE;

    // ands[i]: unique AND values of arr[i...]
    // the size of ands[i] is at most ceil(log(arr[i])), i.e. arr[i].bitCount()
    Set<Integer>[] ands = new Set[n];
    for (int i = 0; i < n; i++) {
        ands[i] = new HashSet<>();
    }

    ands[n - 1].add(arr[n - 1]);

    // computes ands[i] from ands[i + 1]
    for (int i = n - 2; i >= 0; i--) {
        ands[i].add(arr[i]);
        for (int v: ands[i + 1]) {
            ands[i].add(arr[i] & v);
        }
    }

    for (var a : ands) {
        for (int v : a) {
            min = Math.min(min, Math.abs(v - target));
        }
    }

    return min;
}
{% endhighlight %}

# Exclusive Or

## Properties

Distributive Property:

```
(a + b) * (c + d) = (a * c) + (a * d) + (b * c) + (b * d)
(a ^ b) & (c ^ d) = (a & c) ^ (a & d) ^ (b & c) ^ (b & d)
```

## MSB -> LSB

Process the numbres bit by bit from msb to lsb.

[Maximum XOR of Two Numbers in an Array][maximum-xor-of-two-numbers-in-an-array]

{% highlight java %}
public int findMaximumXOR(int[] nums) {
    int max = 0, mask = 0;
    for (int i = 31; i >= 0; i--) {
        // most significant (32 - i) bits
        mask = mask | (1 << i);

        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num & mask);
        }

        // so far, max contains the most significant i bits
        // candidate stands for the potential max we can get if we set the current bit to 1
        int candidate = max | (1 << i);

        // finds a and b in the set so that a ^ b == candidate
        // a ^ b == candidate
        // => a ^ b ^ b == candidate ^ b
        // => a == candidate ^ b
        // if there's no such a and b
        // we pick 0 at this bit so max continues to be the max so far
        for (int prefix : set) {
            if (set.contains(candidate ^ prefix)) {
                max = candidate;
                break;
            }
        }
    }
    return max;
}
{% endhighlight %}

## Trie

It's a more intuitive way to process the numbers bit by bit.

[Maximum XOR of Two Numbers in an Array][maximum-xor-of-two-numbers-in-an-array]

{% highlight java %}
public int findMaximumXOR(int[] nums) {
TrieNode root = new TrieNode();
    for (int num : nums) {
        TrieNode node = root;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (node.children[bit] == null) {
                node.children[bit] = new TrieNode();
            }
            node = node.children[bit];
        }
    }

    int max = 0;
    for (int num: nums) {
        TrieNode node = root;
        int value = 0;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (node.children[bit ^ 1] != null) {
                // complement at the bit of this num exists
                // so the xor at the bit is 1, and the max at this bit will be 1
                node = node.children[bit ^ 1];
                value += 1 << i;
            } else {
                node = node.children[bit];
            }
        }
        max = Math.max(max, value);
    }
    return max;
}
}

class TrieNode {
    TrieNode[] children = new TrieNode[2];
}
{% endhighlight %}

{% highlight java %}
public int findMaximumXOR(int[] nums) {
    TrieNode root = new TrieNode();
    int max = 0;
    for (int num : nums) {
        TrieNode node = root, complement = root;
        int value = 0;

        for (int i = 31; i >= 0; i--) {
            // the i-th bit of num
            int bit = (num >> i) & 1;

            if (node.children[bit] == null) {
                node.children[bit] = new TrieNode();
            }
            node = node.children[bit];

            if (complement.children[bit ^ 1] != null) {
                // complement at the bit of this num exists
                // so the xor at the bit is 1, and the max at this bit will be 1
                complement = complement.children[bit ^ 1];
                value += 1 << i;
            } else {
                complement = complement.children[bit];
            }
        }
        max = Math.max(max, value);
    }
    return max;
}

class TrieNode {
    TrieNode[] children = new TrieNode[2];
}
{% endhighlight %}

[Count Pairs With XOR in a Range][count-pairs-with-xor-in-a-range]

{% highlight java %}
private static final int NUM_BITS = 15;

public int countPairs(int[] nums, int low, int high) {
    TrieNode root = new TrieNode();
    int lowCount = 0, highCount = 0;
    for (int num : nums) {
        lowCount += countSmallerPairs(root, num, low);
        highCount += countSmallerPairs(root, num, high + 1);
        insert(root, num);
    }
    return highCount - lowCount;
}

// counts elements in the trie that xor num < x
private int countSmallerPairs(TrieNode root, int num, int x) {
    TrieNode node = root;
    int count = 0;
    for (int i = NUM_BITS - 1; i >= 0 && node != null; i--) {
        int a = (num >> i) & 1, b = (x >> i) & 1;

        // compares the i-th bits of num and x
        if (b == 0) {
            // finds the bit == a, so they xor to 0
            node = node.children[a];
        } else {
            if (node.children[a] != null) {
                // finds the bit == a, so they xor to 0
                // so the xor < x
                count += node.children[a].count;
            }
            // keeps searching
            node = node.children[1 - a];
        }
    }
    return count;
}

class TrieNode {
    TrieNode[] children;
    int count;

    TrieNode() {
        children = new TrieNode[2];
        count = 0;
    }
}

private void insert(TrieNode root, int num) {
    TrieNode node = root;
    for (int i = NUM_BITS - 1; i >= 0; i--) {
        int b = (num >> i) & 1;
        if (node.children[b] == null) {
            node.children[b] = new TrieNode();
        }
        node = node.children[b];
        node.count++;
    }
}
{% endhighlight %}

Simplied version: no Trie, but similarly, level-traverse all the numbers

{% highlight java %}
public int countPairs(int[] nums, int low, int high) {
    return countSmallerPairs(nums, high + 1) - countSmallerPairs(nums, low);
}

// it's a variant of the trie solution
// the search starts from leaves
// counts pairs in nums that xor < x
private int countSmallerPairs(int[] nums, int x) {
    Map<Integer, Long> count = Arrays.stream(nums).boxed()
        .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    Map<Integer, Long> count2 = new HashMap<>();

    int pairs = 0;
    // iterates through each bit of x, from lsb to msb
    while (x > 0) {
        for (int k : count.keySet()) {
            // counts next level of nums to check
            // by right shifting all nums
            long v = count.get(k);
            count2.put(k >> 1, count2.getOrDefault(k >> 1, 0l) + v);

            // looks for pairs that, after XORing, have the same bits to the left
            // but have a 0 instead of a 1 at lsb.
            // if x & 1 == 0, then there can be no such pairs
            // if lsb == 1
            if ((x & 1) > 0) {
                // k ^ (x - 1) ^ k == x - 1 < x
                pairs += v * count.getOrDefault((x - 1) ^ k, 0l);
            }
        }
        count = count2;
        count2 = new HashMap<>();
        x >>= 1;
    }

    // i < j
    return pairs / 2;
}
{% endhighlight %}

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
[count-pairs-with-xor-in-a-range]: https://leetcode.com/problems/count-pairs-with-xor-in-a-range/
[counting-bits]: https://leetcode.com/problems/counting-bits/
[divide-two-integers]: https://leetcode.com/problems/divide-two-integers/
[find-a-value-of-a-mysterious-function-closest-to-target]: https://leetcode.com/problems/find-a-value-of-a-mysterious-function-closest-to-target/
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
