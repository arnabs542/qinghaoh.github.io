---
layout: post
title:  "Java Cheatsheet"
tags: java
---
# ArrayList
[toArray(T\[\] a)](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#toArray-T:A-)

Convert String ArrayList to String array:
{% highlight java %}
List<String> list = new ArrayList<String>();
String[] array = list.toArray(new String[0])
{% endhighlight %}

# Comparator
[Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)

Compare Strings by length in descending order, then by alphabetical order:
{% highlight java %}
Comparator.comparing(String::length)
    .reversed()
    .thenComparing(Comparator.<String>naturalOrder())
{% endhighlight %}

# String
[repeat(int count)](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#repeat(int))

# StringBuilder
[reverse()](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#reverse--)
[replace(int start, int end, String str)](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#replace-int-int-java.lang.String-)
