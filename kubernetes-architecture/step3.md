# 建立第一個 Pod

## 先預測輸出

現在要讓 Kubernetes 執行 nginx。

若成功送出建立要求，最可能看到哪一句？

**A.** `pod/nginx created`  
**B.** `node/nginx created`  
**C.** `container/nginx created`

先回答，再執行：

`kubectl run nginx --image=nginx`{{exec}}

---

## 指令拆解

```text
kubectl          和 Kubernetes 溝通
run              在這個用法中，建立一個 Pod
nginx            Pod 的名稱
--image=nginx    Pod 內的 Container 使用 nginx Image
```

實際關係是：

```text
Pod：nginx
└── Container：使用 nginx Image 執行 nginx
```

你應該看到：

```text
pod/nginx created
```

這表示 **Pod 物件的建立要求已成功**。

> 但 `created` 還不能單獨證明 Container 已經正常運作；下一頁要再查看狀態。
