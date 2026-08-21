# Node 維護完成：讓它重新接工作

上一頁我們執行了：

```bash
kubectl drain controlplane
```

現在 Node 應該還處於：

```text
Ready,SchedulingDisabled
```

也就是：

```text
Node 本身正常
但是
暫時不接受新的 Pod
```

---

# 先確認現在的狀態

直接點：

`kubectl get nodes`{{exec}}

觀察：

```text
NAME           STATUS
controlplane   Ready,SchedulingDisabled
```

這裡非常重要。

## Ready

代表：

> Node 本身目前是正常的。

## SchedulingDisabled

代表：

> Kubernetes 暫時不會把新的 Pod 排進這台 Node。

所以：

```text
Ready ≠ 一定可以接新 Pod
```

還要看它有沒有被禁止排程。

---

# 假設維修已經完成

工程師現在說：

> 主機維修好了，可以恢復服務了。

那我們就要讓這台 Node：

**重新接受新的 Pod。**

使用：

```bash
kubectl uncordon controlplane
```

直接點：

`kubectl uncordon controlplane`{{exec}}

你應該會看到：

```text
node/controlplane uncordoned
```

---

# 再看一次 Node

直接點：

`kubectl get nodes`{{exec}}

現在應該會重新看到：

```text
NAME           STATUS
controlplane   Ready
```

原本的：

```text
SchedulingDisabled
```

消失了。

代表：

> 這台 Node 已經重新允許 Kubernetes 把新的 Pod 排進來。

---

# 但我們不要只相信文字

我們真的建立一個新的 Pod 測試。

直接點：

`kubectl run nginx-after-maintenance --image=nginx`{{exec}}

接著查看：

`kubectl get pods -o wide`{{exec}}

你可能會看到：

```text
NAME                      READY   STATUS    NODE
nginx-after-maintenance   1/1     Running   controlplane
```

重點看：

```text
NODE
controlplane
```

這代表新的 Pod 又成功被安排到這台 Node。

---

# 所以剛才完整發生了什麼？

我們把 Node 維修流程重新跑一次：

```text
正常 Node
   ↓
cordon
   ↓
停止新的 Pod 排入
   ↓
drain
   ↓
停止新排程 + 驅逐可以移走的 Pod
   ↓
進行主機維護
   ↓
uncordon
   ↓
重新允許新的 Pod 排入
```

這就是非常常見的 Node 維運思路。

---

# 三個指令不要混在一起

| 指令 | 作用 |
|---|---|
| `kubectl cordon NODE` | 不再接受新的 Pod |
| `kubectl drain NODE` | 準備維護，並驅逐可以移走的 Pod |
| `kubectl uncordon NODE` | 重新接受新的 Pod |

可以簡單記成：

```text
cordon
= 暫停接新工作

drain
= 我要維修，原本工作也先處理

uncordon
= 維修完成，重新接工作
```

---

# 情境挑戰

現在不要只背指令。

## 情境 A

工程師說：

> 「這台機器晚點可能要維修，
> 先不要讓新的 Pod 進來，
> 但現在正在跑的 Pod 先不要動。」

你會使用：

A. `cordon`

B. `drain`

C. `uncordon`

### 答案

**A — cordon**

因為只需要停止新的排程，

原本的 Pod 繼續執行。

---

## 情境 B

工程師說：

> 「我要立刻維修這台 Node，
> 可以移走的 Pod 也先處理掉。」

你會使用：

A. `cordon`

B. `drain`

C. `uncordon`

### 答案

**B — drain**

因為現在是真的準備維護 Node。

---

## 情境 C

工程師說：

> 「維修完成了，
> 這台 Node 可以重新開始接 Pod。」

你會使用：

A. `cordon`

B. `drain`

C. `uncordon`

### 答案

**C — uncordon**

它會重新允許新的 Pod 排進這台 Node。

---

# 最後清理練習 Pod

直接點：

`kubectl delete pod nginx-after-maintenance`{{exec}}

再確認：

`kubectl get pods`{{exec}}

---

# Node 基本維運完成

這一章目前至少要會看懂：

```text
kubectl get nodes
```

查看 Node。

```text
kubectl cordon NODE
```

停止新的 Pod 排程。

```text
kubectl drain NODE
```

維修前處理 Node 上的工作。

```text
kubectl uncordon NODE
```

維修完成後恢復排程。

最重要的不是把指令硬背起來，

而是知道：

```text
查看狀態
→ 暫停排程
→ 排空工作
→ 維修
→ 恢復排程
→ 實際驗證
```

---

# 下一章

到這裡：

## ② Node 基本維運實作

完成。

接下來進入：

# ③ Pod 與 Pod 異常排查

接下來會開始遇到比較接近真正維運現場的問題：

- Pod 為什麼不是 Running？
- Pod 一直重啟怎麼辦？
- 怎麼看 Pod 詳細資訊？
- 怎麼查看錯誤訊息？
- 怎麼看 Container Log？
- `Pending`
- `CrashLoopBackOff`
- `ImagePullBackOff`

下一章開始，

我們不只會「建立 Pod」。

而是要學會：

> **Pod 壞掉時，到底要從哪裡開始查。**
