---
title: 預計採用 Windows Communication Foundation：簡化未來整合
ms.date: 03/30/2017
ms.assetid: 3028bba8-6355-4ee0-9ecd-c56e614cb474
ms.openlocfilehash: 6392194b3124c999031123225dcc28c8de6171e9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576521"
---
# <a name="anticipating-adopting-the-windows-communication-foundation-easing-future-integration"></a>預計採用 Windows Communication Foundation：簡化未來整合
如果您目前使用 ASP.NET，並預期未來會使用 WCF，本主題會提供指引，確保新的 ASP.NET Web 服務可與 WCF 應用程式妥善搭配運作。  
  
## <a name="general-recommendations"></a>一般建議  
 任何新服務都採用 ASP.NET 2.0。 如此一來就能使用新版本的改進與增強功能。 不過，它也可讓您在相同的應用程式中使用 ASP.NET 2.0 元件搭配 WCF 元件。  
  
## <a name="protocols"></a>通訊協定  
 使用 ASP.NET 2.0 的新功能以驗證是否符合 WS-I Basic Profile 1.1：  
  
```csharp  
[WebService(Namespace = "http://tempuri.org/")]  
[WebServiceBinding(  
     ConformsTo = WsiProfiles.BasicProfile1_1,  
     EmitConformanceClaims=true)]  
public interface IEcho  
```  
  
 符合 WS-I 基本設定檔1.1 的 ASP.NET Web 服務，會使用 WCF 預先定義的系結來與 WCF 用戶端互通 <xref:System.ServiceModel.BasicHttpBinding> 。  
  
## <a name="service-development"></a>服務開發  
 請避免使用 <xref:System.Web.Services.Protocols.SoapDocumentServiceAttribute> 屬性讓訊息不根據 SOAPAction HTTP 標頭而根據 SOAP 訊息的本文元素完整名稱傳送到方法。 WCF 使用 SOAPAction HTTP 標頭來路由傳送訊息。  
  
## <a name="data-representation"></a>資料表示  
 根據預設，<xref:System.Xml.Serialization.XmlSerializer> 序列化型別的目標 XML 在語意上與 <xref:System.Runtime.Serialization.DataContractSerializer> 序列化型別的目標 XML 是一樣的 (前提是 XML 的命名空間已明確定義)。 定義資料類型以用於 ASP.NET Web 服務，以預期未來採用 WCF 時，請執行下列動作：  
  
1. 使用 .NET Framework 類別，而不是 XML 結構描述來定義型別。  
  
2. 僅將 <xref:System.SerializableAttribute> 和 <xref:System.Xml.Serialization.XmlRootAttribute> 新增至類別，並使用後者明確定義該型別的命名空間。 請勿從 <xref:System.Xml.Serialization> 命名空間新增其他屬性，以控制 .NET Framework 類別轉譯成 XML 的方式。  
  
 採用這個方法之後，您稍後應該可以藉由新增 <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute>，將 .NET 類別納入資料合約中，而不用特別提醒針對傳輸需要而序列化類別的目標 XML。 ASP.NET Web 服務在訊息中使用的型別，將能夠以 WCF 應用程式的資料合約的方式處理，而在其他優點之間，在 WCF 應用程式中提供更好的效能。  
  
## <a name="security"></a>安全性  
 請避免使用 Internet Information Services (IIS) 提供的驗證選項。 WCF 用戶端不支援它們。 如果需要保護服務，請使用 WCF 所提供的選項，因為這些選項較豐富，而且是以標準通訊協定為基礎。  
  
## <a name="see-also"></a>請參閱

- [預計採用 Windows Communication Foundation：簡化未來移轉](anticipating-adopting-wcf-migration.md)
