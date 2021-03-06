---
title: 無伺服器體系結構 - 無伺服器應用
description: 探索由無伺服器體系結構（包括 Web 應用、移動和 IoT）支援的各種體系結構和應用。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 838dcd7b41df0d8297e1ae10f9c04a8d5b83b332
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "72522402"
---
# <a name="serverless-architecture"></a>無伺服器架構

使用[無伺服器](https://azure.com/serverless)體系結構的方法有很多種。 本章介紹集成無伺服器通用體系結構的示例。 它還包括可能構成額外挑戰或在實現無伺服器時需要額外考慮的問題。 最後，提供了幾個設計示例來說明各種無伺服器用例。

無伺服器主機通常使用現有的基於容器或 PaaS 層來管理無伺服器實例。 例如，Azure 函數基於[Azure 應用服務](https://docs.microsoft.com/azure/app-service/)。 應用服務用於橫向擴展實例和管理執行 Azure 函數代碼的運行時。 對於基於 Windows 的函數，主機以 PaaS 運行，並擴展 .NET 運行時。 對於基於 Linux 的函數，主機利用容器。

![Azure 函數體系結構](./media/azure-functions-architecture.png)

WebJobs Core 為函數提供了執行上下文。 語言運行時運行腳本、執行庫並託管目的語言的框架。 例如，Node.js 用於運行 JavaScript 函數，.NET 框架用於運行 C# 函數。 本章稍後將瞭解有關語言和平臺選項的詳細資訊。

某些專案可能受益于對無伺服器採取"全功能"方法。 嚴重依賴微服務的應用程式可以使用無伺服器技術實現所有微服務。 大多數應用都是混合的，遵循 N 層設計，對有意義的元件使用無伺服器，因為這些元件是模組化的，並且可獨立擴展。 為了説明理解這些方案，本節介紹一些使用無伺服器的常見體系結構示例。

## <a name="full-serverless-back-end"></a>完全無伺服器後端

完全無伺服器後端是多種類型的方案的理想選擇，尤其是在構建新的或"綠色欄位"應用程式時。 具有較大 API 表面積的應用程式可能受益于將每個 API 實現為無伺服器函數。 基於微服務體系結構的應用是另一個可以作為完全無伺服器後端實現的示例。 微服務通過各種協定相互通信。 具體方案包括：

- 基於 API 的 SaaS 產品（例如：財務支付處理器）。
- 消息驅動應用程式（例如：設備監視解決方案）。
- 專注于服務之間集成的應用（例如：航空公司預訂應用程式）。
- 定期運行的進程（例如：基於計時器的資料庫清理）。
- 專注于資料轉換的應用（例如：檔上載觸發的導入）。
- 提取轉換和載入 （ETL） 進程。

本文檔後面介紹的其他更具體的用例。

## <a name="monoliths-and-starving-the-beast"></a>巨石和"餓死野獸"

一個常見的挑戰是將現有的整體應用程式遷移到雲。 風險最小的方法是將完全"提升並轉移到"虛擬機器上。 許多商店更願意利用遷移作為更新其代碼庫的機會。 遷移的實用方法稱為"餓死野獸"。 在這種情況下，單體將"按照"的方式遷移，以便開始。 然後，所選服務進行現代化改造。 在某些情況下，服務的簽名與原始名稱相同：它只是作為函數託管。 用戶端將更新以使用新服務而不是單體終結點。 在此期間，資料庫複製等步驟使微服務能夠託管自己的存儲，即使事務仍由單體處理也是如此。 最終，所有用戶端都遷移到新服務。 在替換所有功能之前，單體將"餓死"（其服務不再調用）。 無伺服器和代理的組合可以促進大部分遷移。

![無伺服器單體遷移](./media/serverless-monolith-migration.png)

要瞭解有關此方法的更多詳細資訊，請觀看視頻：[使用無伺服器 Azure 函數將應用帶到雲](https://channel9.msdn.com/Events/Connect/2017/E102)中。

## <a name="web-apps"></a>Web 應用程式

Web 應用程式是無伺服器應用程式的絕佳候選項。 目前，Web 應用有兩種常見方法：伺服器驅動和用戶端驅動（如單頁應用程式或 SPA）。 伺服器驅動的 Web 應用通常使用中介軟體層發出 API 呼叫以呈現 Web UI。 SPA 應用程式直接從瀏覽器進行 REST API 呼叫。 在這兩種情況下，無伺服器可以通過提供必要的業務邏輯來適應中介軟體或 REST API 請求。 常見的體系結構是站起來羽量級靜態 Web 服務器。 單頁應用程式 （SPA） 提供 HTML、CSS、JavaScript 和其他瀏覽器資源。 然後，Web 應用連接到後端的微服務。

## <a name="mobile-back-ends"></a>移動後端

無伺服器應用的事件驅動模式使其成為移動後端的理想選擇。 行動裝置觸發事件，無伺服器代碼執行以滿足請求。 利用無伺服器模型使開發人員能夠增強業務邏輯，而無需部署完整的應用程式更新。 無伺服器方法還使團隊能夠共用終結點並並行工作。

移動開發人員無需成為伺服器方面的專家即可構建業務邏輯。 傳統上，移動應用連接到本機服務。 構建服務層需要瞭解伺服器平臺和程式設計範例。 開發人員使用操作來預配伺服器並對其進行適當配置。 有時，在構建部署管道上花費了幾天甚至幾周的時間。 所有這些挑戰都由無伺服器解決。

無伺服器抽象伺服器端依賴項，使開發人員能夠專注于業務邏輯。 例如，使用 JavaScript 框架構建應用程式的移動開發人員也可以使用 JavaScript 構建無伺服器函數。 無伺服器主機管理作業系統、Node.js 實例以承載代碼、包依賴項等。 開發人員提供了一組簡單的輸入和一個用於輸出的標準範本。 然後，他們可以專注于構建和測試業務邏輯。 因此，他們能夠利用現有技能為移動應用構建後端邏輯，而無需學習新平臺或成為"伺服器端開發人員"。

![無伺服器移動後端](./media/serverless-mobile-backend.png)

大多數雲供應商提供基於移動的無伺服器產品，可簡化整個移動開發生命週期。 產品可以自動調配資料庫以保留資料、處理 DevOps 問題、提供基於雲的生成和測試框架，以及使用開發人員首選語言編寫業務流程腳本的能力。 採用以移動為中心的無伺服器方法可以簡化流程。 無伺服器可消除為移動後端預配、配置和維護伺服器的巨大開銷。

## <a name="internet-of-things-iot"></a>物聯網 (IoT)

IoT 是指一起聯網的物理物件。 它們有時稱為"連接設備"或"智慧設備"。 汽車和自動售貨機的所有產品都可以連接併發送從庫存到感應器資料（如溫度和濕度）的資訊。 在企業中，IoT 通過監視和自動化提供業務流程改進。 IoT 資料可用於調節大型倉庫中的氣候或通過供應鏈跟蹤庫存。 物聯網可以感知化學品洩漏，並在檢測到煙霧時致電消防部門。

設備和資訊的絕對量往往要求一個事件驅動的體系結構來路由和處理消息。 無伺服器是理想的解決方案，原因如下：

- 隨著設備和資料量的增加，實現擴展。
- 適應添加新端點以支援新設備和感應器。
- 便於獨立版本控制，以便開發人員無需部署整個系統即可更新特定設備的業務邏輯。
- 恢復能力和減少停機時間。

IoT 的普及導致多個專門關注 IoT 問題的無伺服器產品，例如[Azure IoT 中心](https://docs.microsoft.com/azure/iot-hub)。 無伺服器自動執行設備註冊、策略實施、跟蹤甚至將代碼部署到*邊緣*設備等任務。 邊緣是指連接到 Internet 但不屬於 Internet 活動的一部分的感應器和執行器等設備。

>[!div class="step-by-step"]
>[上一個](architecture-approaches.md)
>[下一個](serverless-architecture-considerations.md)
