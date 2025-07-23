---
tags:
  - K8s
---
## What is `K8s`, why we need it

K8s，全名為 Kubernetes，最早是由 google 內部開發，現交由 CNCF 維護。K8s 是用來管理多容器的技術，用來確保服務的穩定及高可用性。

但在 docker 同樣有 docker swarm 的狀況下，為何還需要使用 K8s 呢？

### Docker swarm  vs K8s

### 1. **部署規模與擴展性**

- **Docker Swarm**：支援多主機，但彈性與功能有限
    
- **Kubernetes**：設計為大規模部署的解決方案，具備資源調度、自動擴展等進階功能
    
###  2. **彈性與容錯能力**

- **Swarm** 支援基本的高可用（例如服務掛掉會自動重啟）
    
- **K8s** 支援更完整的策略：Pod anti-affinity、liveness probe、readiness probe、自動重啟/重新排程
    
###  3. **網路與流量管理**

- Swarm 提供基本 overlay network 與 service discovery
    
- K8s 提供更細緻的流量控制能力（例如：Ingress、Service Mesh）

###  4. **生態系統與工具支援**

- Kubernetes 擁有完整的工具鏈支援：如 Helm（套件管理）、Prometheus（監控）、Istio（Service Mesh）、Argo CD（GitOps）等
    
- Swarm 社群小，功能持續性發展趨緩（Docker 官方自己也轉向支援 K8s）


簡單來說，docker swarm 適用於**架構規模較小的情況下**，而且使用上的彈性較 K8s 更低。而 K8s 則是可以**處理大規模的容器管理，並且有較爲細緻的設定調整**。

## Reference

- [從異世界歸來的第二天 - Kubernetes 是什麼？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10287154)
- ChatGPT