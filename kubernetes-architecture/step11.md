# Node 基本維運：Drain 是什麼？

上一頁我們學到：

```text
cordon
↓
不要再把新的 Pod 排到這台 Node

但原本的 Pod
↓
繼續執行
```

那如果今天真的要維修這台 Node 呢？

例如：

- 要重新啟動主機
- 要更換硬體
- 要做系統維護
- 希望原本的工作也先離開

這時候只做 `cordon` 就不夠了。

我們會使用：

```bash
kubectl drain
```

---

# Cordon vs Drain

先看最重要的差別：

```text
cordon
↓
停止新的 Pod 被排進來
↓
原本的 Pod 留著

drain
↓
停止新的 Pod 被排進來
↓
並嘗試把可以移走的 Pod 驅逐出去
```

所以：

> **cordon = 先不要接新工作**

> **drain = 我要準備維修，原本的工作也要先處理掉**

---

# 先準備一個實驗 Pod

上一頁我們已經把 `controlplane` 設成：

```text
SchedulingDisabled
```

為了示範 Drain 的效果，

我們先暫時恢復排程。

直接點：

`kubectl uncordon controlplane`{{exec}}

接著建立一個 nginx Pod：

`kubectl run nginx --image=nginx`{{exec}}

再查看：

`kubectl get pods -o wide`{{exec}}

你可能會看到：

```text
NAME    READY   STATUS    NODE
nginx   1/1     Running   controlplane
```

也就是：

```text
Node: controlplane
└── Pod: nginx
```

現在 nginx 正在這台 Node 上執行。

---

# 如果現在要維修 Node 呢？

我們希望：

1. 不要再接新的 Pod
2. 原本可以移走的 Pod 也先離開

這就是 Drain。

---

# 現在實際 Drain

⚠️ 這裡是 Killercoda 練習環境。

直接點：

`kubectl drain controlplane --ignore-daemonsets --delete-emptydir-data --force`{{exec}}

執行過程中，你可能會看到類似：

```text
node/controlplane cordoned
evicting pod default/nginx
pod/nginx evicted
node/controlplane drained
```

---

# 看懂剛才發生什麼

## 1. cordoned

```text
node/controlplane cordoned
```

Drain 會先讓 Node：

**停止接受一般新的 Pod 排程。**

---

## 2. evicting

```text
evicting pod default/nginx
```

Kubernetes 開始把可以驅逐的 Pod 從 Node 移除。

---

## 3. drained

```text
node/controlplane drained
```

代表 Drain 的處理完成。

---

# 現在檢查 Node

直接點：

`kubectl get nodes`{{exec}}

你應該會看到類似：

```text
NAME           STATUS                     ROLES
controlplane   Ready,SchedulingDisabled   control-plane
```

注意：

```text
Ready
```

還在。

代表：

**Node 本身並沒有壞掉。**

但同時出現：

```text
SchedulingDisabled
```

代表：

**目前不要再把一般新的 Pod 排到這台 Node。**

---

# 再看看 nginx Pod

直接點：

`kubectl get pods`{{exec}}

原本的 nginx Pod 應該已經不在了。

可能看到：

```text
No resources found
```

所以剛才：

```text
Node
└── Pod: nginx
```

Drain 之後：

```text
Node（SchedulingDisabled）

nginx Pod
→ 被驅逐
```

---

# 那 Kubernetes 系統自己的 Pod 呢？

直接點：

`kubectl get pods -A`{{exec}}

你會發現：

**不是所有 Kubernetes 系統 Pod 都消失了。**

因為有些系統元件屬於：

- DaemonSet 管理的 Pod
- Static Pod
- Kubernetes 必須保留的系統元件

我們剛才使用：

```text
--ignore-daemonsets
```

就是告訴 Drain：

**不要因為 DaemonSet Pod 而停止 Drain。**

---

# 剛才的參數是什麼？

我們使用：

```bash
kubectl drain controlplane --ignore-daemonsets --delete-emptydir-data --force
```

這一章不用全部背。

先知道：

### `drain`

準備維護 Node，並嘗試驅逐可以移走的 Pod。

### `--ignore-daemonsets`

忽略 DaemonSet 管理的 Pod。

### `--delete-emptydir-data`

允許處理使用 `emptyDir` 暫存資料的 Pod。

### `--force`

允許處理某些沒有 Controller 管理的 Pod。

我們剛剛用 `kubectl run` 建立的 nginx Pod，

就是直接建立的 Pod，

所以這個練習會使用 `--force`。

---

# ⚠️ 實務上不要看到 --force 就亂加

在真正的正式環境，

Drain 前一定要先確認：

- Node 上有哪些 Pod
- Pod 是否有 Controller 管理
- 是否有重要的本機資料
- 移走 Pod 會不會影響服務

所以今天使用這些參數，

主要是因為：

**這是一個可以安全重來的 Killercoda 練習環境。**

---

# 快速比較

| 指令 | 新 Pod | 原本的 Pod | 適合情境 |
|---|---|---|---|
| `cordon` | 不再排入 | 繼續執行 | 先暫停接新工作 |
| `drain` | 不再排入 | 嘗試驅逐 | 準備維護 Node |

最簡單記：

```text
cordon
= 封鎖新工作

drain
= 封鎖新工作 + 清理原本工作
```

---

# 小問題

假設今天工程師說：

> 「這台 Node 明天要重新開機，
> 我希望新的 Pod 不要進來，
> 原本可以移走的 Pod 也先離開。」

比較適合：

### A

`kubectl cordon`

### B

`kubectl drain`

### C

`kubectl get nodes`

## 答案：B — Drain

因為這次不是只有停止新排程，

而是真的要準備維護 Node。

---

# 但是現在有一個問題

我們 Drain 完之後，

Node 還是：

```text
Ready,SchedulingDisabled
```

也就是：

**維護完了，它還是不會重新接新的 Pod。**

那維護完成後，

要怎麼讓這台 Node 恢復正常排程？

下一頁會學最後一個 Node 維運指令：

```bash
kubectl uncordon
```

把：

```text
cordon → drain → uncordon
```

整個維護流程一次串起來。
