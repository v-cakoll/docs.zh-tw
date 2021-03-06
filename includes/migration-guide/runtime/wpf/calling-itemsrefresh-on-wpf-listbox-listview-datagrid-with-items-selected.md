---
ms.openlocfilehash: 710d1517397f423fa40cc0c4a26c3499aac6179e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620083"
---
### <a name="calling-itemsrefresh-on-a-wpf-listbox-listview-or-datagrid-with-items-selected-can-cause-duplicate-items-to-appear-in-the-element"></a>在已選取項目的 WPF ListBox、ListView 或 DataGrid 上呼叫 Items.Refresh 會導致在項目中出現重複的項目

#### <a name="details"></a>詳細資料

在 .NET Framework 4.5 中，如果 <xref:System.Windows.Controls.ListBox?displayProperty=fullName> 中已選取項目，從程式碼呼叫 ListBox.Items.Refresh 可能會導致選取的項目在清單中重複出現。 <xref:System.Windows.Controls.ListView?displayProperty=fullName> 和 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> 會發生類似的問題。 這在 .NET Framework 4.6 中已修正。

#### <a name="suggestion"></a>建議

您可以在呼叫 <xref:System.Windows.Data.CollectionView.Refresh?displayProperty=fullName> 之前以程式設計方式取消選取項目，然後在呼叫完成之後重新選取項目來解決此問題。 此外，此問題在 .NET Framework 4.6 中已修正，因此可藉由升級至該版 .NET Framework 來解決。

| 名稱    | 值       |
|:--------|:------------|
| 影響範圍   |Minor|
|版本|4.5|
|類型|執行階段

#### <a name="affected-apis"></a>受影響的 API

-<xref:System.Windows.Data.CollectionView.Refresh?displayProperty=nameWithType></li></ul>|
