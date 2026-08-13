# 答案與解釋：Node 是工作機器

## 正確答案：A

**Node**

Node 是加入 Kubernetes Cluster 的工作機器。

它可以是：

- 實體機
- 虛擬機
- 雲端主機

Node 會提供：

- CPU
- 記憶體
- 執行資源

讓 Pod 可以在上面運作。

---

## 選項解析

### A ✓ Node

真正提供運算資源的機器。

### B × Pod

Pod 是被安排到 Node 上執行的部署與排程基本單位。

它本身不是一台機器。

### C × Scheduler

Scheduler 會選擇適合的 Node，

但 Scheduler 本身不是 Pod 真正執行的位置。

---

## 結構再看一次

```text
Cluster
└── Node
    ├── Pod
    └── Pod
```

---

## 生活對應

可以把 Node 想成：

**一間真正有場地、設備、電力與人力資源的分店。**

生活比喻：

**一間分店 = Node**

正式概念：

**Node = 加入 Cluster、提供資源的工作機器**

---

## 下一題

現在我們知道：

- Cluster 是整體範圍
- Node 是提供資源的工作機器

那 Kubernetes 真正「建立、部署、排程」的基本單位，

到底是 **Container、Pod，還是 Cluster**？
