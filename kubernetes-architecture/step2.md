# 看見 Cluster 裡的 Node

## 情境

物流公司的主管想先知道：

> 目前系統裡有幾座可用的倉庫？

對應到 Kubernetes，就是先查看 Cluster 裡有哪些 Node。

點擊下面的指令執行：

`kubectl get nodes`{{exec}}

---

## 指令拆解

```text
kubectl    和 Kubernetes 溝通的命令列工具
get        取得、列出資訊
nodes      查看 Cluster 裡的 Node
```

合起來就是：

> 請 Kubernetes 列出目前 Cluster 裡的 Node。

---

## 你現在應該觀察什麼？

請先只看兩欄：

- `NAME`：Node 的名稱
- `STATUS`：Node 的健康狀態

你可能會看到：

```text
NAME           STATUS   ROLES
controlplane   Ready    control-plane
```

`Ready` 表示 Kubernetes 目前判斷這台 Node 的健康狀態正常。

> 注意：`Ready` 和「是否允許排入新的 Pod」不是完全同一件事；不可排程的 Node 仍可能是 Ready。Node 維運章節才會深入這個差異。

---

## 請回答

1. 畫面中有幾台 Node？
2. Node 的名稱是什麼？
3. 哪一個欄位顯示 Node 的健康狀態？
