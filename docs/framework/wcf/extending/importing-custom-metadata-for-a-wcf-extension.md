---
title: 匯入 WCF 擴充的自訂中繼資料
ms.date: 03/30/2017
ms.assetid: 78beb28f-408a-4c75-9c3c-caefe9595b1a
ms.openlocfilehash: f6f858cbe86bd2965decf42be5daa7b3f7d3c8c2
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70796924"
---
# <a name="importing-custom-metadata-for-a-wcf-extension"></a>匯入 WCF 擴充的自訂中繼資料
在 Windows Communication Foundation （WCF）中，中繼資料匯入是從其中繼資料產生服務或其元件部分的抽象表示的程式。 例如，WCF 可以從服務<xref:System.ServiceModel.Description.ServiceEndpoint>的 WSDL <xref:System.ServiceModel.Channels.Binding>檔匯<xref:System.ServiceModel.Description.ContractDescription>入實例、實例或實例。 若要在 WCF 中匯入服務中繼資料，請<xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType>使用抽象類別的執行。 衍生自<xref:System.ServiceModel.Description.MetadataImporter>類別的型別會實作為匯入元資料格式的支援，以利用 WCF 中的 WS 原則匯入邏輯。  
  
 自訂中繼資料包含系統提供的中繼資料匯入工具無法匯入的 XML 項目。 一般來說，包含自訂 WSDL 延伸與自訂原則判斷提示。  
  
 本節說明如何匯入自訂 WSDL 延伸與原則判斷提示， 但不強調如何匯入處理序。 如需如何使用匯出和匯入中繼資料的類型（不論中繼資料是自訂還是系統支援）的詳細資訊，請參閱[匯出和匯入中繼資料](../feature-details/exporting-and-importing-metadata.md)。  
  
## <a name="overview"></a>總覽  
 型別是包含在 WCF 中<xref:System.ServiceModel.Description.MetadataImporter>的抽象類別的執行。 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> <xref:System.ServiceModel.Description.WsdlImporter> 型別會匯入具有與 <xref:System.ServiceModel.Description.MetadataSet?displayProperty=nameWithType> 物件組合在一起之附加原則的 WSDL 中繼資料。 預設之匯入工具無法辨識的原則判斷提示與 WSDL 延伸會傳遞至任何已註冊的自訂原則與 WSDL 匯入工具以便匯入。 一般來說，匯入工具經過實作後，可支援使用者定義的繫結項目或是可修改匯入的合約。  
  
 本章節內容：  
  
1. 如何實作並使用 <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> 介面，以便在產生描述與程式碼之前將 WSDL 資料公開至自訂匯入工具。 您可以透過此介面來檢查或修改使用特定中繼資料集合來執行的描述類型與程式碼編譯。  
  
2. 如何實作並使用 <xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=nameWithType> 介面，以便在產生描述物件之前將原則判斷提示公開到匯入工具。 您可以使用此介面來檢查或修改以下載原則為基礎的繫結或合約。  
  
 如需匯出自訂 WSDL 和原則判斷提示的詳細資訊，請參閱[匯出 WCF 延伸模組的自訂中繼資料](exporting-custom-metadata-for-a-wcf-extension.md)。  
  
## <a name="importing-custom-wsdl-extensions"></a>匯入自訂 WSDL 延伸  
 若要新增對匯入 WSDL 延伸的支援，請實作 <xref:System.ServiceModel.Description.IWsdlImportExtension> 介面，然後將您的實作新增至 <xref:System.ServiceModel.Description.WsdlImporter.WsdlImportExtensions%2A> 屬性。 <xref:System.ServiceModel.Description.WsdlImporter> 也可以將 <xref:System.ServiceModel.Description.IWsdlImportExtension> 介面 (在您的應用程式組態檔中註冊) 的實作載入。 請注意，預設會註冊一些 WSDL 匯入工具，而且註冊之 WSDL 匯入工具的順序很重要。  
  
 一旦 <xref:System.ServiceModel.Description.WsdlImporter> 載入並使用自訂 WSDL 匯入工具，在匯入處理序開始之前會先呼叫 <xref:System.ServiceModel.Description.IWsdlImportExtension.BeforeImport%2A> 方法來啟用中繼資料的修改作業。 接下來，會匯入合約並呼叫 <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%2A> 方法來啟用由中繼資料匯入之合約的修改作業。 最後，呼叫 <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportEndpoint%2A> 方法來啟用匯入端點的修改作業。  
  
 如需詳細資訊，請參閱[如何：匯入自](how-to-import-custom-wsdl.md)定義 WSDL。  
  
