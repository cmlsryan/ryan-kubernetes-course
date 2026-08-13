# 先預測：要怎麼查看 Cluster 裡的 Node？

## 情境

現在我們已經知道：

**Node 是加入 Cluster、提供運算資源的工作機器。**

管理員想確認：

- 目前有幾台 Node？
- Node 的名稱是什麼？
- Node 的健康狀態如何？

應該使用哪一個指令？

---

## A

```bash
kubectl get nodes
```

---

## B

```bash
kubectl run nginx --image=nginx
```

---

## C

```bash
kubectl delete pod nginx
```

---

## 提示

先觀察三個動作：

- `get`
- `run`
- `delete`

現在的需求是：

**查看、建立，還是刪除？**

先選 A、B，還是 C。

下一頁公布答案，並直接在右邊終端機操作。
