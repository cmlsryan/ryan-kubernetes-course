# 答案與實際操作：查看 Cluster 裡的 Node

## 正確答案：A

```bash
kubectl get nodes
```

---

## 指令拆解

`kubectl`

和 Kubernetes 溝通的命令列工具。

`get`

取得、查看資訊。

`nodes`

這次要查看的資源種類。

所以：

```text
kubectl + get + nodes
Kubernetes工具 + 查看 + Node
```

---

## 現在實際執行

請在右邊終端機輸入：

```bash
kubectl get nodes
```

也可以直接點擊執行：

`kubectl get nodes`{{exec}}

---

## 執行後先觀察這幾欄

你可能會看到類似：

```text
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   ...   ...
```

先看三個重點：

### NAME

Node 的名稱。

### STATUS

Node 的狀態。

### Ready

代表 Kubernetes 目前判斷這台 Node 處於可用狀態。

---

## 選項解析

### A ✓ `kubectl get nodes`

列出目前 Cluster 裡的 Node。

### B × `kubectl run nginx --image=nginx`

這是建立一個 Pod，

不是查看 Node。

### C × `kubectl delete pod nginx`

這是刪除 Pod，

不是查看 Node。

---

## 精確提醒

`Ready` 主要反映 Node 的健康與可用狀態。

不要把它簡化成：

**「只要 Ready，所有情況下 Kubernetes 就一定會把新的 Pod 排進來。」**

排程時還會考慮資源需求與其他排程條件。

---

## 下一步

下一頁我們會把今天全部概念串起來：

1. 建立一個 nginx Pod
2. 查看 Pod 狀態
3. 查看 Pod 被安排到哪一個 Node
4. 最後刪除它

這樣就完成今天從概念到實作的完整流程。
