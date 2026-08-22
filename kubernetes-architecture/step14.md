# Pod 異常排查 ②：CrashLoopBackOff

`ImagePullBackOff` 是「Image 拿不到」。

但還有另一種情況：

```text
Container 成功啟動
↓
程式立刻失敗
↓
Kubernetes 再重啟
↓
又失敗
↓
一直重複
```

這就是常見的：

# CrashLoopBackOff

---

## 故意做出一個一直失敗的 Pod

`kubectl run crash-demo --image=busybox --restart=Always -- /bin/sh -c 'echo "程式啟動"; sleep 2; echo "程式發生錯誤"; exit 1'`{{exec}}

先看狀態：

`kubectl get pods`{{exec}}

稍等一下再看：

`sleep 12; kubectl get pods`{{exec}}

你可能會看到：

```text
NAME         READY   STATUS             RESTARTS
crash-demo   0/1     CrashLoopBackOff   2
```

---

## 先看哪一欄？

這次除了 `STATUS`，還要注意：

```text
RESTARTS
```

如果數字一直增加，代表 Container 不斷重新啟動。

---

## 問題

現在 Pod 的 Image 明明下載成功了，

但程式啟動後一直失敗。

哪個工具最能直接看到「程式自己輸出的內容」？

A. `kubectl get pods`  
B. `kubectl logs crash-demo`  
C. `kubectl get nodes`

### 答案：B

---

## 看 Container 日誌

`kubectl logs crash-demo`{{exec}}

可能會看到：

```text
程式啟動
程式發生錯誤
```

如果 Container 已經重新啟動過，

更有用的是查看「上一個已經掛掉的 Container」：

`kubectl logs crash-demo --previous`{{exec}}

---

## get、describe、logs 各看什麼？

```text
get
↓
現在是什麼狀態？

describe
↓
Kubernetes 這邊發生了什麼？

logs
↓
程式自己說了什麼？
```

---

## 再看一次詳細資訊

`kubectl describe pod crash-demo`{{exec}}

觀察：

- State
- Last State
- Restart Count
- Events

---

## 小測驗

看到：

```text
READY      0/1
STATUS     CrashLoopBackOff
RESTARTS   6
```

最合理的第一組排錯方式是：

A. `kubectl get pods` → `kubectl logs POD`  
B. `kubectl delete node`  
C. 直接重裝 Kubernetes

### 答案：A

---

## 清理

`kubectl delete pod crash-demo`{{exec}}

下一頁我們再看第三種常見情況：

# Pending
