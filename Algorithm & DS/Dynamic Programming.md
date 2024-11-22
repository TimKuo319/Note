---
tags:
  - is/evergreen/substantiated
---

DP 本質上的概念其實也是 divide and conquer，都是透過將問題變成小問題，透過這些小問題的答案去組合成最終問題的解。

但 DP 跟遞迴的差異在於，DP 的做法會透過額外去分配記憶體空間來去記憶先前小問題計算過後的結果，進而達到減少計算次數的效果，是一種用空間換時間的方式。

DP 主要有兩種以下類型的方式
- Top - Down
	- 將大問題先分解成小問題 -> 在解決小問題的過程中將小問題的答案記起來 -> 組合小問題的答案來解決大問題
	- Top - Down 其實就是優化版的遞迴，透過額外的儲存空間去儲存小問題的結果來減少重複計算的次數。
```java
public class FibonacciTopDownArray {
    private int[] memo;

    public FibonacciTopDownArray(int n) {
        memo = new int[n + 1]; // 初始化一個大小為 n+1 的陣列
    }

    public int fib(int n) {
        // 基礎情況
        if (n <= 2) return 1;

        // 檢查是否已經計算過
        if (memo[n] != 0) {
            return memo[n];
        }

        // 遞迴計算並做記憶化
        memo[n] = fib(n - 1) + fib(n - 2);
        return memo[n];
    }

    public static void main(String[] args) {
        FibonacciTopDownArray fibonacci = new FibonacciTopDownArray(10);
        System.out.println(fibonacci.fib(10)); 
    }
}

```

- Button - Up
	- 直接小從小問題開始解決，最後透過小問題的結果來解決大問題
	- 通常會用表格去紀錄結果，相對 top-down 的是在遇到子問題的時候才會紀錄，bottom-up 是將==所有==可能的子問題的解都儲存起來。

```java
package org.example;  
public class FibonacciBottomUp {  
  
    public int fib(int n) {  
        int[] dp = new int[n + 1];  
        dp[1] = 1;  
        dp[2] = 1;  
  
        for (int i = 3; i <= n; i++) {  
            dp[i] = dp[i - 1] + dp[i - 2];  
        }  
  
        return dp[n];  
    }  
  
    public static void main(String[] args) {  
        FibonacciBottomUp fibonacci = new FibonacciBottomUp();  
        System.out.println(fibonacci.fib(10)); // 輸出: 55  
    }  
}

```

```java
package org.example;  
public class FibonacciBottomUp {  
  
    public int fib(int n) {  
        if(n <= 2){  
            return 1;  
        }  
  
        int last = 1;  
        int secondLast = 1;  
        int current = 0;  
  
        for(int i = 3; i <= n; i++){  
            current = last + secondLast;  
            secondLast = last;  
            last = current;  
        }  
  
        return current;  
  
    }  
  
    public static void main(String[] args) {  
        FibonacciBottomUp fibonacci = new FibonacciBottomUp();  
        System.out.println(fibonacci.fib(10)); // 輸出: 55  
    }  
}
```


## LeetCode 53

- 暴力法
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0];
        for(int i = 0; i < nums.length; i++){
            int sum = 0;
            for(int j = i; j < nums.length; j++){
                sum = sum + nums[j];
                if(sum > max) {
                    max = sum;
                }
            }
        }
        return max;
    }
}
```

- DP
```java

```


## Reference

- [Dynamic programming 深入淺出 以Maximum Subarray 為例 | by 可愛小松鼠 Cute Squirrel | Medium](https://medium.com/@cutesciuridae/dynamic-programming-%E6%B7%B1%E5%85%A5%E6%B7%BA%E5%87%BA-%E4%BB%A5maximum-subarray-%E7%82%BA%E4%BE%8B-2016aab967c0)
- [Maximum Subarray - Leetcode 圖解演算法 | 圈圈工程師](https://chanchandev.com/dsa/dsa-problem-maximum-subarray/4106250428/)
- [演算法筆記系列 — Dynamic programming 動態規劃 | by Sean Chou | Recording everything | Medium](https://medium.com/%E6%8A%80%E8%A1%93%E7%AD%86%E8%A8%98/%E6%BC%94%E7%AE%97%E6%B3%95%E7%AD%86%E8%A8%98%E7%B3%BB%E5%88%97-dynamic-programming-%E5%8B%95%E6%85%8B%E8%A6%8F%E5%8A%83-de980ca4a2d3)
- [演算法學習筆記：動態規劃（Dynamic Programming） - 拉爾夫的技術隨筆 - Medium](https://medium.com/@ralph-tech/%E6%BC%94%E7%AE%97%E6%B3%95%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-%E5%8B%95%E6%85%8B%E8%A6%8F%E5%8A%83-dynamic-programming-db9453359559)
- [Top-down (Memoization) vs Bottom-up (Tabulation) approach in DP](https://www.enjoyalgorithms.com/blog/top-down-memoization-vs-bottom-up-tabulation)
- [從零開始進入動態規劃的世界（一）- 基礎中的基礎. 如果你在學習資料結構與演算法的路上，總是覺得動態規劃很難，聽到這四個字就害怕，碰… | by Arthur Lin | Medium](https://arthur-lin.medium.com/%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B%E9%80%B2%E5%85%A5%E5%8B%95%E6%85%8B%E8%A6%8F%E5%8A%83%E7%9A%84%E4%B8%96%E7%95%8C-%E4%B8%80-%E5%9F%BA%E7%A4%8E%E4%B8%AD%E7%9A%84%E5%9F%BA%E7%A4%8E-eee0695b3795)
