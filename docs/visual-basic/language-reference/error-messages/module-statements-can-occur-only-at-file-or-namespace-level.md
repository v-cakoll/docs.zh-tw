---
title: "'Module' 陳述式只可以發生在檔案或命名空間層級"
ms.date: 07/20/2015
f1_keywords:
- bc30617
- vbc30617
helpviewer_keywords:
- BC30617
ms.assetid: 5e9de8e5-d26b-4fb2-9e28-814413fe9cef
ms.openlocfilehash: d01c30571fc34e142300ac8706c56d5e99175fcf
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397203"
---
# <a name="module-statements-can-occur-only-at-file-or-namespace-level"></a>'Module' 陳述式只可以發生在檔案或命名空間層級
`Module`語句必須出現在原始程式檔的頂端，緊接在 `Option` 和 `Imports` 語句、全域屬性、命名空間宣告，但在其他所有宣告之前。  
  
 **錯誤識別碼：** BC30617  
  
## <a name="to-correct-this-error"></a>更正這個錯誤  
  
- 將 `Module` 陳述式移至命名空間宣告或原始程式檔的上方。  
  
## <a name="see-also"></a>另請參閱

- [Module 陳述式](../statements/module-statement.md)
