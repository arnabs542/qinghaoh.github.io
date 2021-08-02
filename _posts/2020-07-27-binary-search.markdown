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

[Find Minimum in Rotated Sorted Array][find-minimum-in-rotated-sorted-array]

{% highlight java %}
public int findMin(int[] nums) {
    int low = 0, high = nums.length - 1;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (nums[mid] < nums[high]) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return nums[low];
}
{% endhighlight %}

[Find Minimum in Rotated Sorted Array II][find-minimum-in-rotated-sorted-array-ii]

{% highlight java %}
public int findMin(int[] nums) {
    int low = 0, high = nums.length - 1;
    while (low < high) {
        int mid = (low + high) / 2;
        if (nums[mid] > nums[high]) {
            low = mid + 1;
        } else if (nums[mid] < nums[high]) {
            high = mid;
        } else {
            // finds the pivot index
            if (nums[high - 1] > nums[high]) {
                low = high;
                break;
            }
            high--;
        }
    }
    return nums[low];
}
{% endhighlight %}

[Search in Rotated Sorted Array][search-in-rotated-sorted-array]

{% highlight java %}
public int search(int[] nums, int target) {
    int minIndex = findMinIndex(nums);
    if (target == nums[minIndex]) {
        return minIndex;
    }

    int low = minIndex, high = minIndex - 1 + nums.length;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (nums[mid % nums.length] >= target) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return nums[low % nums.length] == target ? low % nums.length : -1;
}

private int findMinIndex(int[] nums) {
    int low = 0, high = nums.length - 1;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (nums[mid] < nums[high]) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return low;
}
{% endhighlight %}

[Search in Rotated Sorted Array II][search-in-rotated-sorted-array-ii]

## Local Monotocity

[Find Peak Element][find-peak-element]

{% highlight java %}
public int findPeakElement(int[] nums) {
    int low = 0, high = nums.length - 1;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (nums[mid] > nums[mid + 1]) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return low;
}
{% endhighlight %}

[Find a Peak Element II][find-a-peak-element-ii]

{% highlight java %}
public int[] findPeakGrid(int[][] mat) {
    // binary search on columns
    int low = 0, high = mat[0].length - 1;
    while (low < high) {
        int mid = (low + high) >>> 1;

        int row = findMaxRow(mat, mid);
        if (mat[row][mid] > mat[row][mid + 1]) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }

    return new int[]{findMaxRow(mat, low), low};
}

