---
title: 運算子多載
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- operators [.NET Framework], overloads
- names [.NET Framework], overloaded operators
- member design guidelines, operators
- overloaded operators
ms.assetid: 37585bf2-4c27-4dee-849a-af70e3338cc1
ms.openlocfilehash: 893b7d1f76dfb059a0ddca77dfd8654812e9ae12
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289729"
---
# <a name="operator-overloads"></a>運算子多載
運算子多載可讓架構類型看起來像是內建的語言基本專案。

 雖然在某些情況下允許且有用，但應該謹慎使用運算子多載。 在許多情況下，會濫用運算子多載，例如當架構設計工具開始使用運算子進行應該是簡單方法的作業時。 下列指導方針可協助您決定何時及如何使用運算子多載。

 ❌請避免定義運算子多載，但不包括在應該類似基本（內建）類型的類型中。

 ✔️請考慮在型別中定義運算子多載，而該類型應該類似于基本類型。

 例如， <xref:System.String?displayProperty=nameWithType> 已 `operator==` 定義和 `operator!=` 。

 ✔️確實在代表數位的結構中定義運算子多載（例如 <xref:System.Decimal?displayProperty=nameWithType> ）。

 ❌定義運算子多載時，不要太刻意。

 當運算子多載立即明顯明瞭作業的結果時，它會很有用。 例如，能夠從另一個減去一個並取得，是合理的 <xref:System.DateTime> `DateTime` <xref:System.TimeSpan> 。 不過，不適合使用邏輯 union 運算子來結合兩個資料庫查詢，或使用 shift 運算子寫入資料流程。

 ❌除非至少有一個運算元是定義多載的類型，否則請勿提供運算子多載。

 ✔️以對稱方式執行多載運算子。

 例如，如果您多載 `operator==` ，您也應該多載 `operator!=` 。 同樣地，如果您多載 `operator<` ，您也應該多載 `operator>` ，依此類推。

 ✔️請考慮提供具有對應于每個多載運算子之易記名稱的方法。

 許多語言都不支援運算子多載。 基於這個理由，建議多載運算子的型別包含次要方法，並具有適當的網域特定名稱，以提供對等的功能。

 下表包含運算子清單和對應的易記方法名稱。

|C # 運算子符號|中繼資料名稱|易記名稱|
|-------------------------|-------------------|-------------------|
|`N/A`|`op_Implicit`|`To<TypeName>/From<TypeName>`|
|`N/A`|`op_Explicit`|`To<TypeName>/From<TypeName>`|
|`+ (binary)`|`op_Addition`|`Add`|
|`- (binary)`|`op_Subtraction`|`Subtract`|
|`* (binary)`|`op_Multiply`|`Multiply`|
|`/`|`op_Division`|`Divide`|
|`%`|`op_Modulus`|`Mod or Remainder`|
|`^`|`op_ExclusiveOr`|`Xor`|
|`& (binary)`|`op_BitwiseAnd`|`BitwiseAnd`|
|<code>&#124;</code>|`op_BitwiseOr`|`BitwiseOr`|
|`&&`|`op_LogicalAnd`|`And`|
|<code>&#124;&#124;</code>|`op_LogicalOr`|`Or`|
|`=`|`op_Assign`|`Assign`|
|`<<`|`op_LeftShift`|`LeftShift`|
|`>>`|`op_RightShift`|`RightShift`|
|`N/A`|`op_SignedRightShift`|`SignedRightShift`|
|`N/A`|`op_UnsignedRightShift`|`UnsignedRightShift`|
|`==`|`op_Equality`|`Equals`|
|`!=`|`op_Inequality`|`Equals`|
|`>`|`op_GreaterThan`|`CompareTo`|
|`<`|`op_LessThan`|`CompareTo`|
|`>=`|`op_GreaterThanOrEqual`|`CompareTo`|
|`<=`|`op_LessThanOrEqual`|`CompareTo`|
|`*=`|`op_MultiplicationAssignment`|`Multiply`|
|`-=`|`op_SubtractionAssignment`|`Subtract`|
|`^=`|`op_ExclusiveOrAssignment`|`Xor`|
|`<<=`|`op_LeftShiftAssignment`|`LeftShift`|
|`%=`|`op_ModulusAssignment`|`Mod`|
|`+=`|`op_AdditionAssignment`|`Add`|
|`&=`|`op_BitwiseAndAssignment`|`BitwiseAnd`|
|<code>&#124;=</code>|`op_BitwiseOrAssignment`|`BitwiseOr`|
|`,`|`op_Comma`|`Comma`|
|`/=`|`op_DivisionAssignment`|`Divide`|
|`--`|`op_Decrement`|`Decrement`|
|`++`|`op_Increment`|`Increment`|
|`- (unary)`|`op_UnaryNegation`|`Negate`|
|`+ (unary)`|`op_UnaryPlus`|`Plus`|
|`~`|`op_OnesComplement`|`OnesComplement`|

### <a name="overloading-operator-"></a>多載運算子 = =
 多載非常 `operator ==` 複雜。 運算子的語義必須與數個其他成員相容，例如 <xref:System.Object.Equals%2A?displayProperty=nameWithType> 。

### <a name="conversion-operators"></a>轉換運算子
 轉換運算子是一元運算子，允許從某種類型轉換成另一種類型。 運算子必須定義為運算元或傳回類型的靜態成員。 轉換運算子有兩種類型：隱含和明確。

 ❌如果使用者不清楚預期這類轉換，請勿提供轉換運算子。

 ❌請勿在類型的網域外部定義轉換運算子。

 例如，、 <xref:System.Int32> <xref:System.Double> 和 <xref:System.Decimal> 都是數數值型別，而不是 <xref:System.DateTime> 。 因此，不應該有轉換運算子來將轉換成 `Double(long)` `DateTime` 。 在這種情況下，建議使用「函式」。

 ❌如果轉換可能有損失真，請不要提供隱含轉換運算子。

 例如，不應該有從到的隱含轉換， `Double` `Int32` 因為的 `Double` 範圍比更寬 `Int32` 。 即使轉換可能有損失真，也可以提供明確轉換運算子。

 ❌請勿從隱含轉換擲回例外狀況。

 使用者很難以瞭解發生了什麼事，因為他們可能不知道轉換正在進行中。

 <xref:System.InvalidCastException?displayProperty=nameWithType>如果對 cast 運算子的呼叫導致損失真轉換，且運算子的合約不允許損失真轉換，✔️會擲回。

 *部分©2005、2009 Microsoft Corporation。已保留擁有權限。*

 獲 Pearson Education, Inc. 的授權再版，從 Krzysztof Cwalina 和 Brad Abrams 撰寫，並在 2008 年 10 月 22 日由 Addison-Wesley Professional 出版，作為 Microsoft Windows Development Series 一部份的 [Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) 節錄。**

## <a name="see-also"></a>另請參閱

- [成員設計方針](member.md)
- [架構設計方針](index.md)
