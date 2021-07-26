---
layout: post
title:  "Recursion"
tags: recursion
---

[Strobogrammatic Number II][strobogrammatic-number-ii]

{% highlight java %}
public List<String> findStrobogrammatic(int n) {
    return findStrobogrammatic(n, n);
}

private List<String> findStrobogrammatic(int n, int initialN) {
    if (n == 0) {
        return new ArrayList<>(Arrays.asList(""));
    }

    if (n == 1) {
        return new ArrayList<>(Arrays.asList("0", "1", "8"));
    }

    List<String> list = new ArrayList<>();
    for (String s : findStrobogrammatic(n - 2, initialN)) {
        if (n != initialN) {
            list.add("0" + s + "0");
        }
        list.add("1" + s + "1");
        list.add("8" + s + "8");
        list.add("6" + s + "9");
        list.add("9" + s + "6");
    }
    return list;
}
{% endhighlight %}

[Largest Merge Of Two Strings][largest-merge-of-two-strings]

{% highlight java %}
public String largestMerge(String word1, String word2) {
    if (word1.length() == 0  || word2.length() == 0) {
        return word1 + word2;
    }

    return word1.compareTo(word2) > 0 ?
        word1.charAt(0) + largestMerge(word1.substring(1), word2) : word2.charAt(0) + largestMerge(word1, word2.substring(1));
}
{% endhighlight %}

[largest-merge-of-two-strings]: https://leetcode.com/problems/largest-merge-of-two-strings/
[strobogrammatic-number-ii]: https://leetcode.com/problems/strobogrammatic-number-ii/
