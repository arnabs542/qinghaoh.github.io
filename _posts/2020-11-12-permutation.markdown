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

[permutation-sequence]: https://leetcode.com/problems/permutation-sequence/
