---
title: 如何：解決不明確的時間
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], ambiguous time
- ambiguous time [.NET Framework]
ms.assetid: 2cf5fb25-492c-4875-9245-98cac8348e97
ms.openlocfilehash: ad69c0984a9d8c01ebd2198486cd0f6492a6116e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281501"
---
# <a name="how-to-resolve-ambiguous-times"></a>如何：解決不明確的時間

不明確的時間是指對應到多個國際標準時間 (UTC) 的時間。 發生時機是往回調整時鐘時間，例如從時區的日光節約時間轉換到其標準時間期間。 處理不明確的時間時，您可以執行下列其中一項：

- 假設如何將時間對應至 UTC。 例如，您可以假設不明確的時間一律是以時區的標準時間表示。

- 如果不明確的時間是使用者所輸入的資料項目，可交由使用者解決此不明確狀況。

本主題說明如何藉由假設它代表時區的標準時間來解決不明確的時間。

### <a name="to-map-an-ambiguous-time-to-a-time-zones-standard-time"></a>將不明確的時間對應至時區標準時間

1. 呼叫 <xref:System.TimeZoneInfo.IsAmbiguousTime%2A> 方法來判斷時間是否不明確。

2. 如果時間不明確，請從 <xref:System.TimeSpan> 時區的屬性所傳回的物件減去時間 <xref:System.TimeZoneInfo.BaseUtcOffset%2A> 。

3. 呼叫 `static` （ `Shared` 在 Visual Basic .net 中） <xref:System.DateTime.SpecifyKind%2A> 方法，將 UTC 日期和時間值的 <xref:System.DateTime.Kind%2A> 屬性設定為 <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> 。

## <a name="example"></a>範例

下列範例說明如何藉由假設它代表當地時區的標準時間，將不明確的時間轉換為 UTC。

[!code-csharp[System.TimeZone2.Concepts#10](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#10)]
[!code-vb[System.TimeZone2.Concepts#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#10)]

此範例包含名為的方法 `ResolveAmbiguousTime` ，可判斷 <xref:System.DateTime> 傳遞給它的值是否不明確。 如果此值不明確，則方法會傳回 <xref:System.DateTime> 代表對應 UTC 時間的值。 方法會藉由從本地時間減去當地時區的屬性值來處理這項轉換 <xref:System.TimeZoneInfo.BaseUtcOffset%2A> 。

通常不明確的時間是藉由呼叫 <xref:System.TimeZoneInfo.GetAmbiguousTimeOffsets%2A> 方法來抓取 <xref:System.TimeSpan> 物件陣列，其中包含不明確時間的可能 UTC 位移。 不過，此範例會進行任意假設，即不明確的時間應該一律對應至時區標準時間。 屬性會傳回 <xref:System.TimeZoneInfo.BaseUtcOffset%2A> UTC 與時區標準時間之間的位移。

在此範例中，會透過屬性來建立本機時區的所有參考，而 <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> 當地時區永遠不會指派給物件變數。 這是建議的作法，因為呼叫 <xref:System.TimeZoneInfo.ClearCachedData%2A?displayProperty=nameWithType> 方法會使指派當地時區的任何物件失效。

## <a name="compiling-the-code"></a>編譯程式碼

這個範例需要：

- <xref:System>使用語句匯入的命名空間 `using` （在 c # 程式碼中為必要項）。

## <a name="see-also"></a>另請參閱

- [日期、時間和時區](index.md)
- [如何：讓使用者解決不明確的時間](let-users-resolve-ambiguous-times.md)
