# WGPerformanceCalculator iOS CI/CD 配置

本專案主要使用 **Xcode Cloud** 作為 CI/CD 平台，Codemagic 僅用於測試和比較。

## 主要平台：Xcode Cloud

### 觸發條件
- **所有分支的推送**（預設行為）
- 在 Xcode 中直接配置和管理

### 環境變數設定
在 Xcode Cloud Dashboard 中設定：
- `APP_STORE_ID`: 您的 Apple Developer Account Apple ID
- `WGPC_PASSWORD`: 您的 App-Specific Password

### 日常使用
```bash
# 正常開發流程 - 觸發 Xcode Cloud
git add .
git commit -m "Your commit message"
git push origin main
```

## 測試平台：Codemagic

### 觸發條件
- **分支**: `test/codemagic/*` (例如: `test/codemagic/feature-test`)
- **標籤**: `codemagic-test-*` (例如: `codemagic-test-v1.0.0`)

### 環境變數設定
在 Codemagic 專案設定中設定：
- `APP_STORE_ID`: 您的 Apple Developer Account Apple ID
- `WGPC_PASSWORD`: 您的 App-Specific Password

### 測試使用
```bash
# 建立測試分支
git checkout -b test/codemagic/feature-test
git push origin test/codemagic/feature-test

# 或使用測試標籤
git tag codemagic-test-v1.0.0
git push origin codemagic-test-v1.0.0
```

## 工作流程

### 開發階段
1. 使用 Xcode Cloud 進行日常建置和測試
2. 所有功能開發都在主分支或功能分支進行

### 測試階段
1. 當需要測試 Codemagic 功能時，建立 `test/codemagic/*` 分支
2. 比較兩個平台的建置結果和效能

### 發布階段
1. 主要使用 Xcode Cloud 進行正式發布
2. 可選擇使用 Codemagic 作為備用方案

## 注意事項

1. **主要流程**: Xcode Cloud 是主要 CI/CD 平台
2. **避免干擾**: Codemagic 只在明確的測試分支/標籤時觸發
3. **資源管理**: 減少不必要的 Codemagic 建置以節省資源
4. **成本控制**: 主要依賴 Xcode Cloud 的免費額度

## 檔案說明

- `codemagic.yaml`: Codemagic 測試配置
- `exportOptions.plist`: iOS 應用程式匯出設定（兩個平台共用）
- `README.md`: 本說明文件 