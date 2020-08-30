---
layout: post
title:  "Tree Recursion"
tags: tree
---
# Global State

[Convert BST to Greater Tree][convert-bst-to-greater-tree]

{% highlight java %}
private int sum = 0;  // global state

public TreeNode convertBST(TreeNode root) {
    if (root != null) {
        convertBST(root.right);
        sum += root.val;
        root.val = sum;
        convertBST(root.left);
    }
    return root;
}
{% endhighlight %}

[Minimum Absolute Difference in BST][minimum-absolute-difference-in-bst]

{% highlight java %}
private int d = Integer.MAX_VALUE;
private TreeNode prev = null;

public int getMinimumDifference(TreeNode root) {
    if (root == null) {
        return Integer.MAX_VALUE;
    }

    getMinimumDifference(root.left);

    // inorder
    if (prev != null) {
        d = Math.min(d, root.val - prev.val);
    }
    prev = root;

    getMinimumDifference(root.right);

    return d;
}
{% endhighlight %}

[Diameter of Binary Tree][diameter-of-binary-tree]

{% highlight java %}
private int diameter = 0;

public int diameterOfBinaryTree(TreeNode root) {
    height(root);
    return diameter;
}

private int height(TreeNode node) {
    if (node == null) {
        return 0;
    }

    int left = height(node.left);  // DFS
    int right = height(node.right);
    diameter = Math.max(diameter, left + right);
    return Math.max(left, right) + 1;
}
{% endhighlight %}

[Longest Univalue Path][longest-univalue-path]

{% highlight java %}
private int path = 0;

public int longestUnivaluePath(TreeNode root) {
    length(root);
    return path;
}

/**
 * Max length of univalue paths which start from node.
 * @param node the root of the subtree
 * @return the max length of univalue paths starting from node
 */
private int length(TreeNode node) {
    if (node == null) {
        return 0;
    }

    // max univalue paths from the child nodes
    int left = length(node.left);
    int right = length(node.right);

    // max univalue paths from the current node
    int leftPath = 0, rightPath = 0;
    if (node.left != null && node.left.val == node.val) {
        leftPath = left + 1;
    }
    if (node.right != null && node.right.val == node.val) {
        rightPath = right + 1;
    }

    path = Math.max(path, leftPath + rightPath);
    return Math.max(leftPath, rightPath);
}
{% endhighlight %}

[Find Bottom Left Tree Value][find-bottom-left-tree-value]

{% highlight java %}
private int bottomLeft = 0;
private int depth = -1;

public int findBottomLeftValue(TreeNode root) {
    dfs(root, 0);
    return bottomLeft;
}

private void dfs(TreeNode node, int d) {
    if (depth < d) {
        bottomLeft = node.val;
        depth = d;
    }

    if (node.left != null) {
        dfs(node.left, d + 1);
    }
    if (node.right != null) {
        dfs(node.right, d + 1);
    }

    return;
}
{% endhighlight %}

Another solution is right-to-left-BFS.

[Longest ZigZag Path in a Binary Tree][longest-zigzag-path-in-a-binary-tree]

{% highlight java %}
public int longestZigZag(TreeNode root) {
    return dfs(root)[2];
}

private int[] dfs(TreeNode root) {
    if (root == null) {
        // max zigzag path at:
        // [0]: left child node
        // [1]: right child node
        // [2]: this node
        return new int[]{-1, -1, -1};
    }

    int[] left = dfs(root.left);
    int[] right = dfs(root.right);

    // left[1], right[0] makes the path zigzag
    // leaves will have zigzag path == -1 + 1 == 0
    int path = Math.max(Math.max(left[1], right[0]) + 1, Math.max(left[2], right[2]));

    return new int[]{left[1] + 1, right[0] + 1, path};
}
{% endhighlight %}

[Second Minimum Node in a Binary Tree][second-minimum-node-in-a-binary-tree]

{% highlight java %}
private int min;
private long secondMin = Long.MAX_VALUE;

public int findSecondMinimumValue(TreeNode root) {
    // root has the minimum value
    min = root.val;
    dfs(root);
    return secondMin < Long.MAX_VALUE ? (int)secondMin : -1;
}

private void dfs(TreeNode node) {
    if (node == null) {
        return;
    }

    if (min < node.val && node.val < secondMin) {
        secondMin = node.val;
    } else if (min == node.val) {
        dfs(node.left);
        dfs(node.right);
    }
}
{% endhighlight %}

{% highlight java %}
public int findSecondMinimumValue(TreeNode root) {
    // tree leaf
    if (root.left == null) {
        return -1;
    }

    // The second minimum value on left path including root
    int left = root.left.val == root.val ? findSecondMinimumValue(root.left) : root.left.val;
    // The second minimum value on right path including root
    int right = root.right.val == root.val ? findSecondMinimumValue(root.right) : root.right.val;

    // if left == -1 && right == -1, returns -1
    // else left == -1 || right == -1, returns none -1
    // else returns the lesser of the two second minimum values
    return left == -1 || right == -1 ? Math.max(left, right) : Math.min(left, right);
}
{% endhighlight %}

[Lowest Common Ancestor of Deepest Leaves][lowest-common-ancestor-of-deepest-leaves]

{% highlight java %}
public TreeNode lcaDeepestLeaves(TreeNode root) {
    return dps(root).getValue();
}

// <depth, lowest_common_ancestor>
private Pair<Integer, TreeNode> dps(TreeNode node) {
    if (node == null) {
        return new Pair(0, null);
    }

    Pair<Integer, TreeNode> l = dps(node.left), r = dps(node.right);

    int d1 = l.getKey(), d2 = r.getKey();
    return new Pair(Math.max(d1, d2) + 1, d1 == d2 ? node : d1 > d2 ? l.getValue() : r.getValue());
}
{% endhighlight %}

[N-ary Tree Level Order Traversal][n-ary-tree-level-order-traversal]

{% highlight java %}
private List<List<Integer>> list;

public List<List<Integer>> levelOrder(Node root) {
    list = new ArrayList<>();
    dfs(root, 0);
    return list;
}

private void dfs(Node node, int level) {
    if (node == null) {
        return;
    }

    if (list.size() == level) {
        list.add(new ArrayList<>());
    }
    list.get(level).add(node.val);

    for (Node child : node.children) {
        dps(child, level + 1);
    }
}
{% endhighlight %}

Intuitively BFS also works for level order traversal. The condition to increment level is:

{% highlight java %}
int size = q.size();
List<Integer> currLevel = new ArrayList<>();
for (int i = 0; i < size; i++) {
    Node node = q.poll();
    currLevel.add(node.val);
    for (Node child : node.children) {
        q.offer(child);
    }
}
{% endhighlight %}

[convert-bst-to-greater-tree]: https://leetcode.com/problems/convert-bst-to-greater-tree/
[diameter-of-binary-tree]: https://leetcode.com/problems/diameter-of-binary-tree/
[find-bottom-left-tree-value]: https://leetcode.com/problems/find-bottom-left-tree-value/
[longest-univalue-path]: https://leetcode.com/problems/longest-univalue-path/solution/
[longest-zigzag-path-in-a-binary-tree]: https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/
[lowest-common-ancestor-of-deepest-leaves]: https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/
[minimum-absolute-difference-in-bst]: https://leetcode.com/problems/minimum-absolute-difference-in-bst/
[n-ary-tree-level-order-traversal]: https://leetcode.com/problems/n-ary-tree-level-order-traversal/
[second-minimum-node-in-a-binary-tree]: https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/
