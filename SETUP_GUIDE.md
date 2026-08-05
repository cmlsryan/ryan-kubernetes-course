# Killercoda 架設步驟

## 1. 建立 GitHub Repository

建議名稱：

`ryan-kubernetes-course`

建立空白 Repository，預設分支使用 `main`。

## 2. 上傳檔案

把這個資料夾內的：

- `README.md`
- `kubernetes-architecture/`

完整上傳到 Repository 根目錄。

正確結構：

```text
ryan-kubernetes-course/
├── README.md
└── kubernetes-architecture/
    ├── index.json
    ├── intro.md
    ├── step1.md
    ├── step2.md
    ├── step3.md
    ├── step4.md
    ├── step5.md
    ├── step6.md
    └── finish.md
```

## 3. 連接 Killercoda

1. 登入 Killercoda。
2. 進入 Creator / Repository 設定。
3. 填入 GitHub Repository，例如：
   `你的GitHub帳號/ryan-kubernetes-course`
4. Branch 填入：
   `main`
5. 複製 Killercoda 提供的 Deploy Key。
6. 到 GitHub Repository：
   `Settings` → `Deploy keys` → `Add deploy key`
7. 貼上 Deploy Key。
8. 不需要開啟寫入權限，保留唯讀即可。
9. 回到 Killercoda 等待同步。

每次 Commit / Push 到指定分支後，Killercoda 會重新同步 Scenario。

## 4. 測試

進入 Creator Scenarios，開啟：

`Kubernetes 架構：從物流中心到第一個 Pod`

依序測試每一頁及每個 `{{exec}}` 按鈕。
