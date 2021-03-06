---
title: LINQ to SQL 查詢
ms.date: 03/30/2017
ms.assetid: f4897aaa-7f44-4c20-a471-b948c2971aae
ms.openlocfilehash: 478138d26d6cdf656b2487c637a5eb13784126e9
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634376"
---
# <a name="linq-to-sql-queries"></a>LINQ to SQL 查詢
您可以使用與 LINQ 相同的語法來定義 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 查詢。 唯一的差別在於查詢中參考的物件會對應至資料庫中的項目。 如需詳細資訊，請參閱 [LINQ 查詢簡介 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 會將您撰寫的查詢轉譯為對等 SQL 查詢，並將它們傳送給伺服器進行處理。 更精確的說，應用程式會使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] API 來要求執行查詢。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 提供者接著會將查詢轉換為 SQL 文字，並將執行委派給 ADO 提供者。 而 ADO 提供者會將查詢結果傳回為 `DataReader`。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 提供者會將 ADO 結果轉譯為使用者物件的 <xref:System.Linq.IQueryable> 集合。  
  
> [!NOTE]
> .NET Framework 內建型別的大部分方法和運算子都具有直接的翻譯至 SQL。 LINQ 無法轉譯的程式會產生執行時間例外狀況。 如需詳細資訊，請參閱[SQL-CLR 型別對應](sql-clr-type-mapping.md)。  
  
 下表顯示 LINQ 和 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 查詢專案之間的相似性和差異。  
  
|項目|LINQ 查詢|[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 查詢|  
|----------|----------------|----------------------------------------------------------------------|  
|保留查詢之區域變數的傳回型別 (適用於傳回序列的查詢)|泛型 `IEnumerable`|泛型 `IQueryable`|  
|指定資料來源|使用 `From` （Visual Basic）或 `from` （C#）子句|相同|  
|篩選|使用 `Where`/`where` 子句|相同|  
|群組|使用 `Group…By`/`groupby` 子句|相同|  
|選取 (投影)|使用 `Select`/`select` 子句|相同|  
|延後執行與立即執行|請參閱[LINQ 查詢簡介（C#）](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)|相同|  
|實作聯結 (Join)|使用 `Join`/`join` 子句|可以使用 `Join`/`join` 子句，但更有效率地使用 <xref:System.Data.Linq.Mapping.AssociationAttribute> 屬性。 如需詳細資訊，請參閱[跨關聯性查詢](querying-across-relationships.md)。|  
|遠端執行與本機執行||如需詳細資訊，請參閱[遠端與本機執行](remote-vs-local-execution.md)。|  
|資料流 (Streaming) 與快取查詢|不適用於本機記憶體案例||  
  
## <a name="see-also"></a>請參閱

- [LINQ 查詢簡介 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)
- [基本 LINQ 查詢作業](../../../../../csharp/programming-guide/concepts/linq/basic-linq-query-operations.md)
- [LINQ 查詢作業中的型別關聯性](../../../../../csharp/programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)
- [查詢概念](query-concepts.md)
