---
title: Windows 市集應用程式的網路隔離
ms.date: 03/30/2017
ms.assetid: b064497c-d956-46b8-838d-7a0223c7e200
ms.openlocfilehash: 390a0281f03b08322cc1bee469b601fd5a1547c4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "74802172"
---
# <a name="network-isolation-for-windows-store-apps"></a>Windows 市集應用程式的網路隔離

中的 類 和<xref:System.Net.Http.Headers>命名空間中可用於開發 Windows 應用商店應用或桌面應用。 <xref:System.Net> <xref:System.Net.Http> 在 Windows 應用商店應用中使用時，這些命名空間中的類受網路隔離的影響，網路隔離是 Windows 8 使用的應用程式安全模型的一部分。 必須在 Windows 市集應用程式的應用程式資訊清單中啟用適當的網路功能，讓系統允許網路存取。  
  
## <a name="checklist-for-network-isolation"></a>網路隔離的檢查清單  

使用此檢查清單，確定已設定 Windows 市集應用程式的網路隔離。  
  
1. 決定應用程式所需的網路存取要求方向。 這可以是輸出用戶端起始的要求或輸入未經要求的要求，或是這兩種網路要求類型的組合。  
  
2. 判斷該應用程式將與其通訊的網路資源類型。 應用程式可能需要與家用或工作場所網路上受信任的資源進行通訊。 應用程式可能需要與網際網路上的資源進行通訊。 應用程式可能需要存取這兩種類型的網路資源。  
  
3. 在應用程式資訊清單中設定最低必要網路隔離功能。  
  
4. 部署和執行應用程式，以使用針對疑難排解所提供的網路隔離工具對其進行測試。  
  
有關如何配置網路功能和用於解決網路隔離故障的隔離工具的更多詳細資訊，請參閱 Windows 8.x 應用商店開發人員文檔中[如何配置網路隔離功能](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))。
  
## <a name="see-also"></a>另請參閱

- [連線至 Web 服務](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10))
- [網路隔離的方針和檢查清單](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))
- [快速入門：使用 HttpClient 進行連線](https://docs.microsoft.com/previous-versions/windows/apps/hh781239(v=win.10))
- [如何使用 HttpClient 處理常式](https://docs.microsoft.com/previous-versions/windows/apps/hh781241(v=win.10))
- [如何保護 HttpClient 連線](https://docs.microsoft.com/previous-versions/windows/apps/hh781240(v=win.10))
- [HttpClient 範例](https://code.msdn.microsoft.com/windowsapps/HttpClient-sample-55700664)
