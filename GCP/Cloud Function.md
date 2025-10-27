---
tags:
  - GCP
---
## What is `xxx`, why we need it


## Cloud Run vs Cloud Function

### 基本定位與用途

| 項目       | **Cloud Functions**                      | **Cloud Run**                          |
| -------- | ---------------------------------------- | -------------------------------------- |
| **定位**   | 事件驅動的函式（Function as a Service, FaaS）     | 容器化的應用程式（Container as a Service, CaaS） |
| **用途**   | 執行單一功能邏輯，例如處理 Pub/Sub 訊息、上傳檔案觸發、HTTP 請求等 | 執行完整的 Web 應用、API server、後端服務、微服務       |
| **核心概念** | 「我只要一段函式」                                | 「我需要一個容器化應用程式」                         |

### 部署方式與程式結構
| 項目                   | **Cloud Functions**                                 | **Cloud Run**                                      |
| -------------------- | --------------------------------------------------- | -------------------------------------------------- |
| **部署單位**             | 單一函式（function）                                      | 完整容器（container image）                              |
| **開發語言**             | 受限於支援語言（Node.js、Python、Go、Java、.NET、Ruby 等）         | 任意語言（只要能包成 Docker image）                           |
| **啟動方式**             | 由 GCP 自動管理入口（例如 HTTP trigger、Pub/Sub、Storage event） | 你提供 HTTP endpoint（Cloud Run 會將 request 傳進容器的 port） |
| **需要 Dockerfile 嗎？** | 不需要（GCP 會自動建置 runtime）                              | 必須要有（或使用 buildpacks 自動生成）                          |

### 運行與擴展
| 項目        | **Cloud Functions**                           | **Cloud Run**                            |
| --------- | --------------------------------------------- | ---------------------------------------- |
| **觸發方式**  | 事件（例如 HTTP、Pub/Sub、Storage 觸發）                | HTTP Request                             |
| **自動擴展**  | 是，根據事件自動擴展（無法控制）                              | 是，可以設定最大實例數、並行數（concurrency）等            |
| **並行請求**  | 原先每個實例一次只能處理一個請求，gen2 開始單一實例能對應到 **1000** 個請求 | 每個實例可同時處理多個請求（預設 80）                     |
| **啟動時間**  | 通常較快，但 cold start 仍存在                         | cold start 視容器大小而定，可能比 Cloud Function 稍慢 |
| **長時間運行** | 不建議，執行有時間限制（最多 60 分鐘）                         | 可以長時間運行（適合持續性服務）                         |

## Terminology

## Reference
