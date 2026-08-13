# 答案與解釋：Kubernetes 安排的是 Pod

## 正確答案：B

**Pod**

Pod 是 Kubernetes 部署與排程的基本單位。

Scheduler 會替 Pod 選擇適合的 Node。

真正執行應用程式的，

則是 Pod 裡面的 Container。

---

## 選項解析

### A × Container

Container 的確是真正執行程式的地方。

但 Kubernetes 排程時，

主要是把 **Pod** 安排到 Node。

### B ✓ Pod

Pod 會被建立、部署，

然後由 Scheduler 安排到某台 Node。

### C × Cluster

Cluster 是整套 Kubernetes 系統的範圍，

不是被排程到某台機器上的工作單位。

---

## 完整結構

```text
Cluster
└── Node
    └── Pod
        └── Container
```

---

## 生活對應

可以先這樣記：

- 一組需要一起被安排到某間分店的工作 → **Pod**
- 真正執行工作的店員或設備 → **Container**

所以：

**Kubernetes 安排 Pod，Container 負責執行程式。**

---

## 精確提醒

一個 Pod 裡面可以有一個或多個 Container。

實務上很多 Pod 只有一個主要 Container，

但 Pod 並不等於 Container。

---

## 下一題

概念差不多了。

接下來我們第一次把概念接到真正的指令。

如果你想看：

**目前 Cluster 裡有哪些 Node？**

你覺得該用哪個指令？
