# 總整理與清除環境

## 把正式名稱和物流比喻配對

| Kubernetes | 物流比喻 | 真正技術意義 |
|---|---|---|
| Cluster | 整套物流系統 | Kubernetes 管理的整體系統範圍 |
| Node | 倉庫或分店 | 加入 Cluster 的實體機或虛擬機 |
| Pod | 一組被安排的工作 | Kubernetes 部署與排程的基本單位 |
| Container | 工作人員或機器 | 真正執行程式的隔離環境 |
| Scheduler | 總部調度員 | 替尚未指派 Node 的 Pod 選擇合適的 Node |

---

## 清除今天建立的 Pod

執行：

`kubectl delete pod nginx`{{exec}}

再確認：

`kubectl get pods`{{exec}}

你可能會看到：

```text
No resources found in default namespace.
```

因為今天直接建立的是一個沒有 Deployment 等控制器管理的 Pod。

刪除後，不會有控制器替它建立新的副本。

---

## 最後用自己的話回答

1. Kubernetes 為什麼不只是 Docker 的替代品？
2. Cluster、Node、Pod、Container 的包含關係是什麼？
3. Scheduler 負責什麼？
4. `kubectl get pods -o wide` 比一般輸出多幫我們確認什麼？
