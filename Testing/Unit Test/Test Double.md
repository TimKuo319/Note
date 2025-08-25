---
tags:
  - Test
  - UnitTest
---

## What is `Test Double`, why we need it

單元測試的目的是為了確保最小單位的程式碼輸入輸出結果符合預期，所以我們的目的會注重在`測試目標(一個 function or method) 本身。

但我們在撰寫程式時，常常會需要跟其他元件互動(其他物件、DB、第三方 API)，若是在做單元測試的過程中真的去使用到這些外部物件，就讓我們的測試變因變多了，因為增加了測試的相依性。

而這也是這篇的主題 **Test Double**。

*Test Double* 直翻為測試替身，其實就是替要測試的函數使用到的外部元件做替身，來讓整個測試保持在驗證



## Reference

- [單元測試之 mock/stub/spy/fake ? 傻傻搞不清楚 | by CraftsmanHenry | Medium](https://medium.com/@henry-chou/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E4%B9%8B-mock-stub-spy-fake-%E5%82%BB%E5%82%BB%E6%90%9E%E4%B8%8D%E6%B8%85%E6%A5%9A-ba3dc4e86d86)

- [[Day 22] 談 test double 的五種類型 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10273603?sc=rss.iron)  
