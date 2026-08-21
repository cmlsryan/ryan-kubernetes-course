# Pod 異常排查：第一個故障 Pod

前面我們已經知道：

```text
Cluster
└── Node
    └── Pod
        └── Container
```

也已經成功建立過正常的 nginx Pod。

但實際維運時，

真正麻煩的通常不是：

> 「怎麼建立 Pod？」

而是：

> **「為什麼這個 Pod 沒有正常跑起來？」**

---

# 先看一個正常 Pod

先建立正常的 nginx：

`kubectl run normal-nginx --image=nginx`{{exec}}

查看：

`kubectl get pods`{{exec}}

正常情況下，稍等一下可能會看到：

```text
NAME           READY   STATUS    RESTARTS   AGE
normal-nginx   1/1     Running   0          ...
```

先記住：

```text
Running
```

代表 Container 已經成功啟動並正在執行。

---

# 現在故意做壞一個 Pod

這次我們故意指定一個不存在的 Image：

`kubectl run broken-nginx --image=nginx:this-tag-does-not-exist`{{exec}}

Pod 還是會被建立。

但問題來了。

查看：

`kubectl get pods`{{exec}}

稍等幾秒後再執行一次：

`kubectl get pods`{{exec}}

你可能會看到：

```text
NAME           READY   STATUS             RESTARTS   AGE
normal-nginx   1/1     Running            0          ...
broken-nginx   0/1     ImagePullBackOff   0          ...
```

也可能先看到：

```text
ErrImagePull
```

之後才變成：

```text
ImagePullBackOff
```

---

# 先不要急著看答案

現在只看這個畫面：

```text
broken-nginx   0/1   ImagePullBackOff
```

你覺得最可能是哪一種問題？

### A

Node 壞掉了。

### B

Kubernetes 找不到或無法下載指定的 Container Image。

### C

程式成功啟動，但執行到一半當掉。

先自己選。

---

# 答案：B

## ImagePullBackOff

代表 Kubernetes：

> **嘗試取得 Container Image，但是失敗了。**

這次我們故意指定：

```text
nginx:this-tag-does-not-exist
```

這個 Image Tag 不存在，

所以 Kubernetes 沒辦法把 Image 下載下來。

---

# 但維運不能只靠猜

看到：

```text
ImagePullBackOff
```

我們已經大概知道方向。

但真正排查時，

還要找：

> **更詳細的錯誤原因。**

這時候可以使用：

```bash
kubectl describe pod POD名稱
```

直接查看：

`kubectl describe pod broken-nginx`{{exec}}

---

# 往下面找 Events

在輸出的下方，

你可能會看到類似：

```text
Events:
  Type     Reason     Message
  ----     ------     -------
  Normal   Pulling    Pulling image ...
  Warning  Failed     Failed to pull image ...
  Warning  BackOff    Back-off pulling image ...
```

現在其實已經可以把事情串起來：

```text
建立 Pod
   ↓
Scheduler 安排到 Node
   ↓
Node 嘗試取得 Image
   ↓
Image 不存在
   ↓
下載失敗
   ↓
ImagePullBackOff
```

---

# 第一個排錯工具：get

我們剛才第一個使用：

```bash
kubectl get pods
```

它很適合回答：

> **「現在有哪些 Pod？它們大概處於什麼狀態？」**

直接點：

`kubectl get pods`{{exec}}

重點先觀察：

```text
READY
STATUS
RESTARTS
```

---

# 第二個排錯工具：describe

接著使用：

```bash
kubectl describe pod broken-nginx
```

它比較適合回答：

> **「為什麼這個 Pod 變成這個狀態？」**

尤其可以查看：

- Pod 被安排到哪個 Node
- Container 狀態
- Image
- Events
- Kubernetes 發生過哪些錯誤

---

# get 和 describe 不一樣

可以先這樣記：

| 指令 | 主要用途 |
|---|---|
| `kubectl get pods` | 先看誰有問題 |
| `kubectl describe pod POD` | 再查它為什麼有問題 |

所以最基本的排錯流程是：

```text
發現服務有問題
      ↓
kubectl get pods
      ↓
找到異常 Pod
      ↓
kubectl describe pod POD
      ↓
找 Events 與錯誤訊息
```

---

# 小測驗

現在看到：

```text
myapp   0/1   ImagePullBackOff
```

第一步比較合理的是：

### A

直接把整個 Cluster 重開。

### B

先使用 `kubectl describe pod myapp` 查看原因。

### C

直接刪掉 Node。

## 答案

**B**

因為現在已經知道：

```text
有一個 Pod 出問題
```

下一步應該先蒐集資訊，

而不是直接亂改系統。

---

# 一個很重要的觀念

維運不是：

```text
看到錯誤
→ 馬上亂修
```

而是：

```text
看到現象
→ 蒐集資訊
→ 找到原因
→ 再處理
```

所以之後看到 Pod 異常，

先養成一個習慣：

```text
get
↓
describe
↓
再決定下一步
```

---

# 清理這次的 Pod

正常 Pod：

`kubectl delete pod normal-nginx`{{exec}}

故障 Pod：

`kubectl delete pod broken-nginx`{{exec}}

最後確認：

`kubectl get pods`{{exec}}

---

# 今天第一個 Pod 故障

你現在已經真的看過：

```text
ImagePullBackOff
```

它通常表示：

> Kubernetes 無法成功取得 Pod 所需要的 Container Image。

而且你已經學會第一組排錯流程：

```text
kubectl get pods
        ↓
kubectl describe pod POD
        ↓
查看 Events
```

---

# 下一步

但不是所有 Pod 都會卡在 Image。

還有另一種很常見的情況：

```text
Container 成功啟動
↓
程式馬上掛掉
↓
Kubernetes 又重新啟動
↓
又掛掉
↓
一直重複
```

這時候你會看到一個非常經典的狀態：

# CrashLoopBackOff

而且下一個問題會出現新的排錯工具：

```bash
kubectl logs
```
