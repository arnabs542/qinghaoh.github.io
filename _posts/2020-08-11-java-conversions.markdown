---
layout: post
title:  "Java Conversions"
tags: java
---
# int[] -> Integer[]
{% highlight java %}
int[] a = {0, 1, 2};
Integer[] b = Arrays.stream(a).boxed().toArray(Integer[]::new);
{% endhighlight %}

# Integer[] -> int[]
{% highlight java %}
Integer[] a = {0, 1, 2};
int[] b = Arrays.stream(a).mapToInt(Integer::intValue).toArray();
{% endhighlight %}
