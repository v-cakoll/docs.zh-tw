---
title: DiffGram
ms.date: 03/30/2017
ms.assetid: 037f3991-7bbc-424b-b52e-8b03585d3e34
ms.openlocfilehash: 2c521ef33c98234dac5f4b819a800cd524218462
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79151152"
---
# <a name="diffgrams"></a>DiffGram
DiffGram 是 XML 格式，可用來識別資料項目的目前和原始版本。 <xref:System.Data.DataSet> 使用 DiffGram 格式以載入保存內容，並將內容序列化以透過網路連接傳輸。 <xref:System.Data.DataSet>當 編寫為 DiffGram 時，它將使用所有必要的資訊填充 DiffGram，以準確重新創建<xref:System.Data.DataSet>的內容（儘管不是架構），包括**原始**行和**當前**行版本的列值、行錯誤資訊和行順序。  
  
 從 XML Web Service 傳送和擷取 <xref:System.Data.DataSet> 時，會隱含使用 DiffGram 格式。 此外，使用**ReadXml**方法<xref:System.Data.DataSet>從 XML 載入 的內容時，或使用**WriteXml**方法<xref:System.Data.DataSet>在 XML 中寫入 的內容時，可以指定將內容作為 DiffGram 讀取或寫入。 有關詳細資訊，請參閱從[XML 載入資料集](loading-a-dataset-from-xml.md)並將[資料集內容寫入 XML 資料](writing-dataset-contents-as-xml-data.md)。  
  
 雖然 DiffGram 格式在 .NET Framework 中主要是用來當做 <xref:System.Data.DataSet> 內容的序列化格式，您也可以使用 DiffGrams 修改 Microsoft SQL Server 資料庫中的資料表資料。  
  
 通過將所有表的內容寫入**\<衍分圖>** 元素生成衍分圖。  
  
### <a name="to-generate-a-diffgram"></a>若要產生 Diffgram  
  
1. 產生根資料表 (即不具任何父項目的資料表) 的清單。  
  
2. 針對清單中的每個資料表及其子代 (Descendant)，在 Diffgram 的第一個區段中寫出所有資料列的目前版本。  
  
3. 對於 中的每個表<xref:System.Data.DataSet>，請寫出 Diffgram**\<的>前**部分中所有行的原始版本（如果有）。  
  
4. 對於有錯誤的行，請在 Diffgram>部分**\<的錯誤**中寫入錯誤內容。  
  
 Diffgram 會從 XML 檔案的開頭依序處理到結尾。  
  
### <a name="to-process-a-diffgram"></a>若要處理 Diffgram  
  
1. 處理 Diffgram 的第一個區段，其中包含資料列的目前版本。  
  
2. 處理包含已修改行和已刪除行的原始行版本的第二**\<個**或>節。  
  
    > [!NOTE]
    > 如果資料列標示為已刪除，則刪除作業可能也會刪除該資料列的子代，根據目前 `Cascade` 的 <xref:System.Data.DataSet> 屬性而定。  
  
3. 處理>部分**\<的錯誤**。 針對此區段中每個項目的指定資料列和資料行設定錯誤資訊。  
  
> [!NOTE]
> 如果將 <xref:System.Data.XmlWriteMode> 設定為 Diffgram，則目標 <xref:System.Data.DataSet> 和原始 <xref:System.Data.DataSet> 的內容可能不同。  
  
## <a name="diffgram-format"></a>DiffGram 格式  
 DiffGram 格式分成三個區段：目前資料、原始 (或「過去」) 資料和錯誤區段，如下列範例所示。  
  
```xml  
<?xml version="1.0"?>  
<diffgr:diffgram
         xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"  
         xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1"  
         xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
  
   <DataInstance>  
   </DataInstance>  
  
  <diffgr:before>  
  </diffgr:before>  
  
  <diffgr:errors>  
  </diffgr:errors>  
</diffgr:diffgram>  
```  
  
 DiffGram 格式由下列資料區塊組成：  
  
 **\<**  ***資料實例***  **>**  
 此元素的名稱***DataInstance***用於本文檔中的說明目的。 ***DataInstance***元素表示<xref:System.Data.DataSet>的 或 行<xref:System.Data.DataTable>。 元素將包含<xref:System.Data.DataSet>或<xref:System.Data.DataTable>的名稱，而不是*DataInstance。* 無論這個 DiffGram 格式區塊是否經過修改，都會包含目前的資料。 已修改的元素或行使用**diffgr：hasChanges**注釋標識。  
  
 **\<困難：>之前**  
 這個 DiffGram 格式的區塊包含資料列的原始版本。 此塊中的元素使用**diffgr：id**注釋與***DataInstance***塊中的元素匹配。  
  
 **\<差異：錯誤>**  
 此 DiffGram 格式塊包含***DataInstance***塊中特定行的錯誤資訊。 此塊中的元素使用**diffgr：id**注釋與***DataInstance***塊中的元素匹配。  
  
## <a name="diffgram-annotations"></a>DiffGram 註解  
 DiffGram 使用數種註釋，將來自不同 DiffGram 區塊的項目關聯起來，這些 DiffGram 區塊分別代表 <xref:System.Data.DataSet> 中的不同資料列版本或錯誤訊息。  
  
 下表描述了在 DiffGram 命名空間 urn 中定義的 DiffGram 注釋 **：架構-microsoft-com：xml-diffgram-v1**。  
  
