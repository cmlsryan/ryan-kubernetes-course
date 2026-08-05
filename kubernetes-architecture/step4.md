# 確認 Pod 是否正常

## 為什麼還要查看？

剛才 Kubernetes 回覆：

```text
pod/nginx created
```

這代表建立要求成功，但我們還要確認 Pod 是否真的開始運作。

執行：

`kubectl get pods`{{exec}}

---

## 只觀察四欄

```text
NAME    READY   STATUS    AGE
nginx   1/1     Running   ...
```

- `NAME=nginx`：找到剛才建立的 Pod。
- `READY=1/1`：Pod 中需要就緒的 1 個 Container，目前有 1 個已就緒。
- `STATUS=Running`：Pod 已被安排到 Node，Container 已建立，而且至少一個 Container 正在執行、啟動或重新啟動。
- `AGE`：Pod 建立至今的時間。

對這個簡單範例來說，看到：

```text
READY   1/1
STATUS  Running
```

就能判斷 nginx Container 已經正常啟動並就緒。

---

## 請回答

1. 我們建立的是 Pod 還是 Node？
2. 真正執行 nginx 程式的是 Pod 本身，還是 Pod 裡的 Container？
3. 哪兩個欄位一起證明這個簡單範例已經正常就緒？
