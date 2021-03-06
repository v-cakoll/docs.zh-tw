---
ms.openlocfilehash: 9544b65f31772d0f4cee918528a73171fec4de99
ms.sourcegitcommit: 348bb052d5cef109a61a3d5253faa5d7167d55ac
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2020
ms.locfileid: "82021819"
---
### <a name="change-in-default-value-of-useshellexecute"></a>使用殼牌執行的預設值變更

<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType>在 .NET `false` Core 上具有預設值。 在 .NET 框架上`true`,預設值為 。

#### <a name="change-description"></a>變更描述

<xref:System.Diagnostics.Process.Start%2A?displayProperty=nameWithType>允許您直接啟動應用程式,例如,使用諸如啟動「畫圖」`Process.Start("mspaint.exe")`等代碼。 它還允許您間接啟動關聯的應用程式,如果<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType>設定為`true`。 在 .NET Framework<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType>`true`上, 預設值為`Process.Start("mytextfile.txt")`,這意味著如果 將 *.txt*檔與該編輯器關聯,則如將啟動記事本等代碼。 為了防止在 .NET 框架上間接啟動應用,必須顯式<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType>`false`設置為 。 在 .NET 核心上<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType>,預設`false`值為 。 這意味著默認情況下,在調用`Process.Start`時不會啟動關聯的應用程式。

出於性能原因,在 .NET Core 中引入了此更改。 通常,<xref:System.Diagnostics.Process.Start%2A?displayProperty=nameWithType>用於直接啟動應用程式。 直接啟動應用不需要涉及 Windows 外殼並產生相關的性能成本。 為了使此預設案例更快,.NET Core 將<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute?displayProperty=nameWithType>預設`false`值 更改為 。 如果需要,您可以選擇加入較慢的路徑。

#### <a name="version-introduced"></a>介紹的版本

2.1

> [!NOTE]
> 在早期版本的 .NET`UseShellExecute`Core 中,未為 Windows 實現。

#### <a name="recommended-action"></a>建議的動作

如果應用依賴於舊行為,請<xref:System.Diagnostics.Process.Start(System.Diagnostics.ProcessStartInfo)?displayProperty=nameWithType>調用物件<xref:System.Diagnostics.ProcessStartInfo.UseShellExecute>`true`<xref:System.Diagnostics.ProcessStartInfo>上的 「設置為」調用。

#### <a name="category"></a>類別

核心 .NET 函式庫

#### <a name="affected-apis"></a>受影響的 API

- <xref:System.Diagnostics.Process.Start%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.ProcessStartInfo?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.Diagnostics.Process.Start`
- `M:System.Diagnostics.ProcessStartInfo`

-->
