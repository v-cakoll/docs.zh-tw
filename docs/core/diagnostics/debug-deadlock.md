---
title: 調試鎖死-.NET Core
description: 本教學課程會逐步引導您在 .NET Core 中偵測鎖定問題。
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 247521176297254180d794d4d4fc850f30e343b0
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/22/2020
ms.locfileid: "86926397"
---
# <a name="debug-a-deadlock-in-net-core"></a>在 .NET Core 中調試鎖死

**本文適用于：✔️** .net CORE 3.1 SDK 和更新版本

在本教學課程中，您將瞭解如何調試鎖死案例。 使用提供的範例[ASP.NET Core web 應用程式](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios)原始程式碼存放庫，您可能會刻意造成鎖死。 端點會遇到停止回應和執行緒累積的情況。 您將瞭解如何使用各種工具來分析問題，例如核心傾印、核心傾印分析和進程追蹤。

在本教學課程中，您將：

> [!div class="checklist"]
>
> - 調查應用程式停止回應
> - 產生核心傾印檔案
> - 分析傾印檔案中的處理常式執行緒
> - 分析呼叫堆疊和同步區塊
> - 診斷並解決鎖死

## <a name="prerequisites"></a>先決條件

教學課程會使用：

- [.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core)或更新版本
- 用於觸發案例的[範例 debug 目標-web 應用程式](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios)
- [dotnet-列出進程的追蹤](dotnet-trace.md)
- [dotnet-](dotnet-dump.md)用來收集和分析傾印檔案的傾印

## <a name="core-dump-generation"></a>核心傾印產生

若要調查應用程式無回應，核心傾印或記憶體傾印可讓您檢查其執行緒的狀態，以及可能發生爭用問題的任何可能鎖定。 使用範例根目錄中的下列命令，執行[範例 debug](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios)應用程式：

```dotnetcli
dotnet run
```

若要尋找處理序識別碼，請使用下列命令：

```dotnetcli
dotnet-trace ps
```

請記下命令輸出中的處理序識別碼。 我們的處理序識別碼是 `4807` ，但您的程式識別碼不同。 流覽至下列 URL，這是範例網站上的 API 端點：

