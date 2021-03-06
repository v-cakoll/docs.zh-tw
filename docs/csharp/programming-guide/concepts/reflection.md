---
title: 反映 (C#)
ms.date: 07/20/2015
ms.assetid: f80a2362-953b-4e8e-9759-cd5f334190d4
ms.openlocfilehash: a56fb24b63e4d80dbb67b079466b67cd11672023
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "74711667"
---
# <a name="reflection-c"></a>反映 (C#)

反射提供描述程式集、模組<xref:System.Type>和類型的物件（類型）。 您可以使用反映來動態建立類型的執行個體、將類型繫結至現有的物件，或從現有的物件取得類型，並叫用其方法或存取其欄位及屬性。 如果您在程式碼中使用屬性，則反映可讓您存取它們。 如需詳細資訊，請參閱[屬性](../../../standard/attributes/index.md)。

下面是一個簡單的反射示例，使用<xref:System.Object.GetType>方法 （由`Object`基類中所有類型的繼承） 來獲取變數的類型：

> [!NOTE]
> 請確保在`using System;`*.cs*檔的頂部添加和`using System.Reflection;`頂部。

```csharp
// Using GetType to obtain type information:
int i = 42;
Type type = i.GetType();
Console.WriteLine(type);
```

輸出為： `System.Int32`。

下列範例使用反映以取得所載入組件的完整名稱。

```csharp
// Using Reflection to get information of an Assembly:
Assembly info = typeof(int).Assembly;
Console.WriteLine(info);
```

輸出為： `System.Private.CoreLib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e`。

> [!NOTE]
> C# 關鍵字 `protected` 和 `internal` 在 IL 中沒有任何意義，而且不會用於反映 API 中。 IL 中的對應詞彙是「系列」** 和「組件」**。 若要使用反映來識別 `internal` 方法，請使用 <xref:System.Reflection.MethodBase.IsAssembly%2A> 屬性。 若要識別 `protected internal` 方法，請使用 <xref:System.Reflection.MethodBase.IsFamilyOrAssembly%2A>。

## <a name="reflection-overview"></a>反射概述

反映在下列情況下十分有用：

- 當您需要存取程式中繼資料中的屬性時。 如需詳細資訊，請參閱[擷取儲存於屬性中的資訊](../../../standard/attributes/retrieving-information-stored-in-attributes.md)。
- 如需檢查和具現化組件中的類型。
- 如需在執行階段建置新類型。 使用 <xref:System.Reflection.Emit> 中的類別。
- 對於執行晚期繫結，存取在執行階段建立的類型上的方法。 請參閱[動態載入和使用類型](../../../framework/reflection-and-codedom/dynamically-loading-and-using-types.md)主題。

## <a name="related-sections"></a>相關章節

其他資訊：

- [反射](../../../framework/reflection-and-codedom/reflection.md)
- [檢視類型資訊](../../../framework/reflection-and-codedom/viewing-type-information.md)
- [反映和泛型型別](../../../framework/reflection-and-codedom/reflection-and-generic-types.md)
- <xref:System.Reflection.Emit>
- [擷取儲存於屬性中的資訊](../../../standard/attributes/retrieving-information-stored-in-attributes.md)

## <a name="see-also"></a>另請參閱

- [C# 程式設計指南](../index.md)
- [.NET 中的組件](../../../standard/assembly/index.md)
