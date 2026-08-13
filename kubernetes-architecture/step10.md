# 正式名稱題三：Kubernetes 排程的基本單位是什麼？

## 問題

Kubernetes 建立、部署，

並且由 Scheduler 安排到某台 Node 上的**基本單位**是什麼？

---

## A

**Container**

真正執行應用程式的地方。

---

## B

**Pod**

Kubernetes 建立、部署與排程的基本單位。

---

## C

**Cluster**

整套 Kubernetes 系統的管理範圍。

---

## 想一想

流程可能長這樣：

```text
1. 建立一個 ?
2. 部署這個 ?
3. Scheduler 把這個 ? 安排到某台 Node
```

這三個「?」應該是同一個東西。

---

## 提示

真正執行程式的部分，

和 Kubernetes 負責「整組安排位置」的單位，

可能不是同一個東西。

先選 A、B，還是 C。

下一頁才公布答案。
