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

[next-permutation]: https://leetcode.com/problems/next-permutation/
[permutation-sequence]: https://leetcode.com/problems/permutation-sequence/
