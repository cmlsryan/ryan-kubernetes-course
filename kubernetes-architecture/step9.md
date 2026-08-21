# Node 基本維運：先停止接收新的 Pod

前面我們已經知道：

```text
Cluster
└── Node
    └── Pod
```

Node 是真正提供 CPU、記憶體與執行資源的工作機器。

現在開始進入第二個主題：

# Node 基本維運

假設今天有一台 Node 需要進行維護。

例如：

- 準備更新系統
- 準備檢查硬體
- 準備重新啟動
- 管理員暫時不希望新的 Pod 被排到這台 Node

但目前已經在上面執行的 Pod，

**暫時不需要立刻趕走。**

這時候該怎麼做？

---

# 先想一下

下面哪一種處理方式比較合理？

### A

直接把 Node 關機。

### B

讓原本的 Pod 繼續執行，

但暫時不要再安排新的 Pod 到這台 Node。

### C

把 Cluster 裡所有 Pod 全部刪掉。

---

# 答案：B

有些維護工作不需要立刻停止目前正在執行的 Pod。

我們只是希望：

> **從現在開始，不要再把新的 Pod 排到這台 Node。**

Kubernetes 裡負責這件事情的指令叫：

```bash
kubectl cordon
```

---

# Cordon 是什麼？

可以先記成：

```text
cordon
=
Node 暫停接收新的 Pod
```

但有一個非常重要的地方：

**原本已經在 Node 上執行的 Pod 不會因為 cordon 自動被刪除或趕走。**

所以：

```text
Cordon 前
Node
├── Pod A
└── Pod B
```

執行 cordon 後：

```text
Node（不再接新的 Pod）
├── Pod A  ← 繼續執行
└── Pod B  ← 繼續執行
```

---

# 先查看目前的 Node

直接點：

`kubectl get nodes`{{exec}}

找到目前 Node 的名稱。

在 Killercoda 裡通常會看到：

```text
NAME           STATUS   ROLES
controlplane   Ready    control-plane
```

---

# 現在實際 Cordon

直接點：

`kubectl cordon controlplane`{{exec}}

如果成功，通常會看到：

```text
node/controlplane cordoned
```

---

# 再查看一次 Node

直接點：

`kubectl get nodes`{{exec}}

這次注意：

```text
STATUS
```

你可能會看到：

```text
Ready,SchedulingDisabled
```

---

# SchedulingDisabled 是什麼？

它代表：

**這台 Node 本身仍然是 Ready，**

但是：

**Scheduler 不應該再把一般新的 Pod 排到這台 Node。**

所以：

```text
Ready
=
Node 本身目前可用

SchedulingDisabled
=
暫停接收新的 Pod
```

兩個可以同時存在：

```text
Ready,SchedulingDisabled
```

這不是矛盾。

---

# 很重要：Cordon 不等於關機

Cordon 之後：

- Node 沒有被關掉
- Kubernetes 沒有把 Node 刪掉
- 原本的 Pod 不會自動消失
- 只是停止一般新的 Pod 被排進來

所以不要把 cordon 理解成：

```text
停止 Node
```

比較精確的是：

```text
停止新的排程進入這台 Node
```

---

# 快速確認

假設某台 Node 上已經有：

```text
Pod A
Pod B
Pod C
```

現在執行：

```bash
kubectl cordon node01
```

最合理的結果是哪一個？

### A

Pod A、B、C 全部立刻被刪除。

### B

Pod A、B、C 繼續執行，

但新的 Pod 暫時不要再排到這台 Node。

### C

整台 Node 立刻關機。

## 答案：B

這就是 cordon 最重要的概念。

---

# 下一步

但是如果今天不是：

> 「先不要接新的工作」

而是：

> **「我要真的維修這台 Node，所以連原本的工作也希望先移走。」**

那只做 cordon 就不夠了。

下一步我們會學：

```bash
kubectl drain
```

比較：

```text
cordon
→ 不再接新的 Pod
→ 原本的 Pod 繼續跑

drain
→ 準備維護 Node
→ 嘗試把可驅逐的 Pod 移出 Node
```

下一頁直接實際比較：

**cordon 和 drain 到底差在哪裡。**
