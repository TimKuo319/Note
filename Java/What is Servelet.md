---
tags:
  - Java
---

## What is Servelet

>**Servlet**（Server Applet），全稱 **Java Servlet**。是用[Java](https://zh.wikipedia.org/wiki/Java "Java")編寫的[伺服器](https://zh.wikipedia.org/wiki/%E6%9C%8D%E5%8A%A1%E5%99%A8 "伺服器")端[程式](https://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BA%8F "程式")。其主要功能在於互動式地瀏覽和修改資料，生成動態[Web](https://zh.wikipedia.org/wiki/Web "Web")內容。狹義的Servlet是指Java語言實現的一個[介面](https://zh.wikipedia.org/wiki/%E4%BB%8B%E9%9D%A2_(%E8%B3%87%E8%A8%8A%E7%A7%91%E6%8A%80) "介面 (資訊科技)")，廣義的Servlet是指任何實現了這個Servlet介面的[類別](https://zh.wikipedia.org/wiki/%E7%B1%BB_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6) "類別 (電腦科學)")，一般情況下，人們將Servlet理解為後者。 - 維基百科

Servelet 是 Java 的一種規範，主要用來提供 Java Web 應用程式的一個統一的標準，封裝了底層的網路操作。算是最一開始 Java Web 開發最早的基礎。

但隨著需求越來越大，Servelet 複雜的語法使得專案的複雜度增加。因此以 Servelet 為基礎再延伸出了 JSP、Spring Framework 等等技術，方便開發者去維護及擴展更大型的專案。雖然 Servelet 現在已經很少被直接使用，但是像 JSP、Spring Framework 等的底層也都還是 Servelet。

## Servelet vs Servelet Container

Servelet 就像前面提到的是一種規範，但現在文章裡面看到的 Servelet 指的通常都是*實現了 Servelet 介面的類別* ，像是 JSP、Spring Framework 等等，都是透過 Servelet 撰寫的伺服器端程式。

而 Servelet 就需要跑在 Servelet Container 上，常見的 Servelet container 像是 Apache Tomcat(Spring Boot 預設就是使用 Tomcat )、Jetty 等。在 [Servlet和Servlet容器区别_ervlet容器和servlet有什么区别-CSDN博客](https://blog.csdn.net/weixin_56587974/article/details/133711491) 就有不錯的比喻，可以讓人比較明瞭的理解兩者的差異。

## Reference

- [1.Servlet到底是什么 - 随遇而安== - 博客园](https://www.cnblogs.com/55zjc/p/16536646.html)

- [Servlet和Servlet容器区别_ervlet容器和servlet有什么区别-CSDN博客](https://blog.csdn.net/weixin_56587974/article/details/133711491)

- [Servlet 和 Servlet容器 - rickiyang - 博客园](https://www.cnblogs.com/rickiyang/p/12764615.html)