// finds the max row in column mid
private int findMaxRow(int[][] mat, int col) {
    int row = 0;
    for (int i = 0; i < mat.length; i++) {
        if (mat[i][col] > mat[row][col]) {
            row = i;
        }
    }
    return row;
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

// f(x) is monotonically increasing
private boolean condition(int x) {
    return f(x) >= 0;
}
{% endhighlight %}

[Search Insert Position][search-insert-position]

{% highlight java %}
public int searchInsert(int[] nums, int target) {
    int low = 0, high = nums.length;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (nums[mid] >= target) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return low;
}
{% endhighlight %}

[Fixed Point][fixed-point]

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

[Kth Smallest Element in a Sorted Matrix][kth-smallest-element-in-a-sorted-matrix]

{% highlight java %}
// O(n * log(max - min))
public int kthSmallest(int[][] matrix, int k) {
    int low = matrix[0][0], high = matrix[matrix.length - 1][matrix[0].length - 1];
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (condition(matrix, mid, k)) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return low;
}

private boolean condition(int[][] matrix, int value, int k) {
    // starts from top-right
    int count = 0, i = 0, j = matrix[0].length - 1;
    while (i < matrix.length && j >= 0) {
        if (matrix[i][j] > value) {
            j--;
        } else {
            count += j + 1;
            i++;
        }
    }
    return count >= k;
}
{% endhighlight %}

Variant:

[Kth Smallest Prime Fraction][k-th-smallest-prime-fraction]

{% highlight java %}
public int[] kthSmallestPrimeFraction(int[] A, int K) {
    double low = 0, high = 1;
    int p = 0, q = 1;

    int count = 0;
    while (count != K) {
        double mid = (low + high) / 2;

        // starts from top-right
        int i = 0, j = A.length - 1;
        count = 0;  // count of fractions less than mid
        p = 0;
        while (i < A.length && j >= 0) {
            if (A[i] > mid * A[A.length - 1 - j]) {
                j--;
            } else {
                // p / q < curr
                // finds the largest fraction less than mid
                if (p * A[A.length - 1 - j] < q * A[i]) {
                    p = A[i];
                    q = A[A.length - 1 - j];
                }
                count += j + 1;
                i++;
            }
        }

        if (count < K) {
            low = mid;
        } else if (count > K) {
            high = mid;
        }
    }

    return new int[]{p, q};
}
{% endhighlight %}

[Kth Missing Positive Number][kth-missing-positive-number]

{% highlight java %}
public int findKthPositive(int[] arr, int k) {
    int low = 0, high = arr.length;
    while (low < high) {
        int mid = (low + high) >>> 1;
        // if there's no missing positive integer in index range [0, i]
        // arr[i] == i + 1
        //
        // let a[i] = arr[i] - i - 1
        // a[i] is the number of missing positive integers
        // a[i + 1] - a[i] == arr[i + 1] - (i + 1) - 1 - (arr[i] - i - 1)
        //                 == arr[i + 1] - arr[i] - 1 >= 0
        // so a[i] is increasing
        if (arr[mid] - mid - 1 >= k) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }

    // kth -> +k
    return low + k;
}
{% endhighlight %}

[Missing Element in Sorted Array][missing-element-in-sorted-array]

[Missing Number In Arithmetic Progression][missing-number-in-arithmetic-progression]

{% highlight java %}
public int missingNumber(int[] arr) {
    int n = arr.length, d = (arr[n - 1] - arr[0]) / n, low = 0, high = n;

    while (low < high) {
        int mid = (low + high) >>> 1;

        if (arr[mid] == arr[0] + d * mid) {
            // it's easy to prove arr[i] - arr[0] - d * i is monotonic
            // no matter whether d >= 0 or d < 0
            // all numbers up to mid are present
            low = mid + 1;
        } else {
            // a number is missing <= mid
            high = mid;
        }
    }

    return arr[0] + d * low;
}
{% endhighlight %}

[H-Index II][h-index-ii]

{% highlight java %}
public int hIndex(int[] citations) {
    int low = 0, high = citations.length;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (condition(citations, mid)) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return citations.length - low;
}

/**
 * Checks if the author has (N - lower) papers that have at least (N - lower) citations each.
 * @param lower number of papers with lower citations. When h is valid, lower == N - h
 * @return true if the condition is met, otherwise false
 */
private boolean condition(int[] citations, int lower) {
    // a[i] = citations[i] + i
    // a[i + 1] - a[i] == citations[i + 1] + (i + 1) - (citations[i] + i)
    //                 == citations[i + 1] - citations[i] + 1 > 0
    // so a[i] is strictly increasing
    //
    // 1. citations[N - h] >= h: h of his/her N papers have at least h citations each
    // 2. citations[N - h - 1] <= h: the other N - h papers have no more than h citations each
    //
    // Now we will prove we only need to finds the minimum value of lower that satisfies #1.
    // If #1 is true, we have:
    //  h == citations.length - lower
    //  citations[lower] == h
    //
    // Then #2 is true, too:
    //  citations[N - h - 1] = citations[lower - 1] <= citations[lower] == h
    return citations[lower] >= citations.length - lower;
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

[Search Suggestions System][search-suggestions-system]

{% highlight java %}
public List<List<String>> suggestedProducts(String[] products, String searchWord) {
    Arrays.sort(products);

    List<List<String>> result = new ArrayList<>();
    for (int i = 1; i <= searchWord.length(); i++) {
        int low = 0, high = products.length - 1;
        String prefix = searchWord.substring(0, i);
        while (low < high) {
            int mid = (low + high) >>> 1;
            if (condition(products, mid, prefix)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }

        List<String> list = new ArrayList<>();
        for (int j = low; j < Math.min(low + 3, products.length); j++) {
            if (products[j].startsWith(prefix)) {
                list.add(products[j]);
            }
        }
        result.add(list);
    }

    return result;
}

private boolean condition(String[] products, int index, String prefix) {
    return products[index].compareTo(prefix) >= 0;
}
{% endhighlight %}

[Find K Closest Elements][find-k-closest-elements]

Search for the first index from which the `k`-element sliding window starts.

{% highlight java %}
public List<Integer> findClosestElements(int[] arr, int k, int x) {
    int low = 0, high = arr.length - k;
    while (low < high) {
        int mid = (low + high) >>> 1;
        if (arr[mid + k] - x >= x - arr[mid]) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    return Arrays.stream(arr, low, low + k).boxed().collect(Collectors.toList());
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

// f(x) is monotonically decreasing
private boolean condition(int x) {
    return f(x) >= 0;
}
{% endhighlight %}

[Maximum Value at a Given Index in a Bounded Array][maximum-value-at-a-given-index-in-a-bounded-array]

{% highlight java %}
public int maxValue(int n, int index, int maxSum) {
    int low = 0, high = maxSum;
    while (low < high) {
        int mid = low + (high - low + 1) / 2;
        if (condition(mid, n, index, maxSum)) {
            low = mid;
        } else {
            high = mid - 1;
        }
    }
    return low;
}

private boolean condition(int x, int n, int index, long maxSum) {
    return sum(x, n, index) <= maxSum;
}

private long sum(int x, int n, int index) {
    return f(x, index + 1) + f(x, n - index) - x;
}

// formula: (1 + n) * n / 2
private long f(int x, int n) {
    // x > n: 2,3,
    // x < n: 1,1,1,2,3
    return x > n ? (long)(x + (x - n + 1)) * n / 2 : (long)(1 + x) * x / 2 + (n - x);
}
{% endhighlight %}

[Maximum Number of Removable Characters][maximum-number-of-removable-characters]

{% highlight java %}
public int maximumRemovals(String s, String p, int[] removable) {
    int low = 0, high = removable.length;
    while (low < high) {
        int mid = low + (high - low + 1) / 2;
        if (condition(s, p, removable, mid)) {
            low = mid;
        } else {
            high = mid - 1;
        }
    }
    return low;
}

private boolean condition(String s, String p, int[] removable, int k) {
    Set<Integer> set = new HashSet<>();
    for (int i = 0; i < k; i++) {
        set.add(removable[i]);
    }

    int i = 0, j = 0;
    while (i < s.length() && j < p.length()) {
        if (!set.contains(i) && s.charAt(i) == p.charAt(j)) {
            j++;
        }
        i++;
    }
    return j == p.length();
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

[Maximum Font to Fit a Sentence in a Screen][maximum-font-to-fit-a-sentence-in-a-screen]

{% highlight java %}
public int maxFont(String text, int w, int h, int[] fonts, FontInfo fontInfo) {
    int low = 0, high = fonts.length - 1;
    while (low < high) {
        int mid = low + (high - low + 1) / 2;
        if (condition(text, w, h, fonts, mid, fontInfo)) {
            low = mid;
        } else {
            high = mid - 1;
        }
    }
    return condition(text, w, h, fonts, low, fontInfo) ? fonts[low] : -1;
}

private boolean condition(String text, int w, int h, int[] fonts, int index, FontInfo fontInfo) {
    int font = fonts[index];
    if (fontInfo.getHeight(font) > h) {
        return false;
    }
    int sum = 0;
    for (char c : text.toCharArray()) {
        sum += fontInfo.getWidth(font, c);
    }
    return sum <= w;
}
{% endhighlight %}

[Maximum Width Ramp][maximum-width-ramp]

{% highlight java %}
public int maxWidthRamp(int[] A) {
    // decreasing list
    List<Integer> list = new ArrayList<>();
    int max = 0;
    for (int i = 0; i < A.length; i++) {
        int n = list.size();
        if (n == 0 || A[i] < A[list.get(n - 1)]) {
            list.add(i);
        } else {
            // binary searches for the first element
            // which is no greater than A[i]
            int low = 0, high = n - 1;
            while (low < high) {
                int mid = (low + high) >>> 1;
                if (A[list.get(mid)] <= A[i]) {
                    high = mid;
                } else {
                    low = mid + 1;
                }
            }
            max = Math.max(max, i - list.get(low));
        }
    }
    return max;
}
{% endhighlight %}

# Fraction

[Maximum Average Subarray II][maximum-average-subarray-ii]

{% highlight java %}
private static final double MAX_ERROR = 1e-5;

public double findMaxAverage(int[] nums, int k) {
    int n = nums.length;
    double min = nums[0], max = nums[0];
    for (int num : nums) {
        if (num < min) {
            min = num;
        }
        if (num > max) {
            max = num;
        }
    }

    // binary search the max avg between min and max
    while (min + MAX_ERROR < max) {
        double mid = min + (max - min) / 2.0;
        if (hasAvgAbove(nums, k, mid)) {
            min = mid;
        } else {
            max = mid;
        }
    }
    return min;
}

// Checks if there exists a subarray of nums whose length >= k
// and its average >= target
private boolean hasAvgAbove(int[] nums, int k, double target) {
    // avg(nums[i...j])
    // => (nums[i] + nums[i + 1] + ... + nums[j]) / (j - i + 1) >= target
    // => nums[i] + nums[i + 1] + ... + nums[j] >= target * (j - i + 1)
    // => (nums[i] - target) + (nums[i + 1] - target) + ... + (nums[j] - target) >= 0
    //
    // sum is monotonically decreasing (in terms of target)
    double sum = 0, outOfWindowSum = 0;
    for (int i = 0; i < k; i++) {
        sum += nums[i] - target;
    }

    // sliding window
    int i = k;
    while (i < nums.length) {
        if (sum >= 0) {
            return true;
        }

        sum += nums[i] - target;
        outOfWindowSum += nums[i - k] - target;

        // if out-of-windows sum is negative, subtract it
        if (outOfWindowSum < 0) {
            sum -= outOfWindowSum;
            outOfWindowSum = 0;
        }

        i++;
    }

    return sum >= 0;
}
{% endhighlight %}

# Java
## Arrays
[public static \<T\> int binarySearch(T\[\] a, int fromIndex, int toIndex, T key, Comparator\<? super T\> c)](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/Arrays.html#binarySearch(T%5B%5D,int,int,T,java.util.Comparator))

If the range contains multiple elements equal to the specified object, there is no guarantee which one will be found.

**Returns:**

index of the search key, if it is contained in the array within the specified range; otherwise, (-(insertion point) - 1). The *insertion point* is defined as the point at which the key would be inserted into the array: the index of the first element in the range **greater** than the key, or toIndex if all elements in the range are less than the specified key.

{% highlight java %}
if (insertionPoint < 0) {
    insertionPoint = ~insertionPoint;
}
{% endhighlight %}

## Collections
[public static \<T\> int binarySearch(List\<? extends T\> list, T key, Comparator\<? super T\> c)](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/Collections.html#binarySearch(java.util.List,T,java.util.Comparator))

[binary-search]: https://leetcode.com/problems/binary-search/
[find-k-closest-elements]: https://leetcode.com/problems/find-k-closest-elements/
[find-minimum-in-rotated-sorted-array]: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
[find-minimum-in-rotated-sorted-array-ii]: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/
[find-peak-element]: https://leetcode.com/problems/find-peak-element/
[find-a-peak-element-ii]: https://leetcode.com/problems/find-a-peak-element-ii/
[first-bad-version]: https://leetcode.com/problems/first-bad-version/
[fixed-point]: https://leetcode.com/problems/fixed-point/
[h-index-ii]: https://leetcode.com/problems/h-index-ii/
[koko-eating-bananas]: https://leetcode.com/problems/koko-eating-bananas/
[kth-missing-positive-number]: https://leetcode.com/problems/kth-missing-positive-number/
[kth-smallest-element-in-a-sorted-matrix]: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
[k-th-smallest-prime-fraction]: https://leetcode.com/problems/k-th-smallest-prime-fraction/
[magnetic-force-between-two-balls]: https://leetcode.com/problems/magnetic-force-between-two-balls/
[maximum-average-subarray-ii]: https://leetcode.com/problems/maximum-average-subarray-ii/
[maximum-font-to-fit-a-sentence-in-a-screen]: https://leetcode.com/problems/maximum-font-to-fit-a-sentence-in-a-screen/
[maximum-number-of-removable-characters]: https://leetcode.com/problems/maximum-number-of-removable-characters/
[maximum-value-at-a-given-index-in-a-bounded-array]: https://leetcode.com/problems/maximum-value-at-a-given-index-in-a-bounded-array/
[maximum-width-ramp]: https://leetcode.com/problems/maximum-width-ramp/
[minimum-number-of-days-to-make-m-bouquets]: https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/
[missing-element-in-sorted-array]: https://leetcode.com/problems/missing-element-in-sorted-array/
[missing-number-in-arithmetic-progression]: https://leetcode.com/problems/missing-number-in-arithmetic-progression/
[split-array-largest-sum]: https://leetcode.com/problems/split-array-largest-sum/
[search-insert-position]: https://leetcode.com/problems/search-insert-position/
[search-in-rotated-sorted-array]: https://leetcode.com/problems/search-in-rotated-sorted-array/
[search-in-rotated-sorted-array-ii]: https://leetcode.com/problems/search-in-rotated-sorted-array-ii/
[search-suggestions-system]: https://leetcode.com/problems/search-suggestions-system/
[the-k-weakest-rows-in-a-matrix]: https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/
