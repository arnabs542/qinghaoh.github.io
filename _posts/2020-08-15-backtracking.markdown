---
layout: post
title:  "Backtracking"
tags: dfs
---
[Permutations][permutations]

{% highlight java %}
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    backtrack(list, new ArrayList<>(), nums);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int[] nums) {
    if (tmpList.size() == nums.length) {
        list.add(new ArrayList<>(tmpList));
        return;
    }

    // increment
    for(int i = 0; i < nums.length; i++) { 
        if (!tmpList.contains(nums[i])) {
            tmpList.add(nums[i]);
            backtrack(list, tmpList, nums);
            tmpList.remove(tmpList.size() - 1);
        }
    }
}
{% endhighlight %}

{% highlight java %}
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    backtrack(list, Arrays.stream(nums).boxed().collect(Collectors.toList()), 0);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int index) {
    if (index == tmpList.size()) {
        list.add(new ArrayList<>(tmpList));
        return;
    }

    // swap
    for(int i = index; i < tmpList.size(); i++) {
        Collections.swap(tmpList, i, index);
        backtrack(list, tmpList, index + 1);
        Collections.swap(tmpList, index, i);
    }
}
{% endhighlight %}

[permutations]:https://leetcode.com/problems/permutations/
