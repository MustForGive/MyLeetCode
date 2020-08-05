# 299 猜数字游戏

>给定一个未排序的整数数组，找出其中没有出现的最小的正整数。

```zh-cn
示例 1:

输入: [1,2,0]
输出: 3
示例 2:

输入: [3,4,-1,1]
输出: 2
示例 3:

输入: [7,8,9,11,12]
输出: 1

```

## 需求分析

有两种数据需要处理

1. 相同位置 且 数字相同

2. 位置不同 且 数字相同

3. 因为数字都为 0-9 故用一个“桶”来存储secret 位置不同且数字相同 出现的元素

## 整体思路

1. 用一个 变量 来存其中 相同位置且数字相同 的数,a++.

2. 不相同的数则用一个 桶来存储。

3. 遍历数组，用 guess 中的数比较桶中的数，如果相同，则将桶中-- 且 b++;

## 代码

```java

class Solution {
    public String getHint(String secret, String guess) {
        int a=0;
        int b=0;
        int[] secretBucket = new int[10];
        for (int i = 0; i < guess.length(); i++) {
            if (secret.charAt(i)==guess.charAt(i)) {
                a++;
            }else{
                secretBucket[secret.charAt(i)-'0']++;
            }
        }
        for (int i = 0; i < guess.length(); i++) {
            if (secret.charAt(i)!=guess.charAt(i)&&secretBucket[guess.charAt(i)-'0']>0) {
                b++;
                secretBucket[guess.charAt(i)-'0']--;
            }
        }
        return ""+a+"A"+b+"B";
    }
}

```