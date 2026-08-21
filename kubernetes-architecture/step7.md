# 先實際看 Cluster 裡有哪些 Node

前面我們已經知道：

- Cluster 是整套 Kubernetes 管理環境
- Node 是提供 CPU、記憶體與執行資源的工作機器
- Pod 會被安排到某台 Node 上執行

現在不要只記概念。

直接來看看：

**這個 Cluster 裡目前有哪些 Node？**

---

## 問題

如果管理員想確認：

- 目前有幾台 Node
- Node 的名稱是什麼
- Node 現在是否正常

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

---

## 先想一下

先看三個動作：

- `get` = 查看
- `run` = 建立
- `delete` = 刪除

我們現在的需求是：

**查看 Node。**

先自己選 A、B 或 C。

---

# 答案：A

```bash
kubectl get nodes
```

這個指令可以查看目前 Cluster 裡有哪些 Node。

---

## 為什麼？

### A ✓ `kubectl get nodes`

`get` 代表查看。

`nodes` 代表要查看的資源是 Node。

所以：

```text
kubectl + get + nodes
Kubernetes 工具 + 查看 + Node
```

---

### B × `kubectl run nginx --image=nginx`

這個指令是用來：

**建立一個 nginx Pod。**

不是查看 Node。

等等我們會真的操作它。

---

### C × `kubectl delete pod nginx`

這個指令是用來：

**刪除名稱叫 nginx 的 Pod。**

也不是查看 Node。

---

# 現在實際操作

請在右邊終端機輸入：

```bash
kubectl get nodes
```

執行後，你可能會看到類似：

```text
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   ...   ...
```

先觀察三個地方。

---

## NAME

代表 Node 的名稱。

例如：

```text
controlplane
```

就是這台 Node 的名字。

---

## STATUS

代表 Node 目前的狀態。

如果看到：

```text
Ready
```

代表 Kubernetes 目前判斷這台 Node 是正常可用的。

目前先簡單記：

```text
Ready = Node 目前可用
```

---

## 小提醒

`Ready` 不代表：

**任何 Pod 都一定可以被排進這台 Node。**

因為真正排程時還會考慮：

- Pod 需要多少 CPU
- Pod 需要多少記憶體
- Node 還剩多少資源
- 其他排程條件

這些後面再學。

現在知道：

**Ready 代表 Node 本身目前處於可用狀態。**

就夠了。

---

# 剛剛的概念現在真的看到了

前面我們學的是：

```text
Cluster
└── Node
```

現在透過：

```bash
kubectl get nodes
```

我們真的看到 Cluster 裡面的 Node。

所以不要只把 Kubernetes 當成名詞背。

之後會盡量按照：

**概念 → 指令 → 實際結果**

來學。

---

# 下一步：真的建立一個 Pod

現在我們已經：

- 知道 Cluster 是什麼
- 知道 Node 是什麼
- 用指令看到了 Node

接下來要建立第一個真正的工作：

**nginx Pod**

下一頁會操作：

```bash
kubectl run nginx --image=nginx
```

建立之後，再使用：

```bash
kubectl get pods
```

確認 Pod 有沒有成功建立。
