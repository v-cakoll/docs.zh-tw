---
ms.openlocfilehash: f6fd75c5b49156f44d31c650ea452eb549f13b0e
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901961"
---
### <a name="mvc-jsonresult-moved-to-microsoftaspnetcoremvccore"></a><span data-ttu-id="0beb6-101">MVC： JsonResult 已移至 AspNetCore. Core</span><span class="sxs-lookup"><span data-stu-id="0beb6-101">MVC: JsonResult moved to Microsoft.AspNetCore.Mvc.Core</span></span>

<span data-ttu-id="0beb6-102">`JsonResult` 已移至 `Microsoft.AspNetCore.Mvc.Core` 元件。</span><span class="sxs-lookup"><span data-stu-id="0beb6-102">`JsonResult` has moved to the `Microsoft.AspNetCore.Mvc.Core` assembly.</span></span> <span data-ttu-id="0beb6-103">這個類型是用來在[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Json)中定義。</span><span class="sxs-lookup"><span data-stu-id="0beb6-103">This type used to be defined in [Microsoft.AspNetCore.Mvc.Formatters.Json](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Json).</span></span> <span data-ttu-id="0beb6-104">已將元件層級[[TypeForwardedTo]](xref:System.Runtime.CompilerServices.TypeForwardedToAttribute)屬性加入 `Microsoft.AspNetCore.Mvc.Formatters.Json`，以解決大部分使用者的這個問題。</span><span class="sxs-lookup"><span data-stu-id="0beb6-104">An assembly-level [[TypeForwardedTo]](xref:System.Runtime.CompilerServices.TypeForwardedToAttribute) attribute was added to `Microsoft.AspNetCore.Mvc.Formatters.Json` to address this issue for the majority of users.</span></span> <span data-ttu-id="0beb6-105">使用協力廠商程式庫的應用程式可能會遇到問題。</span><span class="sxs-lookup"><span data-stu-id="0beb6-105">Apps that use third-party libraries may encounter issues.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="0beb6-106">引進的版本</span><span class="sxs-lookup"><span data-stu-id="0beb6-106">Version introduced</span></span>

<span data-ttu-id="0beb6-107">3.0 Preview 6</span><span class="sxs-lookup"><span data-stu-id="0beb6-107">3.0 Preview 6</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="0beb6-108">舊的行為</span><span class="sxs-lookup"><span data-stu-id="0beb6-108">Old behavior</span></span>

<span data-ttu-id="0beb6-109">已成功使用以2.2 為基礎的程式庫組建應用程式。</span><span class="sxs-lookup"><span data-stu-id="0beb6-109">An app using a 2.2-based library builds successfully.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="0beb6-110">新的行為</span><span class="sxs-lookup"><span data-stu-id="0beb6-110">New behavior</span></span>

<span data-ttu-id="0beb6-111">使用以2.2 為基礎之程式庫的應用程式無法編譯。</span><span class="sxs-lookup"><span data-stu-id="0beb6-111">An app using a 2.2-based library fails compilation.</span></span> <span data-ttu-id="0beb6-112">提供的錯誤包含下列文字的變化：</span><span class="sxs-lookup"><span data-stu-id="0beb6-112">An error containing a variation of the following text is provided:</span></span>

```
The type 'JsonResult' exists in both 'Microsoft.AspNetCore.Mvc.Core, Version=3.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60' and 'Microsoft.AspNetCore.Mvc.Formatters.Json, Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'
```

<span data-ttu-id="0beb6-113">如需這類問題的範例，請參閱[dotnet/aspnetcore # 7220](https://github.com/dotnet/aspnetcore/issues/7220)。</span><span class="sxs-lookup"><span data-stu-id="0beb6-113">For an example of such an issue, see [dotnet/aspnetcore#7220](https://github.com/dotnet/aspnetcore/issues/7220).</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="0beb6-114">變更的原因</span><span class="sxs-lookup"><span data-stu-id="0beb6-114">Reason for change</span></span>

<span data-ttu-id="0beb6-115">ASP.NET Core 組合的平台層級變更，如[aspnet/公告 # 325](https://github.com/aspnet/Announcements/issues/325)所述。</span><span class="sxs-lookup"><span data-stu-id="0beb6-115">Platform-level changes to the composition of ASP.NET Core as described at [aspnet/Announcements#325](https://github.com/aspnet/Announcements/issues/325).</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="0beb6-116">建議的動作</span><span class="sxs-lookup"><span data-stu-id="0beb6-116">Recommended action</span></span>

<span data-ttu-id="0beb6-117">針對2.2 版 `Microsoft.AspNetCore.Mvc.Formatters.Json` 所編譯的程式庫可能需要重新編譯，才能解決所有取用者的問題。</span><span class="sxs-lookup"><span data-stu-id="0beb6-117">Libraries compiled against the 2.2 version of `Microsoft.AspNetCore.Mvc.Formatters.Json` may need to recompile to address the problem for all consumers.</span></span> <span data-ttu-id="0beb6-118">如果受影響，請聯絡程式庫作者。</span><span class="sxs-lookup"><span data-stu-id="0beb6-118">If affected, contact the library author.</span></span> <span data-ttu-id="0beb6-119">要求重新編譯程式庫至目標 ASP.NET Core 3.0。</span><span class="sxs-lookup"><span data-stu-id="0beb6-119">Request recompilation of the library to target ASP.NET Core 3.0.</span></span>

#### <a name="category"></a><span data-ttu-id="0beb6-120">分類</span><span class="sxs-lookup"><span data-stu-id="0beb6-120">Category</span></span>

<span data-ttu-id="0beb6-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0beb6-121">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="0beb6-122">受影響的 API</span><span class="sxs-lookup"><span data-stu-id="0beb6-122">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Mvc.JsonResult?displayProperty=nameWithType>

<!-- 

### Affected APIs

`T:Microsoft.AspNetCore.Mvc.JsonResult`

-->