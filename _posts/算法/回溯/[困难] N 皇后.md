---
title: N 皇后[困难]
date: 2020-11-25 10:25:54
tags: 
 - 算法
 - 回溯
categories: 
 - [算法,回溯]
cover: https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201125174139.png
---

# N 皇后

![image-20201125174137638](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201125174139.png)

## 分析

> 经典的回溯算法例题，思路不算困难，判断是否有效很容易写多余的代码

### java

```java
class Solution {
    private List<List<String>> res = new ArrayList();

    public List<List<String>> solveNQueens(int n) {
        String[][] arr = new String[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                arr[i][j] = ".";
            }
        }
        process(arr, 0);
        return res;
    }

    public void process(String[][] arr, int h) {
        if (h == arr.length) {
            saveRes(arr);
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            arr[h][i] = "Q";
            if (verify(arr, h, i)) {
                process(arr, h+1);
            }
            arr[h][i] = ".";
        }
    }

    public boolean verify(String[][] arr,int row ,int col) {
        for(int i = 0 ; i < row; i++){
            if(arr[i][col].equals("Q")){
                return false;
            }
        }
        for(int i = row-1 ,j = col -1; i>=0 && j >=0 ; i--,j--){
            if(arr[i][j].equals("Q")){
                return false;
            }
        }
        for(int i = row - 1, j = col + 1;i>=0&& j < arr.length; i--,j++){
            if(arr[i][j].equals("Q")){
                return false;
            }
        }
        return true;

    }

    public void saveRes(String[][] arr) {
        ArrayList list = new ArrayList();
        for (int i = 0; i < arr.length; i++) {
            StringBuilder str = new StringBuilder("");
            for (int j = 0; j < arr[0].length; j++) {
                str.append(arr[i][j]);
            }
            list.add(str.toString());
        }
        res.add(list);
    }
}
```
