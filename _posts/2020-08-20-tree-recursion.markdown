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

[convert-bst-to-greater-tree]: https://leetcode.com/problems/convert-bst-to-greater-tree/
[diameter-of-binary-tree]: https://leetcode.com/problems/diameter-of-binary-tree/
[longest-univalue-path]: https://leetcode.com/problems/longest-univalue-path/solution/
[minimum-absolute-difference-in-bst]: https://leetcode.com/problems/minimum-absolute-difference-in-bst/
