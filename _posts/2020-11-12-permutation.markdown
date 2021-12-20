---
layout: post
title:  "Permutation"
---
[Permutation Sequence][permutation-sequence]

{% highlight java %}
public String getPermutation(int n, int k) {
    List<Integer> num = new ArrayList<Integer>();
    int fact = 1;
    for (int i = 1; i <= n; i++) {
        num.add(i);
        fact *= i;
    }

    StringBuffer sb = new StringBuffer();
    k--;  // list index is 0-based
    for (int i = n; i > 0; i--) {
        fact /= i;
        int index = k / fact;
        sb.append(num.remove(index));
        k -= index * fact;
    }
    return sb.toString();
}
{% endhighlight %}

[Next Permutation][next-permutation]

Algorithm: [Generation in lexicographic order](https://en.wikipedia.org/wiki/Permutation#Generation_in_lexicographic_order)

{% highlight java %}
public void nextPermutation(int[] nums) {
    // Narayana Pandita
    // finds the largest index k such that a[k] < a[k + 1]
    int i = nums.length - 2;
    while (i >= 0 && nums[i + 1] <= nums[i]) {
        i--;
    }

    if (i >= 0) {
        // finds the largest index l greater than k such that a[k] < a[l]
        int j = nums.length - 1;
        while (j > i && nums[j] <= nums[i]) {
            j--;
        }
        // swaps the value of a[k] with that of a[l]
        swap(nums, i, j);
    }
    // reverses the sequence from a[k + 1] up to and including the final element a[n]
    reverse(nums, i + 1);
}

private void reverse(int[] nums, int start) {
    int i = start, j = nums.length - 1;
    while (i < j) {
        swap(nums, i++, j--);
    }
}

private void swap(int[] nums, int i, int j) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}
{% endhighlight %}

[Numbers With Repeated Digits][numbers-with-repeated-digits]

{% highlight java %}
public int numDupDigitsAtMostN(int n) {
    List<Integer> nums = new ArrayList<>();
    int tmp = n + 1;
    while (tmp != 0) {
        nums.add(0, tmp % 10);
        tmp /= 10;
    }

    // counts the number with digits < numDigits
    // e.g. 8765
    // xxx
    // xx
    // x
    int numDigits = nums.size(), noRepeats = 0;
    for (int i = 0; i < numDigits - 1; i++) {
        // excludes leading 0
        noRepeats += 9 * permutation(9, i);
    }

    // counts the number with same prefix
    // e.g. 8765
    // 1xxx ~ 7xxx
    // 80xx ~ 86xx
    // 870x ~ 875x
    // 8760 ~ 8765
    boolean[] used = new boolean[10];
    for (int i = 0; i < numDigits; i++) {
        int d = nums.get(i);
        // skips leading 0
        for (int j = i == 0 ? 1 : 0; j < d; j++) {
            // if the number j is not a part of the prefix
            if (!used[j]) {
                // prefix has (i + 1) digits
                noRepeats += permutation(10 - i - 1, numDigits - i - 1);
            }
        }

        // prefix has repeated number
        if (used[d]) {
            break;
        }
        used[d] = true;
    }
    return n - noRepeats;
}

// A(n, m)
private int permutation(int n, int m) {
    int p = 1;
    for (int i = 0; i < m; i++) {
        p *= n--;
    }
    return p;
}
{% endhighlight %}

[next-permutation]: https://leetcode.com/problems/next-permutation/
[numbers-with-repeated-digits]: https://leetcode.com/problems/numbers-with-repeated-digits/
[permutation-sequence]: https://leetcode.com/problems/permutation-sequence/
