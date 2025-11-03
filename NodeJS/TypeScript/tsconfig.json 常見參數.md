## tsconfig.json

**位置：** 專案根目錄 **作用：** TypeScript 編譯器的設定檔

**範例內容：**

```json
{
  "compilerOptions": {
    /* 基本選項 */
    "target": "ES2020",                     // 編譯目標版本
    "module": "commonjs",                   // 模組系統
    "lib": ["ES2020"],                      // 可用的標準函式庫
    
    /* 輸入輸出 */
    "rootDir": "./src",                     // TypeScript 原始碼位置
    "outDir": "./dist",                     // 編譯後的 JavaScript 輸出位置
    
    /* 型別檢查（強烈建議開啟）*/
    "strict": true,                         // 啟用所有嚴格檢查
    "noImplicitAny": true,                  // 不允許隱含的 any 型別
    "strictNullChecks": true,               // 嚴格檢查 null 和 undefined
    
    /* 模組解析 */
    "esModuleInterop": true,                // 改善模組相容性
    "moduleResolution": "node",             // 使用 Node.js 的模組解析策略
    
    /* 其他 */
    "skipLibCheck": true,                   // 跳過函式庫型別檢查（加快編譯）
    "forceConsistentCasingInFileNames": true // 強制檔名大小寫一致
  },
  "include": ["src/**/*"],                  // 要編譯的檔案
  "exclude": ["node_modules", "dist"]       // 排除的資料夾
}
```

**常用選項說明：**

- `target`：編譯成哪個版本的 JavaScript（ES5, ES2020, ESNext 等）
- `module`：使用哪種模組系統（commonjs, es2015, esnext 等）
- `rootDir`：TypeScript 原始碼的根目錄
- `outDir`：編譯後的 JavaScript 檔案要放在哪裡
- `strict`：啟用所有嚴格型別檢查（建議開啟）
- `include`：哪些檔案要被編譯
- `exclude`：哪些檔案要被排除
