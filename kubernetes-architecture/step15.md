# Pod 異常排查 ③：Pending

前兩種問題：

```text
ImagePullBackOff
→ Image 取得失敗

CrashLoopBackOff
→ Container 啟動後一直失敗
```

現在看另一種：

# Pending

Pending 常表示：

> **Pod 已經建立，但目前還沒有成功進入可執行狀態。**

其中一個常見原因就是：

> **Scheduler 找不到符合條件的 Node。**

---

## 故意建立一個排不上去的 Pod

我們要求 Pod 只能去一台具有不存在標籤的 Node：

`kubectl run pending-demo --image=nginx --overrides='{"spec":{"nodeSelector":{"demo":"no-such-node"}}}'`{{exec}}

查看：

`kubectl get pods`{{exec}}

你應該會看到類似：

```text
pending-demo   0/1   Pending
```

---

## 問題

Pod 一直是 `Pending`，

下一步最值得先查什麼？

A. `kubectl describe pod pending-demo`  
B. `kubectl logs pending-demo`  
C. `kubectl delete node`

### 答案：A

因為 Container 根本還沒有正常跑起來，

這時通常先看 Kubernetes 的排程資訊與 Events。

---

## 查看原因

`kubectl describe pod pending-demo`{{exec}}

往下找到：

```text
Events:
```

你可能會看到類似：

```text
0/1 nodes are available
node(s) didn't match Pod's node affinity/selector
```

意思就是：

> Scheduler 找不到符合條件的 Node，所以 Pod 只能停在 Pending。

---

## Pending 不只一種原因

常見可能包括：

- CPU / 記憶體資源不足
- NodeSelector 不符合
- Taint / Toleration 不符合
- 儲存資源尚未準備好
- 其他排程限制

所以看到 `Pending` 不要直接背原因。

應該先：

```text
kubectl describe pod ...
↓
看 Events
↓
確認真正原因
```

---

## 快速比較

| 狀態 | 第一個排查方向 |
|---|---|
| `ImagePullBackOff` | `describe` 看 Image / Events |
| `CrashLoopBackOff` | `logs` + `describe` |
| `Pending` | `describe` 看 Scheduler / Events |

---

## 清理

`kubectl delete pod pending-demo`{{exec}}

下一頁把三種錯誤放在一起，

練習「看到現象，選對工具」。
