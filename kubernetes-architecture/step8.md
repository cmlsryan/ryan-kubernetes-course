# 建立第一個 Pod：nginx

剛剛我們已經用：

`kubectl get nodes`{{exec}}

看到了 Cluster 裡真正存在的 Node。

現在不要只看環境。

我們直接建立第一個工作：

**nginx Pod**

---

# 先預測

如果要建立一個使用 nginx Image 的 Pod，

應該使用哪一個指令？

### A

```bash
kubectl get pods
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

---

# 答案：B

```bash
kubectl run nginx --image=nginx
```

這個指令會建立一個名稱叫做 `nginx` 的 Pod，

並使用 nginx Image。

---

# 現在直接操作

直接點下面的指令：

`kubectl run nginx --image=nginx`{{exec}}

成功後通常會看到：

```text
pod/nginx created
```

---

## 指令拆解

```text
kubectl run nginx --image=nginx
```

可以拆成：

```text
kubectl
↓
和 Kubernetes 溝通

run
↓
建立並執行 Pod

nginx
↓
Pod 的名稱

--image=nginx
↓
指定使用 nginx Image
```

所以整句可以先理解成：

> 建立一個名稱叫 nginx 的 Pod，使用 nginx Image 執行程式。

---

# created 代表已經正常了嗎？

剛剛看到：

```text
pod/nginx created
```

只代表：

**Kubernetes 已經接受建立這個 Pod 的要求。**

但我們還不知道它現在是不是真的已經正常執行。

所以接著要查看 Pod 狀態。

---

# 查看 Pod

直接點：

`kubectl get pods`{{exec}}

你可能會看到：

```text
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          ...
```

---

# 看懂輸出

這裡先看三個地方。

## NAME

```text
nginx
```

就是 Pod 的名稱。

---

## READY

如果看到：

```text
1/1
```

代表這個 Pod 目前需要 Ready 的 Container 有 1 個，

而現在已經有 1 個 Ready。

先簡單記成：

```text
1/1 = Container 已經準備完成
```

---

## STATUS

如果看到：

```text
Running
```

代表 Pod 已經進入執行狀態。

先記：

```text
Running = Pod 正在執行
```

但要注意：

**Running 不代表應用程式所有功能一定完全正常。**

這一章先知道 Pod 已經進入執行狀態就可以。

---

# 把剛才的概念串起來

我們剛剛實際完成：

```text
建立 nginx Pod
        ↓
Kubernetes 接收建立要求
        ↓
Pod 被安排到某台 Node
        ↓
Pod 裡的 Container 執行 nginx
```

所以現在的結構可以想成：

```text
Cluster
└── Node
    └── Pod: nginx
        └── Container: nginx
```

---

# 小問題

我們現在知道：

- nginx Pod 已經建立
- STATUS 是 Running
- Container 已經 Ready

但還有一件事情不知道：

**這個 Pod 到底被安排到哪一台 Node？**

我們建立 Pod 的時候，

並沒有自己指定它要去哪台 Node。

那到底是誰幫它決定位置？

下一頁直接用：

```bash
kubectl get pods -o wide
```

找出 nginx Pod 真正被安排到哪一台 Node，

再把前面學過的 **Scheduler** 實際驗證一次。
