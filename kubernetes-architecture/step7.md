# 答案與解釋：Cluster 是整套系統範圍

## 正確答案：C

**Cluster**

Cluster 是整套 Kubernetes 系統的管理範圍。

它可以包含：

- 管理 Kubernetes 的元件
- 多台 Node
- Node 上執行的 Pod

---

## 選項解析

### A × Container

Container 是實際執行應用程式的地方。

它只是整套系統裡面很小的一部分，不是整體範圍。

### B × Node

Node 是加入 Cluster、提供 CPU、記憶體等資源的工作機器。

它也只是 Cluster 裡面的一部分。

### C ✓ Cluster

Cluster 代表整套 Kubernetes 管理環境的範圍。

---

## 生活對應

可以先把它想成：

**整套連鎖公司 = Cluster**

但要記得，這只是幫助理解的比喻。

Cluster 並不是真的公司，而是整套 Kubernetes 管理環境。

---

## 結構先看一次

```text
Cluster
├── Node A
│   └── Pod
└── Node B
    └── Pod
```

---

## 下一題

既然 Cluster 裡面有很多工作機器，

那麼：

**真正提供 CPU、記憶體與運算資源的機器，在 Kubernetes 裡叫什麼？**
