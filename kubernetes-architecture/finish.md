# 第一章完成：Kubernetes 架構

你已經完成：

- 理解 Kubernetes 為什麼存在
- 看見 Cluster 裡的 Node
- 建立第一個 Pod
- 確認 Pod 裡的 Container 正常就緒
- 看見 Scheduler 把 Pod 安排到哪個 Node
- 清除練習資源

## 最重要的一張圖

```text
Cluster
└── Node
    └── Pod
        └── Container
```

更完整地說：

> Kubernetes 編排 Cluster 裡的容器化工作負載；Scheduler 替尚未分配位置的 Pod 選擇合適的 Node，而真正執行程式的是 Pod 裡的 Container。

下一章才會進入 Node 基本維運。
