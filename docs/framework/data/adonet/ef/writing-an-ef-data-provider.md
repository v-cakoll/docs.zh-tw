---
title: 撰寫 Entity Framework 資料提供者
ms.date: 03/30/2017
ms.assetid: 092e88c4-a301-453a-b5c3-5740c6575a9f
ms.openlocfilehash: 29aa8cb1c6b31d9ada6b01860d76bcf03d37416c
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70854157"
---
# <a name="writing-an-entity-framework-data-provider"></a>撰寫 Entity Framework 資料提供者
本節將討論如何撰寫 Entity Framework 提供者，以支援 SQL Server 以外的資料來源。 Entity Framework 包含支援 SQL Server 的提供者。  
  
## <a name="introducing-the-entity-framework-provider-model"></a>Entity Framework 提供者模型簡介  
 Entity Framework 與資料庫無關，而且您可以使用 ADO.NET 提供者模型來撰寫提供者，以連接到不同的資料來源集合。  
  
 Entity Framework 資料提供者 (使用 ADO.NET 資料提供者模型所建置) 會執行下列功能：  
  
- 將實體資料模型 (EDM) 基本型別對應到提供者類型。  
  
- 公開提供者特有的函式。  
  
- 為指定的 DbQueryCommandTree 產生提供者特有的命令，以支援 Entity Framework 查詢。  
  
- 為指定的 DbModificationCommandTree 產生提供者特定的更新命令，以透過 Entity Framework 支援更新。  
  
- 公開存放結構定義的對應檔案，以便支援根據資料庫產生模型。  
  
- 透過概念模型公開中繼資料 (如資料表和檢視表)。  
  
 ![b42a7a5c&#45;0ac0&#45;4911&#45;86be&#45;0460a78760ba](./media/b42a7a5c-0ac0-4911-86be-0460a78760ba.gif "b42a7a5c-0ac0-4911-86be-0460a78760ba")  
  
## <a name="sample"></a>範例  
 如需支援 SQL Server 以外之資料來源的 Entity Framework 提供者範例，請參閱[Entity Framework 範例提供者](https://code.msdn.microsoft.com/windowsdesktop/Entity-Framework-Sample-6a9801d0)。  
  
## <a name="in-this-section"></a>本節內容  
 [SQL 產生](sql-generation.md)  
  
 [修改 SQL 產生](modification-sql-generation.md)  
  
 [提供者資訊清單規格](provider-manifest-specification.md)  
  
## <a name="see-also"></a>另請參閱

- [處理資料提供者](working-with-data-providers.md)
