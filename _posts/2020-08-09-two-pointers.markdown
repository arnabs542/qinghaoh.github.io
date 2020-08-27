---
layout: post
title:  "Two Pointers"
tags: array
---
[Remove All Adjacent Duplicates in String][remove-all-adjacent-duplicates-in-string]

[Backspace String Compare][backspace-string-compare]

Scan from the end of the Strings.

{% highlight java %}
public boolean backspaceCompare(String S, String T) {
    int i = S.length() - 1, j = T.length() - 1;
    int back = 0;
    while (true) {
        back = 0;
        while (i >= 0 && (back > 0 || S.charAt(i) == '#')) {
            back += S.charAt(i) == '#' ? 1 : -1;
            i--;
        }
        back = 0;
        while (j >= 0 && (back > 0 || T.charAt(j) == '#')) {
            back += T.charAt(j) == '#' ? 1 : -1;
            j--;
        }
        if (i >= 0 && j >= 0 && S.charAt(i) == T.charAt(j)) {
            i--;
            j--;
        } else {
            break;
        }
    }
    return i == -1 && j == -1;
}
{% endhighlight %}

[remove-all-adjacent-duplicates-in-string]: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/
[backspace-string-compare]: https://leetcode.com/problems/backspace-string-compare/
