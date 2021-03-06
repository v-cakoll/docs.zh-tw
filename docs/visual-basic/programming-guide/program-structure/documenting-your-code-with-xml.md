---
title: 使用 XML 加入程式碼註解
ms.date: 07/20/2015
helpviewer_keywords:
- XML [Visual Basic], documenting code
- XML comments, Visual Basic
- Visual Basic code, documenting with XML
ms.assetid: a0d35dc7-c5f9-4d74-92ff-a1c6f28d5235
ms.openlocfilehash: f391fb909cfe4de8f27afb24d6db389e2c8cdfae
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590925"
---
# <a name="document-your-code-with-xml-visual-basic"></a>使用 XML 記錄您的程式碼（Visual Basic）

在 Visual Basic 中，您可以使用 XML 來記錄程式碼。

## <a name="xml-documentation-comments"></a>XML 文件註解

Visual Basic 提供簡單的方式來自動建立專案的 XML 檔。 您可以自動產生類型和成員的 XML 基本架構，然後提供摘要、每個參數的描述性檔，以及其他備註。 使用適當的設定時，XML 檔會自動發出至 XML 檔案，該檔案具有與您專案相同的根檔案名。 如需詳細資訊，請參閱[-doc](../../reference/command-line-compiler/doc.md)。

您可以使用 xml 檔案，或以 XML 的方式操作。 這個檔案位於與專案的輸出 .exe 或 .dll 檔案相同的目錄中。

XML 檔的開頭為 `'''` 。 這些註解在處理時有一些限制：

- 文件必須是語式正確的 XML。 如果 XML 的格式不正確，則會產生警告，而且檔檔案會包含批註，指出發生錯誤。

- 開發人員可以自由建立自己的標記集合。 有一組建議的標記（請參閱[XML 註解標記](../../language-reference/xmldoc/index.md)）。 其中一些建議的標記具有特殊意義：

  - \<param>標記是用來描述參數。 如果使用，編譯器會驗證參數存在，而且所有參數在文件中都有描述。 如果驗證失敗，編譯器會發出警告。

  - `cref` 屬性可以附加至任何標記，以提供程式碼項目的參考。 編譯器會驗證此程式碼項目存在。 如果驗證失敗，編譯器會發出警告。 在 `Imports` 尋找屬性中所描述的類型時，編譯器也會遵循任何語句 `cref` 。

  - \<summary>Visual Studio 中的 IntelliSense 會使用此標記來顯示類型或成員的其他資訊。

## <a name="related-sections"></a>相關章節

如需有關建立含有檔批註之 XML 檔案的詳細資訊，請參閱下列主題：

- [-doc](../../reference/command-line-compiler/doc.md)

- [XML 註解標籤](../../language-reference/xmldoc/index.md)

- [處理 XML 檔案](processing-the-xml-file.md)

- [如何：建立 XML 文件](how-to-create-xml-documentation.md)

- [Visual Studio 中的 XML 工具](/visualstudio/xml-tools/xml-tools-in-visual-studio)

## <a name="see-also"></a>另請參閱

- [使用 Visual Basic 開發應用程式](../../developing-apps/index.md)
- [Visual Basic 程式設計指南](../index.md)
