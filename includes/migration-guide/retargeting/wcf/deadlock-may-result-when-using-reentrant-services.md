---
ms.openlocfilehash: dd7d3e445772e4b5ec148576ccd1374d56e251bd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614372"
---
### <a name="deadlock-may-result-when-using-reentrant-services"></a>使用可重新進入服務時，可能會造成死結

#### <a name="details"></a>詳細資料

可重新進入服務可能會造成死結，將服務的執行個體限制為一次執行一個執行緒。 容易發生此問題的服務在其程式碼中會有下列 <xref:System.ServiceModel.ServiceBehaviorAttribute>：

```csharp
[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Reentrant)]
```

#### <a name="suggestion"></a>建議

若要解決此問題，您可執行以下動作：

- 將服務的並行模式設為 <xref:System.ServiceModel.ConcurrencyMode.Single?displayProperty=nameWithType> 或 &lt;System.ServiceModel.ConcurrencyMode.Multiple?displayProperty=nameWithType&gt;。 例如：

```csharp
[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Reentrant)]
```

- 安裝 .NET Framework 4.6.2 的最新更新，或升級至更新版本的 .NET Framework。 這會停用 <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> 中的 <xref:System.Threading.ExecutionContext> 流程。 這是可設定的行為；相當於在您的組態檔中新增下列應用程式設定：

```xml
<appSettings>
  <add key="Switch.System.ServiceModel.DisableOperationContextAsyncFlow" value="true" />
</appSettings>
```

Reentrant 服務的值 `Switch.System.ServiceModel.DisableOperationContextAsyncFlow` 絕對不可設定為 `false`。

| 名稱    | 值       |
|:--------|:------------|
| 影響範圍   | Minor       |
| 版本 | 4.6.2       |
| 類型    | 正在重定目標 |

#### <a name="affected-apis"></a>受影響的 API

- <xref:System.ServiceModel.ServiceBehaviorAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.ConcurrencyMode.Reentrant?displayProperty=nameWithType>
