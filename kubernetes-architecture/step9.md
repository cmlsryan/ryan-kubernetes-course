# 實作：Pod 被 Scheduler 安排到哪一台 Node？

上一頁我們已經建立了 nginx Pod：

```bash
kubectl run nginx --image=nginx
```

也用：

```bash
kubectl get pods
```

確認它目前正在執行。

但還有一個問題：

**這個 Pod 到底被安排到哪一台 Node？**

---

# 先回想一下

建立 Pod 時，

我們並沒有自己指定：

> 「請把 nginx 放到某一台 Node。」

那 Kubernetes 到底是怎麼決定位置的？

前面學過：

**Scheduler 會替 Pod 選擇適合的 Node。**

現在我們直接驗證。

---

# 查看更多 Pod 資訊

直接點：

`kubectl get pods -o wide`{{exec}}

你可能會看到類似：

```text
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE
nginx   1/1     Running   0          ...   10.x.x.x     controlplane
```

---

# 這次重點看 NODE

找到：

```text
NODE
```

這一欄代表：

**這個 Pod 目前被安排在哪一台 Node 上。**

例如：

```text
controlplane
```

表示 nginx Pod 現在執行在 `controlplane` 這台 Node。

---

# 把 Scheduler 串回來

剛剛整個流程其實是：

```text
建立 nginx Pod
        ↓
Pod 還沒有執行位置
        ↓
Scheduler 幫 Pod 選 Node
        ↓
Pod 被安排到 Node
        ↓
Container 開始執行 nginx
```

所以：

```text
Scheduler
   ↓
幫 Pod 選 Node
```

---

# 現在完整結構就看得到了

假設剛才 `NODE` 欄顯示：

```text
controlplane
```

那現在可以畫成：

```text
Cluster
└── Node: controlplane
    └── Pod: nginx
        └── Container: nginx
```

---

# 快速確認

### Q1

整套 Kubernetes 管理環境叫什麼？

**Cluster**

### Q2

真正提供 CPU、記憶體與執行資源的是什麼？

**Node**

### Q3

Scheduler 安排的是什麼？

**Pod**

### Q4

真正執行 nginx 程式的是什麼？

**Container**

---

# 再看一次目前 Pod

直接點：

`kubectl get pods -o wide`{{exec}}

這次請自己找出：

- Pod 名稱
- Pod 狀態
- Pod 所在的 Node

---

# 最後清除練習資源

這個 nginx Pod 是我們剛剛為了練習建立的。

現在把它刪掉：

`kubectl delete pod nginx`{{exec}}

如果成功，會看到類似：

```text
pod "nginx" deleted
```

---

# 確認 Pod 已經刪除

再點：

`kubectl get pods`{{exec}}

如果沒有其他 Pod，

你可能會看到：

```text
No resources found
```

代表剛才的 nginx Pod 已經被清除。

---

# 今天這一段你真的做過了

不是只背名詞。

你已經實際操作：

```text
kubectl get nodes
        ↓
查看 Node

kubectl run nginx --image=nginx
        ↓
建立 Pod

kubectl get pods
        ↓
查看 Pod 狀態

kubectl get pods -o wide
        ↓
查看 Pod 被安排到哪台 Node

kubectl delete pod nginx
        ↓
刪除練習 Pod
```

---

# 最重要的一張圖

```text
Cluster
└── Node
    └── Pod
        └── Container

Scheduler
   ↓
幫 Pod 選 Node
```

---

# 下一步

到這裡，Kubernetes 基礎架構就已經從：

**生活比喻 → 正式名稱 → 真實指令 → 真實 Output**

全部串起來了。

下一個主題就可以開始進入：

**Node 基本維運實作。**
