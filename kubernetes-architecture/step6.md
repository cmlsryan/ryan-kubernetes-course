# 把生活比喻換成 Kubernetes 真正名稱

前面我們一直用：

- 整套公司
- 分店
- 工作
- 調度員

來理解 Kubernetes。

現在不用再繞比喻了。

把剛剛的概念正式換成 Kubernetes 名稱：

| 前面的生活概念 | Kubernetes 正式名稱 |
|---|---|
| 整套管理環境 | Cluster |
| 提供 CPU、記憶體的工作機器 | Node |
| 被 Kubernetes 建立、部署、排程的工作單位 | Pod |
| Pod 裡真正執行程式的部分 | Container |
| 幫 Pod 選擇 Node 的角色 | Scheduler |

---

## 先快速確認 3 題

不用每一題都等到下一頁公布答案。

先自己想，再往下看。

### Q1

整套 Kubernetes 管理環境叫什麼？

A. Node  
B. Cluster  
C. Pod

### Q2

真正提供 CPU、記憶體等資源的工作機器叫什麼？

A. Node  
B. Pod  
C. Scheduler

### Q3

Kubernetes 建立、部署並安排到某台 Node 的基本單位是什麼？

A. Container  
B. Pod  
C. Cluster

---

# 答案

### Q1：B — Cluster

Cluster 是整套 Kubernetes 管理環境。

它裡面可以包含多台 Node，以及在 Node 上執行的 Pod。

### Q2：A — Node

Node 是加入 Cluster 的工作機器。

它可以是：

- 實體機
- 虛擬機
- 雲端主機

Node 提供 CPU、記憶體與執行環境。

### Q3：B — Pod

Kubernetes 排程的基本單位是 Pod。

Scheduler 會替 Pod 選擇適合的 Node。

真正執行應用程式的，則是 Pod 裡面的 Container。

---

## 最重要的結構

```text
Cluster
└── Node
    └── Pod
        └── Container
