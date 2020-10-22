---
title: 不同的子序列 II[中等]
date: 2020-10-21 10:25:54
tags: 
 - 算法
 - 动态规划
categories: 
 - [算法,动态规划]
cover: https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201021180824.png
---

# 不同的子序列 II

![image-20201021180822786](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201021180824.png)

## 分析

> 假设来到中间位置 i ，以 i 结尾的不同子序列要根据两个条件判断 ，字符是不是第一次出现和 i -1 结尾的不同子序列的个数，如果 i 位置字符是第一次出现，那么 i 位置的不同子序列个数为 i -1 的个数 * 2 + 1，之前的每个不同子序列都能和 i 位置字符组成新的子序列，加 1 是加上新的字符，如果不是第一次出现，那么 i 位置的不同子序列格式为 i - 1 的个数 * 2 减去前一次出现字符前一个位置的不同子序列的个数。
>
> 难点 ：
>
> 1. dp [ i ] 保存的是 原数组 i  + 1 结尾的个数，减去上次出现位置的前一个在原数组的位置 x 就是减去 dp[x] 
> 2. 减去上次出现的位置的前一个位置结果可能为负数，需要加上 mod。
>
> 步骤：
>
> 1. 定义状态
>
>    dp [i] ：以原数组 i - 1位置结尾的不同子序列的个数
>
> 2. 初始化
>
>    dp[0] = 0
>
> 3. 状态转移
>
>    如果字符第一次出现 dp[i] = 2 * dp[ i -1 ] + 1
>
>    不是第一次出现 dp [ i ] = (2 * dp[ i -1] - dp[ 上次出现位置前一个位置 ])
>
> 4. 选出结果
>
>    dp[dp.length -1]

## 代码

### java

```java
class Solution {
    public int distinctSubseqII1(String strs) {
        if(strs.length() < 2){
            return strs.length();
        }
        int mod = (int)1e9+7;
        int [] dp = new int [strs.length()+1];
        int[] x = new int[26];
        Arrays.fill(x,-1);
        for(int i = 0 ; i < strs.length() ; i++){
           int ch =  strs.charAt(i) - 'a';
            dp[i + 1] = dp[i] * 2 + 1;
            if(x[ch] >= 0){
                dp[i + 1] -= (dp[x[ch]] + 1);
            }
            if (dp[i + 1] < 0) dp[i + 1]+=mod ;
            dp[i + 1] %= mod;
            x[ch] = i;
        }
        return dp[dp.length -1];
    }
    
     // 优化的方法，思想都是一样的，巧妙的利用了数组覆盖的方式减去重复的序列，注意越界问题
     // 每一次都要求和，效率低
     public int distinctSubseqII(String strs) {
        int mod = (int)1e9+7;
        long[] dp = new long[26];
        for(char ch : strs.toCharArray()){
            dp[ch - 'a'] = (Arrays.stream(dp).sum() +1) % mod;
        }
        return (int)(Arrays.stream(dp).sum() % mod);
    }
}
```

### go

```go
func distinctSubseqII(S string) int {
	mod :=int(math.Pow(10,9)+7)
	len := len(S)
	dp := make([]int, len+1)
	idx := make([]int, 26)
	for i := 0; i < 26; i++ {
		idx[i] = -1
	}
	for i := 0; i < len; i++ {
		x := S[i] - 'a'
		dp[i+1] = dp[i]*2 + 1
		if idx[x] >= 0 {
			dp[i+1] -= (dp[idx[x]] + 1)
		}
		if dp[i+1] < 0 {
			dp[i+1] += mod
		}
		dp[i+1] %= mod
		idx[x] = i
	}
	return dp[len]
}
```