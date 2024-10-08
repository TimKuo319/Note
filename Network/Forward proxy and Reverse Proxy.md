#Network

## Forward Proxy

`Forward Proxy`，中文名稱叫做正向代理，代表許多 client 會透過一個 proxy，被導流到 server 中，對於 server 來說，他只會知道是 forward proxy 將訊息傳遞給他的，並不會知道實際的位置在哪裡。

## Reverse Proxy

`Reverse Proxy` 就是一個相對於 `Forward Proxy` 的概念。前面提到，forward proxy 會讓 server 不知道實際的 client 來自哪裡。而 reverse proxy 就是 ==讓 client 不知道實際的 server 在那裡。== 對於 client 來說，他們都是打向 proxy server，但並不知道他們被實際導向的 server 位置。

## Pros and Cons

- [ ] proxy 的優缺點

## Reference

[系統設計 - 正向代理跟反向代理 · jyt0532's Blog](https://www.jyt0532.com/2019/11/18/proxy-reverse-proxy/)