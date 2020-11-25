---
title: 全排列 II[中等]
date: 2020-11-25 10:25:54
tags: 
 - 算法
 - 回溯
categories: 
 - [算法,回溯]
cover: https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201125151426.png
---

# 全排列 II

![image-20201125151425696](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201125151426.png)

## 分析

> 全排列的去重，想到了排序判断和前一个位置的值是否相等，可惜还是差一点思路，想到了同父节点下不能有两个相同的数字，怎么表示同个父节点，其实就是两个数字都可以选择。因为排序的原因总是先选第一个不重复的数字，当可以选择第二个重复的数字时，如果第一个重复的数字可以选择，那么就忽略第二个重复的数字。否则可以选择第二个数字，因为重复的数字不是同一个父节点

### java

```java
class Solution {
    private boolean[] vi;
    private List<List<Integer>> res = new ArrayList();

    public List<List<Integer>> permuteUnique(int[] nums) {
        ArrayList<Integer> arr = new ArrayList();
        vi = new boolean[nums.length];
        Arrays.sort(nums);
        process(nums, arr);
        return res;
    }
    public void process(int[] nums, List arr) {
        if (arr.size() == nums.length) {
            res.add(new ArrayList(arr));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (vi[i] || (i > 0 && nums[i] == nums[i - 1] && !vi[i - 1])) {
                continue;
            }
            arr.add(nums[i]);
            vi[i] = true;
            process(nums, arr);
            vi[i] = false;
            arr.remove(arr.size()-1);
        }
        return;
    }
}
```
