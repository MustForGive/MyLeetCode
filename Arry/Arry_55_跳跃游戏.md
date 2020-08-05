# 55 跳跃游戏

>问题

```zh-cn
 * 给定一个非负整数数组，你最初位于数组的第一个位置。
 *
 * 数组中的每个元素代表你在该位置可以跳跃的最大长度。
 *
 * 判断你是否能够到达最后一个位置。
```

>案例

```zh-cn
  示例 1:
  
  输入: [2,3,1,1,4]
  输出: true
  解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
  
  
  示例 2:
  
  输入: [3,2,1,0,4]
  输出: false
  解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达 后一个位置。
```

## 方法一：回溯法

>方法概要

 我们可以通过从 左向右 对每一种可能进行递归遍历，看其是否能到达最终点

>需求分析

我们需要设计一个递归回溯的程序：需要终止条件、循环体

>整体思路

1. 当 当前的下标到了最后一个下标时，将返回true

2. 下标的最大位置为 furthestJump=当前下标 + 下标的元素

3. 如果furthestJump 比 数组长度都大，则将数组长度设为最大位置。

4. 循环将当前的下标 增加 到其最大长度

>代码

```java
public class Solution {
    public boolean canJumpFromPosition(int position, int[] nums) {
        if (position == nums.length - 1) {
            return true;
        }

        int furthestJump = Math.min(position + nums[position], nums.length - 1);
        for (int nextPosition = position + 1; nextPosition <= furthestJump; nextPosition++) {
            if (canJumpFromPosition(nextPosition, nums)) {
                return true;
            }
        }

        return false;
    }

    public boolean canJump(int[] nums) {
        return canJumpFromPosition(0, nums);
    }
}

```

## 方法二：动态规划-自顶向下

>方法概述

自顶向下的动态规划更像是回溯法的优化，当我们理解完回溯法后，我们发现。当一个元素被定为能到达目的后，它就不会再改变。

所以我们可以采用记忆化的方式，将各个元素分为：GOOD、BAD、UNKNOW。通过另外一个数组对元素进行标记

>需求分析

1. 我们需要设计递归回溯的模式。

2. 设计终止条件

3. 对各个元素进行 记忆化 标记。

>整体思路

1. 将 记忆化 数组均初始化为UNKNOW

2. 优化递归算法，每步计算之前，会先判断这个元素的 记忆化存储
    1. 若已知结果 直接返回结果 true\false

    2. 若不知，则按步骤继续

```java
enum Index {
    GOOD, BAD, UNKNOWN
}

public class Solution {
    Index[] memo;

    public boolean canJumpFromPosition(int position, int[] nums) {
        if (memo[position] != Index.UNKNOWN) {
            return memo[position] == Index.GOOD ? true : false;
        }

        int furthestJump = Math.min(position + nums[position], nums.length - 1);
        for (int nextPosition = position + 1; nextPosition <= furthestJump; nextPosition++) {
            if (canJumpFromPosition(nextPosition, nums)) {
                memo[position] = Index.GOOD;
                return true;
            }
        }

        memo[position] = Index.BAD;
        return false;
    }

    public boolean canJump(int[] nums) {
        memo = new Index[nums.length];
        for (int i = 0; i < memo.length; i++) {
            memo[i] = Index.UNKNOWN;
        }
        memo[memo.length - 1] = Index.GOOD;
        return canJumpFromPosition(0, nums);
    }
}

```

## 方法三：动态规划-自底向下

>方法概述

自底向下与自顶向下区别就是 消除了回溯。更具有时间效率，以及空间效率

通常 自底向上就是自顶向下的反转

我们需要 自右向左 的遍历数组。

>需求分析

与上面需求大体相同，只是消除了递归 且 将自右向左的遍历数组

>整体思路

1. 需要额外的数组进行 记忆化 存储

2. 最后一个下标需要标为 GOOD

3. 从右向左 两层循环的遍历数组
    1. 根据已知结果，如果当前下标能到达 元素为GOOD 的下标，则标当前下标元素为GOOD

    2. 若未知，则按正常步骤计算

>代码

```java
enum Index {
    GOOD, BAD, UNKNOWN
}

public class Solution {
    public boolean canJump(int[] nums) {
        Index[] memo = new Index[nums.length];
        for (int i = 0; i < memo.length; i++) {
            memo[i] = Index.UNKNOWN;
        }
        memo[memo.length - 1] = Index.GOOD;

        for (int i = nums.length - 2; i >= 0; i--) {
            int furthestJump = Math.min(i + nums[i], nums.length - 1);
            for (int j = i + 1; j <= furthestJump; j++) {
                if (memo[j] == Index.GOOD) {
                    memo[i] = Index.GOOD;
                    break;
                }
            }
        }

        return memo[0] == Index.GOOD;
    }
}

```

## 方法三：贪心法

>方法概述

通过自底向上的方法，我们可以看出，我们只需要 从右向左 遍历。

该元素能否到达与其最远到达的下标 和 最近的到达最后坐标有关

所以，我们只需要标记出最近出现的 能到达最后坐标 的元素即可。

>需求分析

1. 需要从右向左循环遍历数组

2. 若 到达的最远坐标 > 其最近出现的到达最后坐标(lastPos) ，即证明这条路可通

3. 最终只需要确定 lastPos 是否为 0 即可

>整体思路

同上

>代码

```java
public class Solution {
    public boolean canJump(int[] nums) {
        int lastPos = nums.length - 1;
        for (int i = nums.length - 1; i >= 0; i--) {
            if (i + nums[i] >= lastPos) {
                lastPos = i;
            }
        }
        return lastPos == 0;
    }
}

```

