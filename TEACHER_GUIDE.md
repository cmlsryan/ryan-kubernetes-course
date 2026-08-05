# 教師試教指南

## 建議時間：30–40 分鐘

### 0–8 分鐘：不碰終端機

使用 `intro.md` 與 `step1.md`。

教學流程：

1. 公司有多座倉庫，每座倉庫各自做事。
2. 一座倉庫壞掉，其他倉庫不會因為各自有 Docker 就自動統一協調。
3. 引導學生選擇「需要一套統一觀察與協調的系統」。
4. 再揭曉 Kubernetes。
5. 建立 Cluster → Node → Pod → Container 的畫面。

### 8–15 分鐘：查看 Node

執行：

`kubectl get nodes`

只問：

- 幾台 Node？
- 名稱？
- STATUS 是什麼？

精確說法：

- `Ready` 表示 Node 健康狀態正常。
- 不要說 Ready 一定代表可排程；cordon 後仍可能 Ready。

### 15–23 分鐘：建立 Pod

先讓學生預測輸出，再執行：

`kubectl run nginx --image=nginx`

精確說法：

- `pod/nginx created` 表示建立要求成功。
- 不代表 Container 已經確定 Running，所以要再查看。

### 23–28 分鐘：確認狀態

執行：

`kubectl get pods`

觀察：

- NAME
- READY
- STATUS
- AGE

對本範例，`READY=1/1` 與 `STATUS=Running` 一起確認 nginx 已正常就緒。

### 28–33 分鐘：Scheduler

執行：

`kubectl get pods -o wide`

觀察 `NODE=controlplane`。

精確說法：

- Scheduler 替尚未分配 Node 的 Pod 選擇符合條件的 Node。
- 本環境只有一台可排程 Node，因此結果是 controlplane。
- 不要說 Scheduler 一定選即時 CPU 最低的 Node。

### 33–38 分鐘：整理與刪除

執行：

`kubectl delete pod nginx`

再執行：

`kubectl get pods`

解釋：

- 這是直接建立的 Pod，沒有控制器維持副本數。
- 刪除後不會自動重建。

## 教授可能追問

### Kubernetes 建立的是 Pod 還是 Container？

Kubernetes 建立並排程的是 Pod；Pod 內會包含一個或多個 Container。今天建立的是 nginx Pod，裡面執行一個使用 nginx Image 的 Container。

### 為什麼需要 Pod？

Pod 提供 Container 在 Kubernetes 中共同使用的執行環境，例如網路命名空間、排程位置，以及可共享的 Volume。Kubernetes 把 Pod 當作部署與排程基本單位。

### Running 是否代表有人正在使用網站？

不是。Running 是 Pod phase，表示 Pod 已被安排到 Node、Container 已建立，而且至少一個 Container 正在執行、啟動或重新啟動。本範例再配合 READY 1/1，可確認 Container 已就緒。

### `-o wide` 是什麼？

`-o` 是 output，指定輸出格式；`wide` 顯示更多欄位。今天主要用它查看 Pod 所在的 Node。

### 為什麼只有一台 Node 還需要 Scheduler？

排程流程仍然存在，只是只有一台可選。多 Node 環境中，Scheduler 才會在多台符合條件的 Node 之間做選擇。
