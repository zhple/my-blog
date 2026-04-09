---
title: 算法培训
description: 写算法的一些总结
slug: 算法
date: 2026-04-07 20:54:00+0000
image: back1.jpg
categories:
    - Algorithm Category
tags:
    - Algorithm Tag
weight: 2       # You can add weight to some posts to override the default sorting (date descending)
---
## 力扣509题 
- 第一种解法 
    就纯递归
    这个方法会有大量的重复计算，但在理解上是最简单的，同时时间复杂度也是很高的
    ![Image 1](1-1.png)
    代码也是非常简单
    因为高中时有做过所以基本上想一下就写出来了
    ```c++
    class Solution {
    public:
        int fib(int n) {
            if (n <= 1){
                return n;
            }

            return fib(n -1 ) + fib(n - 2);
        }
    };
    ```
- 第二种解法
    动态规划 用数组来记忆
    用这种方法就能发现运行时间上已经快很多，因为避免了许多的重复计算
    ![Image 1](1-2.png)
    我在之前有做过简单的像跳格子这种动态规划题，所以这个方法写的也很快
    核心思路就是把已经处理过的节点存下，再次到达时重入就行
    从下向上推导
    ```c++
    class Solution {
    public:
        int fib(int n) {
            if (n <= 1) return n;
            vector<int> dp(n + 1);
            dp[0] = 0;
            dp[1] = 1;
            for (int i = 2; i <= n; ++i) {
                dp[i] = dp[i - 1] + dp[i - 2];
            }
            return dp[n];
        }
    };
    ```
- 第三种解法
    上面动规的一种空间层面的优化
    通过上面两个方法可以发现，其实没有必要把所有节点都存下来，每次计算变化的只有前两项，来俩变量就行
    如此依赖空间复杂度就到常数级了
    ![Image 1](1-3.png)
    思路不变，优化了空间，动态数组的扩容所占的空间和内存肯定比操作俩数字变量高
    ```c++
    class Solution {
    public:
        int fib(int n) {
            if (n <= 1) return n;
            int prev = 0, curr = 1;
            int res = 0;
            for (int i = 2; i <= n; ++i) {
                res = prev + curr; 
                prev = curr;       
                curr = res;        
            }
            return res;
        }
    };
    ```