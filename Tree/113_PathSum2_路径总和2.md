# 113 Path Sum2  路径总和2

### 代码
```java
//递归回溯
class Solution {
    List<List<Integer>> res =new LinkedList<>();
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
    helper(root,sum,new ArrayList<Integer>());
    return res;
    }
    public void helper(TreeNode root,int sum,List<Integer> curr_list){
        if(root==null) return;
        sum-=root.val;
        curr_list.add(root.val);
        if(root.left==null && root.right==null && sum==0){
            res.add(new ArrayList<Integer>(curr_list));
            curr_list.remove(curr_list.size()-1);
            return;
        }
        if(root.left!=null) helper(root.left,sum,curr_list);
        if(root.right!=null) helper(root.right,sum,curr_list);
        curr_list.remove(curr_list.size()-1);
    }
}
```