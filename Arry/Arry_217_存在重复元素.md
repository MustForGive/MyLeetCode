# 217 存在重复元素

>给定一个整数数组，判断是否存在重复元素。

>如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。

## 示例

```zh-cn
示例 1:

输入: [1,2,3,1]
输出: true

示例 2:

输入: [1,2,3,4]
输出: false
```

一道很简单的题，最重要的是要有思路。

>需求分析

1. 只要找出两个重复的数即可。

2. 直接暴力法肯定会超时。需要找出更为简便的方法去找出重复的数

>方法一 ： 排序法

>整体思路

1. 只需将数组排序然后，查找相邻两个元素是否重复即可

>代码

```java
class solution{
    public boolean containsDuplicate(int[] nums){
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
}

```

>方法二 ： 通过 hash 的方法将其找出

>整体思路

1. 使用更加快速的数据结构用作动态集合 -- 哈希表 他们的 search 和 insert 的平均时间复杂度O(1)

```java
public boolean containsDuplicate(int[] nums) {
    Set<Integer> set = new HashSet<>(nums.length);
    for (int x: nums) {
        if (set.contains(x)) return true;
        set.add(x);
    }
    return false;
}
```
