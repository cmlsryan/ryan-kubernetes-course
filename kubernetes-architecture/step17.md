# Pod 異常排查：綜合實戰

最後用一個小實戰把第三個主題收起來。

今天不是要背很多狀態，

而是練習：

> **看到 Pod 不正常時，知道怎麼一步一步查。**

---

## 建立一個故障 Pod

`kubectl run final-broken --image=nginx:not-a-real-tag`{{exec}}

先不要直接猜原因。

第一步：

`kubectl get pods`{{exec}}

稍等後再看：

`sleep 8; kubectl get pods`{{exec}}

---

## 問題 1

如果看到：

```text
final-broken   0/1   ImagePullBackOff
```

下一步該做什麼？

### 答案

`kubectl describe pod final-broken`{{exec}}

---

## 問題 2

在 `describe` 裡最值得先找哪個區塊？

A. Events  
B. AGE  
C. Pod 名稱

### 答案：A

Events 往往直接記錄：

- Pulling
- Failed
- BackOff
- FailedScheduling

等關鍵事件。

---

## 問題 3

如果今天看到的是：

```text
CrashLoopBackOff
```

除了 `describe` 之外，

還應該想到哪個指令？

### 答案

```bash
kubectl logs POD名稱
```

如果 Container 已重啟過：

```bash
kubectl logs POD名稱 --previous
```

---

# 第三個主題總整理

## ImagePullBackOff

```text
Image 無法成功取得
```

先查：

```text
describe → Events
```

## CrashLoopBackOff

```text
Container 啟動後一直失敗並重新啟動
```

先查：

```text
logs
describe
```

## Pending

```text
Pod 尚未成功進入可執行狀態
```

先查：

```text
describe → Events
```

---

# 三個最重要的排錯指令

```bash
kubectl get pods
kubectl describe pod POD名稱
kubectl logs POD名稱
```

再補一個常用的：

```bash
kubectl get pods -o wide
```

---

# 最重要的思考方式

不要這樣：

```text
看到錯誤
↓
馬上刪掉
↓
希望它自己好
```

應該這樣：

```text
發現異常
↓
get 看現象
↓
describe / logs 蒐集資訊
↓
判斷原因
↓
再處理
```

---

## 清理

`kubectl delete pod final-broken --ignore-not-found`{{exec}}

最後確認：

`kubectl get pods`{{exec}}

---

# 第 ③ 個主題完成

你已經完成：

- Pod 基本狀態觀察
- `ImagePullBackOff`
- `CrashLoopBackOff`
- `Pending`
- `kubectl get pods`
- `kubectl get pods -o wide`
- `kubectl describe pod`
- `kubectl logs`
- `kubectl logs --previous`
- 用 Events 判斷 Kubernetes 發生的事情

下一個主題：

# ④ Deployment
