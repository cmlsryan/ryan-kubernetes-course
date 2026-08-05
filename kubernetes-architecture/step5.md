# Pod 被安排到哪個 Node？

## 情境

貨物不會自己決定去哪座倉庫。

同樣地，新的 Pod 也不會自己選 Node。Kubernetes 中的 **Scheduler** 會替尚未被指派 Node 的 Pod，選擇一台符合條件的 Node。

要查看 nginx Pod 被安排到哪裡，執行：

`kubectl get pods -o wide`{{exec}}

---

## `-o wide` 是什麼？

```text
-o       output，指定輸出格式
wide     顯示比預設格式更多的欄位
```

今天最重要的新欄位是：

```text
NODE
```

你可能會看到：

```text
NAME    STATUS    NODE
nginx   Running   controlplane
```

這代表 nginx Pod 被安排到 `controlplane` 這台 Node。

---

## 為什麼一定是 controlplane？

這個練習環境只有一台可排程的 Node，因此 Scheduler 沒有其他 Node 可選。

在多 Node Cluster 裡，Scheduler 會根據 Pod 的資源需求、Node 可提供的容量與排程規則，選擇符合條件的 Node。

不要簡化成：

> Scheduler 一定選即時 CPU 使用率最低的 Node。

---

## 現在的完整畫面

```text
Cluster
└── Node：controlplane
    └── Pod：nginx
        └── Container：nginx
```

請用自己的話回答：

> nginx Pod 為什麼會出現在 controlplane？
