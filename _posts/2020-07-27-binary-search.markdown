---
layout: post
title:  "Binary Search"
---
# Template
[Binary Search][binary-search]

{% highlight java %}
public int search(int[] nums, int target) {
    int low = 0, high = nums.length - 1;
    while (low <= high) {
        int mid = (low + high) >>> 1;
        if (nums[mid] == target) {
            return mid;
        }
        if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1;
}
{% endhighlight %}

{% highlight java %}
public int search(int[] nums, int target) {
    int low = 0, high = nums.length - 1;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    return nums[low] == target ? low : -1;
}
{% endhighlight %}

{% highlight java %}
public int search(int[] nums, int target) {
    int low = 0, high = nums.length - 1;
    while (low < high) {
        int mid = low + (high - low + 1) / 2;
        if (nums[mid] > target) {
            high = mid - 1;
        } else {
            low = mid;
        }
    }
    return nums[low] == target ? low : -1;
}
{% endhighlight %}

There can be variants of this template. For example: [First Bad Version][first-bad-version], [The K Weakest Rows in a Matrix][the-k-weakest-rows-in-a-matrix]

* `low <= high`, `low < high`, ...
* `low = mid + 1`, `low = mid`, ...
* `high = mid - 1`, `high = mid`, ...
* `return -1`, `return low`, `return high`, ...

## While Condition

`while (low < high)` is a better choice. The while loop only exits when `low == high`, which means there's only one element left.

## Boundary

The initial boundary `[low, high]` should include ***all*** possible answers. When each loop begins, any value within the range `[low, high]` could be the answer.

## Mid

{% highlight java %}
mid = low + (high - low) / 2;  // lower mid
mid = low + (high - low + 1) / 2;  // upper mid
{% endhighlight %}

To avoid infinite loop, here's a rule of thumb:

* lower mid: `low = mid + 1` and `high = mid`
* upper mid: `low = mid` and `high = mid - 1`

## Boundary Update

Rule of thumb: always use a logic that you can exclude `mid`.

{% highlight java %}
if (nums[mid] > target) {
    high = mid - 1;
} else {
    low = mid;
}
{% endhighlight %}

{% highlight java %}
if (nums[mid] < target) {
    low = mid + 1;
} else {
    high = mid;
}
{% endhighlight %}

To understand the corner cases, test your code with these examples: `[0]`, `[0, 1]`, `[0, 1, 2]` and `[0, 1, 2, 3]`.

# Variants
[Search Insert Position][search-insert-position]

{% highlight java %}
public int searchInsert(int[] nums, int target) {
    int low = 0, high = nums.length - 1;
    while (low <= high) {
        int mid = (low + high) >>> 1;
        if (nums[mid] == target) {
            return mid;
        }
        if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return low;
}
{% endhighlight %}

[Find K Closest Elements][find-k-closest-elements]

Search for the first index from which the `k`-element sliding window starts.

{% highlight java %}
public List<Integer> findClosestElements(int[] arr, int k, int x) {
    int low = 0, high = arr.length - k;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (x - arr[mid] > arr[mid + k] - x) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    return Arrays.stream(arr, low, low + k).boxed().collect(Collectors.toList());
}
{% endhighlight %}

[Kth Missing Positive Number][kth-missing-positive-number]

{% highlight java %}
public int findKthPositive(int[] arr, int k) {
    int low = 0, high = arr.length;
    while (low < high) {
        int mid = (low + high) >>> 1;
        // If there's no missing positive integer in index range [0, i]
        //  then arr[i] == i + 1
        // Therefore, arr[i] - i - 1 is the number of missing positive integers
        //
        // Let b[i] = arr[i] - i - 1,
        //  b[i + 1] - b[i] == arr[i + 1] - (i + 1) - 1 - (arr[i] - i - 1)
        //                  == arr[i + 1] - arr[i] - 1
        // Since arr[] is strictly increasing, arr[i + 1] - arr[i] >= 1
        //  b[i + 1] - b[i] >= 1 - 1 = 0
        // So b[i] is a sorted array. We can apply binary search to it. 
        if (arr[mid] - mid - 1 < k) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }

    // When the loop exits, low == high
    // From the binary search process, we know for index i in range [0, low),
    //  the number of missing positive integers is arr[i] - i - 1 < k
    //  arr[i] < k + i + 1 <= k + low
    //
    // The possible value of low when binary search finishes is low <= arr.length
    //  if low < arr.length, since low == high, we have arr[low] - low - 1 >= k
    //  k + low <= arr[low] - 1 < arr[low]
    //  so arr[low] is outside of the number range [1, low + k]
    //
    // Therefore, in the number range of [1, low + k],
    //  only arr[0], arr[1], ..., arr[low - 1] are present, in total `low` positive integers
    //  the number of missing positive integers is (low + k) - low == k
    return low + k;
}
{% endhighlight %}

[H-Index II][h-index-ii]

{% highlight java %}
public int hIndex(int[] citations) {
    int low = 0, high = citations.length;
    while (low < high) {
        int mid = (low + high) >>> 1;
        // Find the first index i so that
        //  citations[i] >= citations.length - i
        //  where citations.length - i is the potential h
        //
        // Let b[i] = citations[i] + i
        //  b[i + 1] - b[i] == citations[i + 1] + (i + 1) - (citations[i] + i)
        //                  == citations[i + 1] - citations[i] + 1 > 0
        // So b[i] is strictly increasing
        if (citations[mid] >= citations.length - mid) {
            high = mid;
        } else {
            // citations[i] < citations.length - i
            //  so citations[i] < h, which contradicts "at least h citations EACH"
            low = mid + 1;
        }
    }
    return citations.length - low;
}
{% endhighlight %}

[Koko Eating Bananas][koko-eating-bananas]

{% highlight java %}
public int minEatingSpeed(int[] piles, int H) {
    int target = H - piles.length;
    int low = 1, high = 1_000_000_000;  // high can be max(piles)
    while (low < high) {
        int mid = (low + high) >>> 1;
        int sum = 0;
        for (int i = 0; i < piles.length; i++) {
            // Math.ceil((double)x / n) == (x - 1) / n + 1
            sum += (piles[i] - 1) / mid;
            if (sum > target) {
                break;
            }
        }

        if (sum > target) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }

    return low;
}
{% endhighlight %}


# Generalization

@zhijun_liao

Minimize `k`, s.t. `condition(k) == true`

{% highlight java %}
public int binarySearch(int[] arr) {
    int low = min(searchSpace), high = max(searchSpace);
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (condition(mid)) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return low;
}

private boolean condition(int value) {
}
{% endhighlight %}

[Minimum Number of Days to Make m Bouquets][minimum-number-of-days-to-make-m-bouquets]

{% highlight java %}
public int minDays(int[] bloomDay, int m, int k) {
    int max = 0;
    for (int d : bloomDay) {
        max = Math.max(max, d);
    }

    if (bloomDay.length < m * k) {
        return -1;
    }

    int low = 1, high = max;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (condition(bloomDay, m, k, mid)) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }

    return low;
}

private boolean condition(int[] bloomDay, int m, int k, int day) {
    int bouquet = 0, flower = 0;
    for (int d : bloomDay) {
        if (d <= day) {
            if (++flower % k == 0) {
                bouquet++;
            }
        } else {
            flower = 0;
        }
    }
    return bouquet >= m;
}
{% endhighlight %}

[Split Array Largest Sum][split-array-largest-sum]

{% highlight java %}
public int splitArray(int[] nums, int m) {
    int sum = 0, max = 0;
    for (int num : nums) {
        sum += num;
        max = Math.max(max, num);
    }

    int low = max, high = sum;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (condition(nums, mid, m)) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }        
    return low;
}

