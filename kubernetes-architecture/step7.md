# 實作：看看 Cluster 裡有哪些 Node

前面我們已經知道：

- Cluster = 整套 Kubernetes 管理環境
- Node = 提供 CPU、記憶體等資源的工作機器

現在不要只背定義。

直接看看：

**我們現在這個 Cluster 裡，到底有哪些 Node？**

---

# 先預測

如果管理員想查看：

- Cluster 裡有幾台 Node
- Node 的名稱
- Node 目前的狀態

應該使用哪一個指令？

### A

```bash
kubectl get nodes
```

### B

```bash
kubectl run nginx --image=nginx
```

### C

```bash
kubectl delete pod nginx
```

先自己選 A、B 或 C。

提示：

- `get` = 查看
- `run` = 建立
- `delete` = 刪除

我們現在要做的是：

**查看 Node。**

---

# 答案：A

```bash
kubectl get nodes
```

指令可以拆成：

```text
kubectl + get + nodes

kubectl = 和 Kubernetes 溝通
get     = 查看
nodes   = Node 資源
```

---

# 現在直接操作

直接點下面的指令：

`kubectl get nodes`{{exec}}

執行後，你可能會看到類似：

```text
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   ...   ...
```

---

# 看懂 Output

這一章先看三個重點就夠了。

## NAME

Node 的名稱。

例如：

```text
controlplane
```

代表這台 Node 名稱叫做 `controlplane`。

---

## STATUS

代表 Node 目前的狀態。

如果看到：

```text
Ready
```

代表 Kubernetes 目前判斷這台 Node 處於可用狀態。

先記：

```text
Ready = Node 目前可用
```

---

## ROLES

你可能會看到：

```text
control-plane
```

代表這台 Node 同時負責 Kubernetes 的管理功能。

目前先知道即可，

後面學 Node 維運時會再更詳細說明。

---

# 小提醒

`Ready` 不代表：

**任何 Pod 都一定可以被排到這台 Node。**

真正排程時，

Kubernetes 還會考慮：

- Pod 需要多少資源
- Node 還剩多少資源
- 排程條件
- 其他限制

這裡先記住最基本的：

> **Ready = Kubernetes 目前判斷這台 Node 可以正常參與 Cluster。**

---

# 剛剛我們真的看到 Node 了

前面只是概念：

```text
Cluster
└── Node
```

現在透過：

`kubectl get nodes`{{exec}}

我們真的從 Kubernetes 裡把 Node 查了出來。

所以接下來會繼續按照：

**學概念 → 馬上操作 → 看真實 Output**

來學。

---

# 下一步：建立第一個 Pod

現在我們已經：

- 知道 Cluster 是什麼
- 知道 Node 是什麼
- 真的查看過 Node

接下來要在 Kubernetes 裡建立第一個工作：

**nginx Pod**

下一頁會直接操作：

```bash
kubectl run nginx --image=nginx
```

然後再用：

```bash
kubectl get pods
```

確認 Pod 到底有沒有成功執行。