### <a name="importing-custom-policy-assertions"></a>匯入自訂原則判斷提示  
 類型和[system.servicemodel 中繼資料公用程式工具（Svcutil）](../servicemodel-metadata-utility-tool-svcutil-exe.md)會在附加至 WSDL 檔案的原則運算式中，自動處理各種原則判斷提示類型的處理。 <xref:System.ServiceModel.Description.WsdlImporter> 這些工具會負責收集、正常化與合併附加至 WSDL 繫結與 WSDL 連接埠的原則運算式。  
  
 若要新增對匯入自訂原則判斷提示的支援，請實作 <xref:System.ServiceModel.Description.IPolicyImportExtension> 介面，然後將您的實作新增至 <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A> 屬性。 <xref:System.ServiceModel.Description.MetadataImporter> 也可以將 <xref:System.ServiceModel.Description.IPolicyImportExtension> 介面 (在您的應用程式組態檔中註冊) 的實作載入。 請注意，預設會註冊一些原則匯入工具，而且註冊之原則匯入工具的順序很重要。  
  
 中繼資料系統會針對附加至訊息、作業與端點原則主體的每個原則替代項目組合，在所有已註冊的原則匯入延伸上重複呼叫 <xref:System.ServiceModel.Description.IPolicyImportExtension.ImportPolicy%2A?displayProperty=nameWithType> 方法。 一旦匯入 WSDL 連接埠，附加至連接埠與對應 WSDL 繫結的原則會在呼叫原則匯入延伸之前合併。 原則替代項目會透過 <xref:System.ServiceModel.Description.PolicyConversionContext> 以 <xref:System.ServiceModel.Description.PolicyAssertionCollection> 物件形式來提供。 每一個 <xref:System.ServiceModel.Description.PolicyAssertionCollection> 都是由 <xref:System.Xml.XmlElement> 物件所表示的原則判斷提示集合。  
  
 <xref:System.ServiceModel.Description.PolicyConversionContext.Contract%2A> 物件上的 <xref:System.ServiceModel.Description.PolicyConversionContext.BindingElements%2A> 和 <xref:System.ServiceModel.Description.PolicyConversionContext> 屬性都會公開從 WSDL 所匯入的 <xref:System.ServiceModel.Description.ContractDescription> 和 <xref:System.ServiceModel.Channels.BindingElement> 物件。 原則匯入延伸會透過下列方式處理原則判斷提示：尋找特定原則判斷提示類型的執行個體、對 <xref:System.ServiceModel.Description.ContractDescription> 或 <xref:System.ServiceModel.Channels.BindingElement> 物件執行對應的變更，然後再從對應的 <xref:System.ServiceModel.Description.PolicyAssertionCollection> 執行個體移除原則判斷提示。  
  
 `wsp:Optional` 屬性與巢狀原則運算式尚未正常化，因此原則匯入延伸必須處理這些原則建構。 同時，您也可以使用相同的 <xref:System.ServiceModel.Description.ContractDescription> 和 <xref:System.ServiceModel.Channels.BindingElement> 物件來多次呼叫原則匯入延伸，證明了原則匯入延伸應該禁得起此行為的重複使用。  
  
> [!IMPORTANT]
> 無效或不適當的中繼資料可以傳遞至匯入工具。 請確定自訂匯入工具禁得起所有格式的 XML 使用。  
  
## <a name="see-also"></a>另請參閱

- [如何：匯入自訂 WSDL](how-to-import-custom-wsdl.md)
- [如何：匯入自訂原則判斷提示](how-to-import-custom-policy-assertions.md)
- [如何：撰寫 ServiceContractGenerator 的延伸模組](how-to-write-an-extension-for-the-servicecontractgenerator.md)
