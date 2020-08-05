# 41 缺失的第一个正数

>给定一个未排序的整数数组，找出其中没有出现的最小的正整数。

```zh-cn
示例 1:

输入: [1,2,0]
输出: 3


示例 2:

输入: [3,4,-1,1]
输出: 2
```

![缺失正数示意图](./assets/缺失正数示意图.png)

## 数据预处理

首先可以不用考虑三类数： 0 、 <0 、 >n  

因为缺失的数一定是 正数 且一定 小于n+1

我们可以通过将这三类数用 “1” 代替

## 如何实现就地算法

当读到数字 a 时，我们只需将第 a 个数组元素进行变负

例如：

当nums[2] 为负数时，就代表 2 这个数字已经在数组中出现

注意:

由于 n（即数组长度） 数组中没有第 n 个元素，可以将第 0 个元素设置为第 n 个元素。

即 nums[0] = n

## 整体思路

1.遍历数组，看是否存在1，若无，则直接返回 1

2.若数组中存在 1 且数组长度也只为 1 时，返回 2

3.将数组中所有的 负数、 0 、 大于N 的数均赋值为 1

4.每次出现的数字 a 将数组中第 a 个元素设为 负数

5.依次遍历数组，数组中出现的第一个的正数则为缺失的第一个正数

6.若数组中没有，则检查 nums[0] 即 n 是否为 负数，为负数则返回n

7.以上结果都没有的话，则返回 n+1

```java

class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;

        int contains = 0;
        for (int i = 0; i < nums.length; i++) {
            if(nums[i]==1){
                contains++;
                break;
            }
        }
        if (contains == 0) {
            return 1;
        }
        if (n==1) {
            return 2;
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i]<=0||nums[i]>n) {
                nums[i]=1;
            }
        }

        for (int i = 0; i < nums.length; i++) {
            int a = Math.abs(nums[i]);

            if (a==n) {
                nums[0]= -Math.abs(nums[0]);
            } else {
                nums[a]= -Math.abs(nums[a]);
            }
        }
        for (int i = 1; i < nums.length; i++) {
            if (nums[i]>0) {
                return i;
            }
        }

        if (nums[0]>0) {
            return n;
        }

        return n+1;

    }
}

```
