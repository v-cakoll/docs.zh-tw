---
title: ICorProfilerCallback7 介面
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback7
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.assetid: a0be019e-aaa1-4036-990f-565f114d4b5c
ms.openlocfilehash: 9a49d8e1ff31942c6564ab560d6726b9ede26466
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499138"
---
# <a name="icorprofilercallback7-interface"></a>ICorProfilerCallback7 介面
[在 .NET Framework 4.6.1 及更新版本中支援]  
  
 [ICorProfilerCallback6](icorprofilercallback6-interface.md) 子類別提供一種回呼方法，以讓 Common Language Runtime 用來通知分析工具已更新記憶體中模組相關聯的符號資料流。  
  
## <a name="methods"></a>方法  
  
|方法|描述|  
|------------|-----------------|  
|[ModuleInMemorySymbolsUpdated 方法](icorprofilercallback7-moduleinmemorysymbolsupdated-method.md)|通知分析工具已更新記憶體中模組相關聯的符號資料流。|  
  
## <a name="requirements"></a>規格需求  
 **平台：** 請參閱[系統需求](../../get-started/system-requirements.md)。  
  
 **標頭：** CorProf.idl、CorProf.h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [分析介面](profiling-interfaces.md)
