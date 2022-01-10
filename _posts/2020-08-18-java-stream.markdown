---
layout: post
title:  "Java Stream"
tags: java
---
# Summing numbers

## Stream.reduce()

[T reduce(T identity, BinaryOperator\<T\> accumulator)](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-T-java.util.function.BinaryOperator-)

{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .reduce(0, (a, b) -> a + b);
{% endhighlight %}

{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .reduce(0, Integer::sum);
{% endhighlight %}

## Stream.collect() 

{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .collect(Collectors.summingInt(Integer::intValue));
{% endhighlight %}

## IntStream.sum()

{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .mapToInt(Integer::intValue)
  .sum();
{% endhighlight %}

## Range

{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .skip(1)
  .limit(1)
  .mapToInt(Integer::intValue)
  .sum();
{% endhighlight %}

# Frequency Map

{% highlight java %}
Map<Integer, Long> count = Arrays.stream(nums).boxed()
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
{% endhighlight %}

# Max

{% highlight java %}
int[] arr = {0, 1, 2};
Arrays.stream(arr).max().getAsInt()
{% endhighlight %}