[https://localhost:5001/api/diagscenario/deadlock](https://localhost:5001/api/diagscenario/deadlock)

網站的 API 要求將會停止回應，而不會回應。 讓要求執行約10-15 秒。 然後使用下列命令建立核心傾印：

### <a name="linux"></a>[Linux](#tab/linux)

```bash
sudo dotnet-dump collect -p 4807
```

### <a name="windows"></a>[Windows](#tab/windows)

```console
dotnet-dump collect -p 4807
```

---

## <a name="analyze-the-core-dump"></a>分析核心傾印

若要啟動核心傾印分析，請使用下列命令來開啟核心傾印 `dotnet-dump analyze` 。 引數是稍早收集到的核心傾印檔案的路徑。

```dotnetcli
dotnet-dump analyze  ~/.dotnet/tools/core_20190513_143916
```

因為您正在查看可能的停止回應，所以您想要在程式中進行執行緒活動的整體操作。 您可以使用命令，如下 `threads` 所示：

```console
> threads
*0 0x1DBFF (121855)
 1 0x1DC01 (121857)
 2 0x1DC02 (121858)
 3 0x1DC03 (121859)
 4 0x1DC04 (121860)
 5 0x1DC05 (121861)
 6 0x1DC06 (121862)
 7 0x1DC07 (121863)
 8 0x1DC08 (121864)
 9 0x1DC09 (121865)
 10 0x1DC0A (121866)
 11 0x1DC0D (121869)
 12 0x1DC0E (121870)
 13 0x1DC10 (121872)
 14 0x1DC11 (121873)
 15 0x1DC12 (121874)
 16 0x1DC13 (121875)
 17 0x1DC14 (121876)
 18 0x1DC15 (121877)
 19 0x1DC1C (121884)
 20 0x1DC1D (121885)
 21 0x1DC1E (121886)
 22 0x1DC21 (121889)
 23 0x1DC22 (121890)
 24 0x1DC23 (121891)
 25 0x1DC24 (121892)
 26 0x1DC25 (121893)
...
...
 317 0x1DD48 (122184)
 318 0x1DD49 (122185)
 319 0x1DD4A (122186)
 320 0x1DD4B (122187)
 321 0x1DD4C (122188)
 ```

輸出會顯示目前在進程中執行的所有線程，及其相關聯的偵錯工具執行緒識別碼和作業系統執行緒識別碼。 根據輸出，有超過300個執行緒。

下一步是取得每個執行緒的呼叫堆疊，以進一步瞭解執行緒目前執行的作業。 `clrstack`命令可以用來輸出呼叫堆疊。 它可以輸出單一呼叫堆疊或所有呼叫堆疊。 使用下列命令，輸出進程中所有線程的所有呼叫堆疊：

```console
clrstack -all
```

輸出的代表性部分如下所示：

```console
  ...
  ...
  ...
        Child SP               IP Call Site
00007F2AE37B5680 00007f305abc6360 [GCFrame: 00007f2ae37b5680]
00007F2AE37B5770 00007f305abc6360 [GCFrame: 00007f2ae37b5770]
00007F2AE37B57D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae37b57d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE37B5920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE37B5950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE37B5CA0 00007f30593044af [GCFrame: 00007f2ae37b5ca0]
00007F2AE37B5D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae37b5d70]
OS Thread Id: 0x1dc82
        Child SP               IP Call Site
00007F2AE2FB4680 00007f305abc6360 [GCFrame: 00007f2ae2fb4680]
00007F2AE2FB4770 00007f305abc6360 [GCFrame: 00007f2ae2fb4770]
00007F2AE2FB47D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae2fb47d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE2FB4920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE2FB4950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE2FB4CA0 00007f30593044af [GCFrame: 00007f2ae2fb4ca0]
00007F2AE2FB4D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae2fb4d70]
OS Thread Id: 0x1dc83
        Child SP               IP Call Site
00007F2AE27B3680 00007f305abc6360 [GCFrame: 00007f2ae27b3680]
00007F2AE27B3770 00007f305abc6360 [GCFrame: 00007f2ae27b3770]
00007F2AE27B37D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae27b37d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE27B3920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE27B3950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE27B3CA0 00007f30593044af [GCFrame: 00007f2ae27b3ca0]
00007F2AE27B3D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae27b3d70]
OS Thread Id: 0x1dc84
        Child SP               IP Call Site
00007F2AE1FB2680 00007f305abc6360 [GCFrame: 00007f2ae1fb2680]
00007F2AE1FB2770 00007f305abc6360 [GCFrame: 00007f2ae1fb2770]
00007F2AE1FB27D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae1fb27d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE1FB2920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE1FB2950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE1FB2CA0 00007f30593044af [GCFrame: 00007f2ae1fb2ca0]
00007F2AE1FB2D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae1fb2d70]
OS Thread Id: 0x1dc85
        Child SP               IP Call Site
00007F2AE17B1680 00007f305abc6360 [GCFrame: 00007f2ae17b1680]
00007F2AE17B1770 00007f305abc6360 [GCFrame: 00007f2ae17b1770]
00007F2AE17B17D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae17b17d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE17B1920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE17B1950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE17B1CA0 00007f30593044af [GCFrame: 00007f2ae17b1ca0]
00007F2AE17B1D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae17b1d70]
OS Thread Id: 0x1dc86
        Child SP               IP Call Site
00007F2AE0FB0680 00007f305abc6360 [GCFrame: 00007f2ae0fb0680]
00007F2AE0FB0770 00007f305abc6360 [GCFrame: 00007f2ae0fb0770]
00007F2AE0FB07D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae0fb07d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE0FB0920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE0FB0950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE0FB0CA0 00007f30593044af [GCFrame: 00007f2ae0fb0ca0]
00007F2AE0FB0D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae0fb0d70]
OS Thread Id: 0x1dc87
        Child SP               IP Call Site
00007F2AE07AF680 00007f305abc6360 [GCFrame: 00007f2ae07af680]
00007F2AE07AF770 00007f305abc6360 [GCFrame: 00007f2ae07af770]
00007F2AE07AF7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae07af7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE07AF920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE07AF950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE07AFCA0 00007f30593044af [GCFrame: 00007f2ae07afca0]
00007F2AE07AFD70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae07afd70]
OS Thread Id: 0x1dc88
        Child SP               IP Call Site
00007F2ADFFAE680 00007f305abc6360 [GCFrame: 00007f2adffae680]
00007F2ADFFAE770 00007f305abc6360 [GCFrame: 00007f2adffae770]
00007F2ADFFAE7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2adffae7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2ADFFAE920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2ADFFAE950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2ADFFAECA0 00007f30593044af [GCFrame: 00007f2adffaeca0]
00007F2ADFFAED70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2adffaed70]
...
...
```

觀察所有300多個執行緒的呼叫堆疊，會顯示大多數執行緒共用通用呼叫堆疊的模式：

```console
OS Thread Id: 0x1dc88
        Child SP               IP Call Site
00007F2ADFFAE680 00007f305abc6360 [GCFrame: 00007f2adffae680]
00007F2ADFFAE770 00007f305abc6360 [GCFrame: 00007f2adffae770]
00007F2ADFFAE7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2adffae7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2ADFFAE920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2ADFFAE950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2ADFFAECA0 00007f30593044af [GCFrame: 00007f2adffaeca0]
00007F2ADFFAED70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2adffaed70]
```

呼叫堆疊似乎會顯示要求到達我們的鎖死方法中，而後者又會進行的呼叫 `Monitor.ReliableEnter` 。 這個方法表示執行緒正嘗試進入監視器鎖定。 他們正在等候鎖定的可用性。 這可能是由不同的執行緒鎖定。

接下來的步驟是找出哪個執行緒實際持有監視器鎖定。 因為監視器通常會將鎖定資訊儲存在同步處理區塊資料表中，所以我們可以使用 `syncblk` 命令來取得詳細資訊：

```console
> syncblk
Index         SyncBlock MonitorHeld Recursion Owning Thread Info          SyncBlock Owner
   43 00000246E51268B8          603         1 0000024B713F4E30 5634  28   00000249654b14c0 System.Object
   44 00000246E5126908            3         1 0000024B713F47E0 51d4  29   00000249654b14d8 System.Object
-----------------------------
Total           344
CCW             1
RCW             2
ComClassFactory 0
Free            0
```

這兩個有趣的資料行是**MonitorHeld**和**擁有線程資訊**。 **MonitorHeld**資料行會顯示執行緒是否取得監視器鎖定，以及等候中線程的數目。 [**擁有線程資訊**] 資料行會顯示目前擁有監視器鎖定的執行緒。 執行緒資訊有三個不同的個子。 第二個 subcolumn 顯示作業系統執行緒識別碼。

此時，我們知道兩個不同的執行緒（0x5634 和0x51d4）會保存監視器鎖定。 下一步是查看這些執行緒的作用。 我們需要檢查它們是否在無限期地停滯住。 讓我們使用 `setthread` 和 `clrstack` 命令來切換至每個執行緒，並顯示呼叫堆疊。

若要查看第一個執行緒，請執行 `setthread` 命令，並尋找0x5634 執行緒的索引（我們的索引是28）。 鎖死函式正在等候取得鎖定，但它已經擁有鎖定。 它處於鎖死狀態，等待它已經保留的鎖定。

```console
> setthread 28
> clrstack
OS Thread Id: 0x5634 (28)
        Child SP               IP Call Site
0000004E46AFEAA8 00007fff43a5cbc4 [GCFrame: 0000004e46afeaa8]
0000004E46AFEC18 00007fff43a5cbc4 [GCFrame: 0000004e46afec18]
0000004E46AFEC68 00007fff43a5cbc4 [HelperMethodFrame_1OBJ: 0000004e46afec68] System.Threading.Monitor.Enter(System.Object)
0000004E46AFEDC0 00007FFE5EAF9C80 testwebapi.Controllers.DiagScenarioController.DeadlockFunc() [C:\Users\dapine\Downloads\Diagnostic_scenarios_sample_debug_target\Controllers\DiagnosticScenarios.cs @ 58]
0000004E46AFEE30 00007FFE5EAF9B8C testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_0() [C:\Users\dapine\Downloads\Diagnostic_scenarios_sample_debug_target\Controllers\DiagnosticScenarios.cs @ 26]
0000004E46AFEE80 00007FFEBB251A5B System.Threading.ThreadHelper.ThreadStart_Context(System.Object) [/_/src/System.Private.CoreLib/src/System/Threading/Thread.CoreCLR.cs @ 44]
0000004E46AFEEB0 00007FFE5EAEEEEC System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/_/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
0000004E46AFEF20 00007FFEBB234EAB System.Threading.ThreadHelper.ThreadStart() [/_/src/System.Private.CoreLib/src/System/Threading/Thread.CoreCLR.cs @ 93]
0000004E46AFF138 00007ffebdcc6b63 [GCFrame: 0000004e46aff138]
0000004E46AFF3A0 00007ffebdcc6b63 [DebuggerU2MCatchHandlerFrame: 0000004e46aff3a0]
```

第二個執行緒是相似的。 它也會嘗試取得其已擁有的鎖定。 所有等待的剩餘300個以上執行緒，很可能也會等待造成鎖死的其中一個鎖定。

## <a name="see-also"></a>另請參閱

- [dotnet-列出進程的追蹤](dotnet-trace.md)
- [dotnet-](dotnet-counters.md)檢查 managed 記憶體使用量的計數器
- [dotnet-](dotnet-dump.md)用來收集和分析傾印檔案的傾印
- [dotnet/診斷](https://github.com/dotnet/diagnostics/tree/master/documentation/tutorial)

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [.NET Core 中提供哪些診斷工具](index.md)
