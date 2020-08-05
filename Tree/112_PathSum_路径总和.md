# 112 Path Sum 路径综合


### 代码

```java
//递归
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null) return false;
        sum-=root.val;
        if(root.left ==null && root.right==null)
        return (sum==0);
        return (hasPathSum(root.left,sum)||hasPathSum(root.right,sum));
    }
}
```

```java
//BFS迭代
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        LinkedList<String> paths = new LinkedList();
        if (root == null)
            return false;
        LinkedList<TreeNode> node_que = new LinkedList();
        LinkedList<Integer> sum_que = new LinkedList();
        node_que.add(root);
        sum_que.add(sum-root.val);
        TreeNode node;
        while(!node_que.isEmpty()){
            node = node_que.poll();
            int  curr_sum = sum_que.poll();

            if(node.left==null && node.right==null&&curr_sum==0){
                return true;
            }
            if(node.left!=null) {
                sum_que.add(curr_sum-node.left.val);
                node_que.add(node.left);
            }
            if(node.right!=null){
                sum_que.add(curr_sum-node.right.val);
                node_que.add(node.right);
            } 
        }
        return false;
    }
}
```
```java
//DFS迭代
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        LinkedList<String> paths = new LinkedList();
        if (root == null)
            return false;
        LinkedList<TreeNode> node_stack = new LinkedList();
        LinkedList<Integer> sum_stack = new LinkedList();
        node_stack.add(root);
        sum_stack.add(sum-root.val);
        TreeNode node;
        while(!node_stack.isEmpty()){
            node = node_stack.pop();
            int  curr_sum = sum_stack.pop();
            if(node.left==null && node.right==null&&curr_sum==0){
                return true;
            }
            if(node.right!=null) {
                sum_stack.add(curr_sum-node.right.val);
                node_stack.add(node.right);
            }
            if(node.left!=null){
                sum_stack.add(curr_sum-node.left.val);
                node_stack.add(node.left);
            } 
        }
        return false;
    }
}
```