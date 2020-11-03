---
title: K 站中转内最便宜的航班[中等]
date: 2020-10-20 10:25:54
tags: 
 - 算法
 - 动态规划
categories: 
 - [算法,动态规划]
cover: https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201026161113.png
---

# 最长上升子序列

![image-20201026161056811](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201026161113.png)

## 分析

> 困扰了好久，对这种没有顺序性选择的动态规划还是不熟悉，dp 不知道如何定义合适
>
> 重点是 k 站内中转，那么很容易想到 k - 1 站中转最划算的，那么数组还是起点和终点两个属性，是不是应该定义成三维数组？先考虑 k 站如何通过 k - 1 站推理出来。
>
> 假设 k - 1 次中转可以到达站已经求出来了，那么可以 k 站中转就一定满足以 k - 1次中转可到达的站为起点，到达新的终点，新的终点就可以在 k 站中转到达。那么 当 k = 0 时，就是求直达终点的站点，这个时候就要想到，dp数组定义的时候可以定死 k 站内到达某个站，起点在 k = 0 的时候初始化，那么 k + 1 不管终点是什么，起点永远是 k = 0 时初始化的位置，最后只要取 k 内站能不能到达某个站就可以了。初始化 dp 十分重要，考虑到 k 站时不能到达终点，那么就要在开始 初始化所有 k ，终点值为费用，
>
> 步骤：
>
> 1. 定义状态
>
>    dp [i] [k] ：  k 站内中转到达 i 位置的最小费用
> 
> 2. 初始化
>
>    dp[target] [k] = 费用：k 站内终点初始化为 直达到达终点的费用
>
> 3. 状态转移
>
>   如果遍历到数组 i，起点为 start,终点为 end ， k - 1 可以到达 start 位置 ，那么 k 就等于 min( dp[end] [k] = dp [start] [k-1] + 新费用 , dp [end] [k]) 
>
> 4. 选出结果
>   dp[target] [k]

## 代码

### java

```java
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {

        int [][] dp = new int[k+1][n];
        //因为需要求最小值，无用位置初始化为最大值
        for(int i = 0 ; i < dp.length ; i++){
                Arrays.fill(dp[i],Integer.MAX_VALUE);
        }
        for(int[] arr:flights){
            if(arr[0] == src){
                for (int[] ints : dp) {
                    //初始化直达位置在k站的费用为直达费用
                    //主要是防止 k 站不能到达，但是可以直达
                    ints[arr[1]] = arr[2];
                }
            }
        }
        for(int i = 1 ; i <= k ; i++){
            for(int j = 0 ;j < flights.length ; j++){
                int srcc = flights[j][0];
                int target = flights[j][1];
                int price = flights[j][2];
                //如果新的起点，在k - 1 的位置有终点，也就是不为初始化的值，那么新的终点就可以在 k 站内到达
                if(dp[i-1][srcc] != Integer.MAX_VALUE){
                    dp[i][target] = Math.min(dp[i-1][srcc] + price , dp[i][target]);
                }
            }

        }
        return dp[k][dst] == Integer.MAX_VALUE ? -1 : dp[k][dst];

    }
}
```

### go

```go
func lengthOfLIS(nums []int) int {
	if len(nums)==0 || len(nums) == 1 {
		return len(nums);
	}
	dp := make([]int,len(nums))
	for i := 0; i < len(dp); i++ {
		dp[i] = 1;
	}
	res := 1;
	max := func (a,b int)int {
		if a > b {
			return a
		}
		return b
	}
	for i := 1; i < len(dp); i++ {
		for j := 0; j < i; j++ {
			if nums[i] > nums[j] {
				dp[i] = max(dp[j] + 1,dp[i])
			}
		}
		res = max(dp[i],res);

	}
	return res;

}
```