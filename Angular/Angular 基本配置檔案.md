

## `app.config.ts` 

**用途：**

- 定義整個應用程式的全域 providers 配置
- 在 `main.ts` 中透過 `bootstrapApplication()` 使用
- 配置路由、HTTP client、動畫等服務

## `angular.json`

Angular CLI 的**專案等級配置檔案**，控制整個 workspace 的建置、測試、部署流程。

**用途：**

- 定義專案結構與建置目標
- 配置開發伺服器、production 建置選項
- 設定檔案路徑、資源處理、環境變數
- 配置 SSR 建置輸出

## `tsconfig.app.json`

為專案層級的 typescript 配置，相對於 `tsconfig.json` 來說，`tsconfig.json` 主要是提供比較通用的配置。

而`tsconfig.app.json`則可以用來 :
- 針對應用程式程式碼的特定編譯設定
- 定義哪些檔案要被編譯
- 排除測試檔案和其他不需要的檔案

## `app.config.server.ts`

專門用於配置 SSR 的檔案。

## `app.router.server.ts`

專門用於配置 SSR 路由的黨案。