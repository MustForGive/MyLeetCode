# 二叉树前、中、后序的（递归、迭代）遍历方法-Java
## 递归方法
    递归方法还是很简单，不需要我们多做什么，电脑已经自己完成的栈的实现。

```java
//三种遍历的递归方式
class Solution {
    public List<Integer> preorderTraversal(TreeNode rootds) {
        List<Integer> res = new ArrayList<>();
        ds(root, res);
        return res;
    }

    private void ds(TreeNode root, List<Integer> res) {
        if (root == null) return;
        //前序遍历
        res.add(root.val);
        ds(root.left, res);
        ds(root.right, res);
        //中序遍历
        ds(root.left, res);
        res.add(root.val);
        ds(root.right, res);
        //后序遍历
        ds(root.left, res);
        ds(root.right, res);
        res.add(root.val);
    }
}
```
## 迭代法
    就是遍历二叉树，所以题目略......
### 二叉树的前序遍历
    前序遍历：中，左，右
#### 主要思路
1. 从根节点开始，向`左子树`开始遍历，直到`为空`，每拿到一个`节点`，都将节点存储在`stack`和`res`中。
2. 直到它的`节点`为空时，开始遍历`右子树`。
3. 遍历所有结果
```java
//迭代
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        Deque<TreeNode> stack = new LinkedList<>();//用来存节点的栈
        TreeNode p = root;//可有可无
        List<Integer> res = new ArrayList<>();//存储最后结果的数组
        while(p !=null || !stack.isEmpty()){
            while(p!=null){
                stack.push(p);//将每个向左遍历的节点都存入栈中
                res.add(p.val);
                p=p.left;
            }
            p= stack.pop().right;
        }
        return res;
    }
}
```

### 二叉树的中序遍历
    中序遍历：左，右，中
#### 主要思路
1. 整体思路对比中序遍历依然是使用`栈`来进行遍历
2. 些许不同，每次存储`结果`进入`res`时，先将栈中的节点`pop()`
```java
//迭代
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode p = root;
        while(p!=null || !stack.isEmpty() ){
            while(p!=null){
                stack.push(p);
                p=p.left;
            }
            p=stack.pop();
            res.add(p.val);
            p=p.right;
        }
        return res;
    }
}
```
### 二叉树的后序遍历
    后序遍历：左，右，中
#### 主要思路
1. 由上面的`前序遍历`我们直到，中序遍历是：`中，左，右`
2. 那我们可以试试将前序遍历逆序输出是什么样：`右，左，中`
3. 此时离最后真正的`后序遍历`还有一点小改动，我们此时只需要将`从左到右`遍历改为`从右到左`遍历
4. 至此我们改动`前序遍历`成为`后序遍历`
```java
//迭代
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<Integer> list = new LinkedList<>();
        Deque<TreeNode> stack = new LinkedList<>();
        while(root!=null || !stack.isEmpty()){
            while(root!=null){
                list.addFirst(root.val);//将每次的结果插入链前
                stack.push(root);
                root=root.right;
            }
            root = stack.pop().left;
        }
        return list;
    }
}
```