/**
 * Check whether with the given largest subarray sum, the array can be split into m subrrays.
 * @param nums original array
 * @param s largest subarray sum
 * @param m number of subarrays
 * @return true if the array can be split into m subarrays, otherwise false
 */
private boolean condition(int[] nums, int s, int m) {
    int count = 1, sum = 0;
    for (int num : nums) {
        if (sum + num > s) {
            count++;
            sum = 0;
        }
        sum += num;
    }

    // count is the min number of subarrays that nums can be split into
    // and the sum of each subarray is no more than s
    return count <= m;
}
{% endhighlight %}

Similarly,

Maximize `k`, s.t. `condition(k) == true`

{% highlight java %}
public int binarySearch(int[] arr) {
    int low = min(searchSpace), high = max(searchSpace);
    while (low < high) {
        int mid = low + (high - low + 1) / 2;
        if (condition(mid)) {
            low = mid;
        } else {
            high = mid - 1;
        }
    }
    return low;
}

private boolean condition(int value) {
}
{% endhighlight %}
[Magnetic Force Between Two Balls][magnetic-force-between-two-balls]

{% highlight java %}
public int maxDistance(int[] position, int m) {
    Arrays.sort(position);

    int low = 0, high = position[position.length - 1] - position[0];
    while (low < high) {
        int mid = low + (high - low + 1) / 2;
        if (condition(position, mid, m)) {
            low = mid;
        } else {
            high = mid - 1;
        }
    }

    return low;
}

/**
 * Counts the max number of balls can be placed into baskets with the given minimum distance.
 * This function is monotonically decreasing with respect to d.
 * @param position basket postitions
 * @param d minimum distance between any two balls
 * @return true if number of balls can be places into baskets is no less than m, otherwise false
 */
private boolean condition(int[] position, int d, int m) {
    // Always place first object at position[0]
    // This ensures we can place the most balls
    int count = 1, curr = position[0];
    for (int i = 1; i < position.length; i++) {
        if (position[i] - curr >= d) {
            count++;
            curr = position[i];
        }
    }

    return count >= m;
}
{% endhighlight %}

# Java
## Arrays
[public static \<T\> int binarySearch(T\[\] a, int fromIndex, int toIndex, T key, Comparator\<? super T\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#binarySearch-T:A-int-int-T-java.util.Comparator-)

The *insertion point* is defined as the point at which the key would be inserted into the array: the index of the first element in the range **greater** than the key, or toIndex if all elements in the range are less than the specified key.

## Collections
[public static \<T\> int binarySearch(List\<? extends T\> list, T key, Comparator\<? super T\> c)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#binarySearch-java.util.List-T-java.util.Comparator-)

[binary-search]: https://leetcode.com/problems/binary-search/
[find-k-closest-elements]: https://leetcode.com/problems/find-k-closest-elements/
[first-bad-version]: https://leetcode.com/problems/first-bad-version/
[h-index-ii]: https://leetcode.com/problems/h-index-ii/
[koko-eating-bananas]: https://leetcode.com/problems/koko-eating-bananas/
[kth-missing-positive-number]: https://leetcode.com/problems/kth-missing-positive-number/
[magnetic-force-between-two-balls]: https://leetcode.com/problems/magnetic-force-between-two-balls/
[minimum-number-of-days-to-make-m-bouquets]: https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/
[split-array-largest-sum]: https://leetcode.com/problems/split-array-largest-sum/
[search-insert-position]: https://leetcode.com/problems/search-insert-position/
[the-k-weakest-rows-in-a-matrix]: https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/
