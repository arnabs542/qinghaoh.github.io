---
layout: post
title:  "Combinatorics"
tags: math
usemathjax: true
---
# Number of k-combinations

\\[{\binom {n}{k}}={\binom {n-1}{k-1}}+{\binom {n-1}{k}}\\]

{% highlight java %}
long choose[n][k];
for (int i = 0; i < n; i++) {
    choose[i][0] = 1;
}

for (int i = 1; i < n; i++) {
    for (int j = 1; j < k; j++) {
        choose[i][j] = (choose[i - 1][j - 1] + choose[i - 1][j]) % mod;
    }
}
{% endhighlight %}

# Stars and Bars

[Stars and bars](https://en.wikipedia.org/wiki/Stars_and_bars_(combinatorics))

## Theorem one

Positivity: Place \\(n\\) objects into \\(k\\) bins, such that all bins contain at least one object. 

\\[\binom {n-1}{k-1}\\]

## Theorem two

[Number of combinations with repetition](https://en.wikipedia.org/wiki/Combination#Number_of_combinations_with_repetition): a sample of \\(k\\) elements from a set of \\(n\\) elements allowing for duplicates (i.e., with replacement) but disregarding different orderings (e.g. {2,1,2} = {1,2,2}).

Non-negativity: Place \\(n\\) objects into \\(k\\) bins. Some bins can be empty.

\\[\left({\binom {n}{k}}\right)={\binom {n+k-1}{k}}.\\]

[count-primes]: https://leetcode.com/problems/count-primes/
