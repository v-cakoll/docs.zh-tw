---
ms.openlocfilehash: e3b9711ac66901d69838de4c9f309d086b06fd4d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620115"
---
### <a name="xslt-forward-compat-now-works"></a>XSLT 往後相容性現在可運作

#### <a name="details"></a>詳細資料

在 .NET Framework 4 中，XSLT 1.0 往後相容性有下列問題：<ul><li>如果樣式表的版本設為 2.0，而且剖析器遇到無法辨識的 XSLT 1.0 結構，則載入樣式表會失敗。</li><li>如果樣式表版本設為 1.1， <code>xsl:sort</code> 結構就無法排序資料。</li></ul>在 .NET Framework 4.5 中，這些問題已獲得修正，而且 XSLT 1.0 往後相容性模式可正常運作。

#### <a name="suggestion"></a>建議

大部分的應用程式應該不會受到影響，不過在某些情況下，資料的排序方式將不同於 xsl:sort 現在遵守的方式。 如果在 1.1 樣式表中使用 <code>xsl:sort</code>，確認應用程式不會依據未排序的資料順序。 如果應用程式依賴 4.0 排序行為，請從樣式表中移除 <code>xsl:sort</code>。

| 名稱    | 值       |
|:--------|:------------|
| 影響範圍   |Edge|
|版本|4.5|
|類型|執行階段

#### <a name="affected-apis"></a>受影響的 API

-<xref:System.Xml.Xsl.XslCompiledTransform?displayProperty=nameWithType></li></ul>|
