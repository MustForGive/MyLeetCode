# 桶排序_164最大间距

>问题概述

```zh-cn
 * 给定一个无序的数组，找出数组在排序之后，相邻元素之间最大的差值。
 *
 * 如果数组元素个数小于 2，则返回 0。
```

>示例

```zh-cn
 * 示例 1:
 *
 * 输入: [3,6,9,1]
 * 输出: 3
 * 解释: 排序后的数组是 [1,3,6,9], 其中相邻元素 (3,6) 和 (6,9) 之间都存在最大差值 3。
 *
 * 示例 2:
 *
 * 输入: [10]
 * 输出: 0
 * 解释: 数组元素个数小于 2，因此返回 0。
```

## 先前知识:桶排序

>方法概述：

`桶排序是一个线性复杂度排序方式，当题目中有要求时，可以使用桶排序`
桶排序体现了分治思想，通过创建若干个小桶，通过`某种映射`将待排序的元素放到桶中，使每个桶中的最大值都小于它后面桶的最小值，每个桶的元素数量尽可能的平均。然后对每个桶中元素进行排序，最后对所有桶中的元素依次输出。

>需求分析

需要三个很重要的变量

1. 确定桶的容量也就是桶的大小`BucketSize=（max-min）/(nums.length-1)`

2. 确定桶的个数：`BucketNum=(max-min)/BucketSize`

3. 确定元素`num`在桶中的下标`index=(num-min)/BucketSize`

## 根据桶排序解决最大间距

>整体思路

1. 定义桶，桶中还有最大值和最小值

2. 找出数组中的最大值，最小值

3. 通过最大值最小值确定`桶的容量`

4. 通过`桶的容量`确定`桶的个数`

5. 循环每个数组中的元素确定在桶中的`下标`然后将元素放入桶中。

>代码

```java
class Solution {
    public int maximumGap(int[] nums) {
        if(nums==null||nums.length<2)
            return 0;
        double min=nums[0];
        double max=nums[0];
        for(int i=0;i<nums.length;i++){       //遍历所有元素，找到最大值和最小值
            min = nums[i]<min ? nums[i]:min;
            max = nums[i]>max ? nums[i]:max;
        }
        if(min==max)
            return 0;
        Bucket[] buckets=new Bucket[nums.length];
        int gap=(int)Math.ceil((max-min)/(nums.length-1));      //计算桶的容量
        for(int i=0;i<nums.length;i++){                         //遍历每个元素，计算该元素应该放置的桶的位置，将元素放入桶中，更新桶的最大值和最小值
            int index=getBucketIndex((int)min,nums[i],gap);
            putInBucket(buckets,nums[i],index);
        }
        int maxGap=buckets[0].max-buckets[0].min;
        int pre=buckets[0].max;
        for(int i=1;i<buckets.length;i++){                   //遍历所有桶，计算最大间距（桶间间距）
            if(buckets[i]!=null){
                if((buckets[i].min-pre)>maxGap){
                    maxGap=buckets[i].min-pre;
                }
                pre=buckets[i].max;
            }
        }
        return maxGap;
    }
    //内部类 桶
    class Bucket{
        int max=0;
        int min=0;
        boolean hasNum=false;
    }
    //根据元素的数值计算该元素应该在哪个桶中
    public int getBucketIndex(int min,int num,int gap){
        return (int)(num-min)/gap;
    }
    //将元素放入桶种，更新桶的最大值和最小值
    public void putInBucket(Bucket[] buckets,int num,int index){
        if(buckets[index]==null){
            buckets[index]=new Bucket();
            buckets[index].hasNum=true;
            buckets[index].max=num;
            buckets[index].min=num;
        }
        if(num>buckets[index].max)
            buckets[index].max=num;
        if(num<buckets[index].min)
            buckets[index].min=num;
    }
}
```