|Annotation|描述|  
|----------------|-----------------|  
|**id**|用於將**\<分差中的元素：在>和****\<差異：錯誤之前，>** 塊與***DataInstance*****>** 塊**\<** 中的元素配對。 具有**diffgr：id**注釋的值位於表單 *[表名稱][Row識別碼]* 中。 例如：`<Customers diffgr:id="Customers1">`。|  
|**父 Id**|標識**\<*****DataInstance*****>** 塊中的哪個元素是當前元素的父元素。 具有**差異值的值：父Id**注釋位於表單 *[表名稱][Row識別碼]* 中。 例如：`<Orders diffgr:parentId="Customers1">`。|  
|**hasChanges**|將**\<*****DataInstance*****>** 塊中的行標識為已修改的行。 **具有更改**注釋可以具有以下兩個值之一：<br /><br /> **插入**<br /> 標識 **"已添加**的行"。<br /><br /> **改 性**<br /> 標識**在\<分段：>塊之前**包含**原始**行版本的 **"已修改**"行。 請注意，**刪除的**行將在**\<diffgr：>** 塊之前具有**原始**行版本，但在**\<*****DataInstance*****>** 塊中沒有注釋的元素。|  
|**hasErrors**|使用**RowError**標識**\<** ***DataInstance*****>** 塊中的行。 錯誤元素放置在**\<差異：錯誤>** 塊中。|  
|**錯誤**|包含**\<diffgr：error>** 塊中特定元素的**RowError**文本。|  
  
 <xref:System.Data.DataSet> 將其內容讀取或寫為 DiffGram 時，亦會包含其他附註。 下表描述了這些附加注釋，這些注釋在命名空間**urn：架構-微軟-com：xml-msdata**中定義。  
  
|Annotation|描述|  
|----------------|-----------------|  
|**RowOrder**|保留原始資料的資料列順序，並識別特定 <xref:System.Data.DataTable> 中資料列的索引。|  
|**隱藏**|將列標識為將**列映射**屬性設置為**映射類型。** 該屬性以**msdata 格式編寫：隱藏** *[列名]*="*值*"。 例如：`<Customers diffgr:id="Customers1" msdata:hiddenContactTitle="Owner">`。<br /><br /> 請注意，隱藏的資料行只有在包含資料時才會寫為 DiffGram 屬性。 否則會予以忽略。|  
  
## <a name="sample-diffgram"></a>範例 DiffGram  
 以下是 DiffGram 格式的範例。 這個範例顯示在確認變更前，資料表中資料列的更新結果。 CustomerID 為 "ALFKI" 的資料列已經修改，但尚未更新。 因此，在**\<*****DataInstance*****>** 塊中有一個 **"** 客戶1"的"客戶1"**的"** 當前"行，在**\<diffgr：>** 塊之前，有一個**原始**行，其中具有"客戶1"的**diffgr：id。** 具有"ANATR"客戶 ID 的行包含一個**RowError，** 因此帶有"`diffgr:hasErrors="true"`**\<帶**"的批號，並且在 diffgr：error>塊中存在相關元素。  
  
```xml  
<diffgr:diffgram xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
  <CustomerDataSet>  
    <Customers diffgr:id="Customers1" msdata:rowOrder="0" diffgr:hasChanges="modified">  
      <CustomerID>ALFKI</CustomerID>  
      <CompanyName>New Company</CompanyName>  
    </Customers>  
    <Customers diffgr:id="Customers2" msdata:rowOrder="1" diffgram:hasErrors="true">  
      <CustomerID>ANATR</CustomerID>  
      <CompanyName>Ana Trujillo Emparedados y Helados</CompanyName>  
    </Customers>  
    <Customers diffgr:id="Customers3" msdata:rowOrder="2">  
      <CustomerID>ANTON</CustomerID>  
      <CompanyName>Antonio Moreno Taquera</CompanyName>  
    </Customers>  
    <Customers diffgr:id="Customers4" msdata:rowOrder="3">  
      <CustomerID>AROUT</CustomerID>  
      <CompanyName>Around the Horn</CompanyName>  
    </Customers>  
  </CustomerDataSet>  
  <diffgr:before>  
    <Customers diffgr:id="Customers1" msdata:rowOrder="0">  
      <CustomerID>ALFKI</CustomerID>  
      <CompanyName>Alfreds Futterkiste</CompanyName>  
    </Customers>  
  </diffgr:before>  
  <diffgr:errors>  
    <Customers diffgr:id="Customers2" diffgr:Error="An optimistic concurrency violation has occurred for this row."/>  
  </diffgr:errors>  
</diffgr:diffgram>  
```  
  
## <a name="see-also"></a>另請參閱

- [在資料集中使用 XML](using-xml-in-a-dataset.md)
- [從 XML 載入資料集](loading-a-dataset-from-xml.md)
- [將資料集內容當作 XML 資料寫入](writing-dataset-contents-as-xml-data.md)
- [DataSet、DataTable 和 DataView](index.md)
- [ADO.NET 概觀](../ado-net-overview.md) \(部分機器翻譯\)
