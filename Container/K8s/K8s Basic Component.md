---
tags:
  - K8s
---
## Master Node(Control Plane)

每一個 k8s cluster 都會具備一個 master node 負責用來管理容器之間的狀況。它包含了以下的 component

- API server
	- 是唯一操作 k8s 資源的方式，使用者不論是透過 CLI 或 UI，最終的指令都是傳遞到這裡，並由 api server 向以下其他的 component 溝通。

- Scheduler
	- 負責處理 pod 的調度情況，會監控 cluster 內所有的 worknode 並依照使用者的定義去為 pod 分配最佳的 workernode

- Controller Manager
	- Controller 負責處理不同的資源物件的狀態，內建有許多 controller，通常都在 Master node 中
	- 而 controller manager 是負責執行**多個 build-in controller**來確保 cluster 處在期望的狀態。

- etcd
	- 用來保存整個 cluster 的運行狀態。

## Worker Node

Worker Node 是真正運行 Pod 的主機，而 Pod 是 k8s 中最小的主機單位。可以是物理機也可以是虛擬機

- Pod
	- Pod 是 k8s 中最基礎的單元，每個 Pod 可以有一個或多個 container 組成。

- Kubelet 
	- 負責與 master node 通訊，當 master node 要執行某個操作的時候也會傳遞給 kubelet 對該 node 進行操作
	
- Kube-proxy
	- 負責處理 node 與 cluster 內部或外部的網路通訊
	
## Reference

- [Cluster Architecture | Kubernetes](https://kubernetes.io/docs/concepts/architecture/)

- [從異世界歸來的第三天 - Kubernetes 的組件 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10287576)

- [[Kubernetes] Cluster Architecture | 小信豬的原始部落](https://godleon.github.io/blog/Kubernetes/k8s-CoreConcept-Cluster-Architecture/)