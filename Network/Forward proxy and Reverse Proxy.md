#Network

## Forward Proxy

`Forward Proxy`，中文名稱叫做正向代理，代表許多 client 會透過一個 proxy，被導流到 server 中，對於 server 來說，他只會知道是 forward proxy 將訊息傳遞給他的，並不會知道實際的位置在哪裡。

常見用途：
- 匿名使用者
	- 某些國家可能會被限制觀看某些內容 -> 透過 forward proxy 來繞過限制
- 企業防火牆
	- 企業針對內部 client 在使用網路時，可以去過濾 client request 或 response，來達到控管流量及保護內部網路的效果。
- Caching
	- 透過 proxy 做 cache，這樣當公司或學校等組織人員訪問相同的靜態資源時，就能得到更快地回應。-> 現在通常由 CDN 取代。
## Reverse Proxy

`Reverse Proxy` 就是一個相對於 `Forward Proxy` 的概念。前面提到，forward proxy 會讓 server 不知道實際的 client 來自哪裡。而 reverse proxy 就是 ==讓 client 不知道實際的 server 在那裡。== 對於 client 來說，他們都是打向 proxy server，但並不知道他們被實際導向的 server 位置。

常見用途：
- Load balancing
	- proxy 後方有多台 server，透過 proxy 來進行流量分發
- 隱藏 server 真實位置

## Summary

Forward Proxy 跟 Reverse Proxy 的區別主要在於用途，本質上都是由一台 proxy server 來作為中間層，使 client 或 server 達到想要的目的。

## Reference

[系統設計 - 正向代理跟反向代理 · jyt0532's Blog](https://www.jyt0532.com/2019/11/18/proxy-reverse-proxy/)