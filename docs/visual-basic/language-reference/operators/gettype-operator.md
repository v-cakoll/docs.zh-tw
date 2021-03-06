---
title: GetType Operator
ms.date: 07/20/2015
f1_keywords:
- vb.GetType
helpviewer_keywords:
- GetType operator [Visual Basic]
- GetType keyword [Visual Basic]
ms.assetid: 4f733297-2503-4607-850c-15eba65fff90
ms.openlocfilehash: 37644a9c37ffde084120c5f1e1ee8c87a04ffc3c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371151"
---
# <a name="gettype-operator-visual-basic"></a>GetType 運算子 (Visual Basic)
傳回 <xref:System.Type> 指定之類型的物件。 <xref:System.Type>物件會提供類型的相關資訊，例如其屬性、方法和事件。  
  
## <a name="syntax"></a>語法  
  
```vb  
GetType(typename)  
```  
  
## <a name="parameters"></a>參數  
  
|參數|說明|  
|---|---|  
|`typename`|您想要其資訊的類型名稱。|  
  
## <a name="remarks"></a>備註  
 運算子會傳回 `GetType` <xref:System.Type> 所指定的物件 `typename` 。 您可以在中傳遞任何已定義類型的名稱 `typename` 。 這包括下列項目：  
  
- 任何 Visual Basic 資料類型，例如 `Boolean` 或 `Date` 。  
  
- 任何 .NET Framework 類別、結構、模組或介面，例如 <xref:System.ArgumentException?displayProperty=nameWithType> 或 <xref:System.Double?displayProperty=nameWithType> 。  
  
- 您的應用程式所定義的任何類別、結構、模組或介面。  
  
- 您的應用程式所定義的任何陣列。  
  
- 您的應用程式所定義的任何委派。  
  
- Visual Basic、.NET Framework 或您的應用程式所定義的任何列舉。  
  
 如果您想要取得物件變數的型別物件，請使用 <xref:System.Type.GetType%2A?displayProperty=nameWithType> 方法。  
  
 在 `GetType` 下列情況下，運算子可能會很有用：  
  
- 您必須在執行時間存取類型的中繼資料。 <xref:System.Type>物件提供中繼資料，例如類型成員和部署資訊。 例如，您需要此來反映元件。 如需詳細資訊，請參閱 <xref:System.Reflection?displayProperty=nameWithType> 。  
  
- 您想要比較兩個物件參考，以查看它們是否參考相同類型的實例。 如果有的話，會傳回 `GetType` 相同物件的參考 <xref:System.Type> 。  
  
## <a name="example"></a>範例  
 下列範例顯示 `GetType` 使用中的運算子。  
  
 [!code-vb[VbVbalrOperators#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#26)]  
  
## <a name="see-also"></a>另請參閱

- [Visual Basic 中的運算子優先順序](operator-precedence.md)
- [依功能列出運算子](operators-listed-by-functionality.md)
- [運算子和運算式](../../programming-guide/language-features/operators-and-expressions/index.md)
