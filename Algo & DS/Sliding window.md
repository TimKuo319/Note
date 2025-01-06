
sliding window 算是 two pointer 的一種類型。主要有兩種不同的 sliding window

1. Fixed Size Sliding Window

2. Variable Size Sliding Window

這兩者的核心本質上都是透過移動 window 的 left 跟 right 來透過 window 去計算我們想要的值。

解題的主要核心在於去關注 window 內的狀況來決定 left 跟 right 的變更。因為自己在剛寫題目的時後很常只關注到為了讓 left 和 right 移動，而忽略了清楚表達 window 內的公式導致思路混亂。

## Reference

- [Sliding Window Technique - GeeksforGeeks](https://www.geeksforgeeks.org/window-sliding-technique/)
