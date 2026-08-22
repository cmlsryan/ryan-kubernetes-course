# Pod 排錯工具：get、describe、logs

到目前為止，真正需要先記住的工具只有三個：

```text
kubectl get
kubectl describe
kubectl logs
```

重點不是背指令，

而是知道：

> **什麼時候該用哪一個。**

---

# ① get：先找誰有問題

`kubectl get pods`{{exec}}

適合先看：

- Pod 名稱
- READY
- STATUS
- RESTARTS
- AGE

如果還想看 Pod 被安排到哪個 Node：

`kubectl get pods -o wide`{{exec}}

---

# ② describe：看 Kubernetes 發生了什麼

格式：

```bash
kubectl describe pod POD名稱
```

適合查看：

- Image
- Node
- Container State
- Restart Count
- Events
- Scheduler / Pull Image 等錯誤

---

# ③ logs：看程式自己說了什麼

格式：

```bash
kubectl logs POD名稱
```

如果 Container 已經重啟過：

```bash
kubectl logs POD名稱 --previous
```

它適合查看：

> 應用程式本身輸出的錯誤訊息。

---

# 情境題

## 情境 A

```text
STATUS = ImagePullBackOff
```

你最想先看：

A. `describe`  
B. `logs`

### 答案：A

---

## 情境 B

```text
STATUS   = CrashLoopBackOff
RESTARTS = 8
```

你最想先看：

A. `logs`  
B. `get nodes`

### 答案：A

---

## 情境 C

```text
STATUS = Pending
```

你最想先看：

A. `describe`  
B. `logs`

### 答案：A

---

# 一個實用排錯順序

遇到不知道原因的 Pod：

```text
1. kubectl get pods
        ↓
2. kubectl get pods -o wide
        ↓
3. kubectl describe pod POD
        ↓
4. kubectl logs POD
        ↓
5. 根據資訊再決定怎麼修
```

注意：

這不是說每次五個都一定要跑。

而是：

> **先用快速資訊縮小範圍，再深入。**

---

# 小挑戰

下面三個 Pod，請直接說第一個最適合用的工具：

```text
A. web-1   ImagePullBackOff
B. api-1   CrashLoopBackOff
C. db-1    Pending
```

答案：

```text
A → describe
B → logs（也可以搭配 describe）
C → describe
```

下一頁做一次完整的小型故障排查。
