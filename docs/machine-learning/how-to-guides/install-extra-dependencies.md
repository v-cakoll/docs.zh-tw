---
title: 安裝額外的 ML.NET 依賴項目
description: 瞭解如何安裝ML.NET包依賴於但未隨 NuGet 套件安裝的任何本機庫
ms.date: 04/02/2020
author: natke
ms.author: nakersha
ms.custom: how-to
ms.openlocfilehash: 0b870542706c065295d48205b7780e568e267ab6
ms.sourcegitcommit: 2b3b2d684259463ddfc76ad680e5e09fdc1984d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80888299"
---
# <a name="install-extra-mlnet-dependencies"></a><span data-ttu-id="18eca-103">安裝額外的 ML.NET 依賴項目</span><span class="sxs-lookup"><span data-stu-id="18eca-103">Install extra ML.NET dependencies</span></span>

<span data-ttu-id="18eca-104">在大多數情況下,在所有作業系統上安裝ML.NET就像引用相應的 NuGet 包一樣簡單。</span><span class="sxs-lookup"><span data-stu-id="18eca-104">In most cases, on all operating systems, installing ML.NET is as simple as referencing the appropriate NuGet package.</span></span>

```bash
dotnet add package Microsoft.ML
```

<span data-ttu-id="18eca-105">但是,在某些情況下,還有其他安裝要求,尤其是在需要本機組件時。</span><span class="sxs-lookup"><span data-stu-id="18eca-105">In some cases though, there are additional installation requirements, particularly when native components are required.</span></span> <span data-ttu-id="18eca-106">本文檔介紹這些案例的安裝要求。</span><span class="sxs-lookup"><span data-stu-id="18eca-106">This document describes the installation requirements for those cases.</span></span> <span data-ttu-id="18eca-107">這些部分按具有附加依賴項的特定`Microsoft.ML.*`NuGet 包細分。</span><span class="sxs-lookup"><span data-stu-id="18eca-107">The sections are broken down by the specific `Microsoft.ML.*` NuGet package that has the additional dependency.</span></span>

## <a name="microsoftmltimeseries-microsoftmlautoml"></a><span data-ttu-id="18eca-108">微軟.ML.時間系列,微軟.ML.自動ML</span><span class="sxs-lookup"><span data-stu-id="18eca-108">Microsoft.ML.TimeSeries, Microsoft.ML.AutoML</span></span>

<span data-ttu-id="18eca-109">這兩個包都依賴於`Microsoft.ML.MKL.Redist`。具有`libiomp`依賴 項。</span><span class="sxs-lookup"><span data-stu-id="18eca-109">Both of these packages have a dependency on `Microsoft.ML.MKL.Redist`, which has a dependency on `libiomp`.</span></span>

### <a name="windows"></a><span data-ttu-id="18eca-110">Windows</span><span class="sxs-lookup"><span data-stu-id="18eca-110">Windows</span></span>

<span data-ttu-id="18eca-111">無需額外的安裝步驟。</span><span class="sxs-lookup"><span data-stu-id="18eca-111">No extra installation steps required.</span></span> <span data-ttu-id="18eca-112">將 NuGet 套件添加到專案時,將安裝庫。</span><span class="sxs-lookup"><span data-stu-id="18eca-112">The library is installed when the NuGet package is added to the project.</span></span>

### <a name="linux"></a><span data-ttu-id="18eca-113">Linux</span><span class="sxs-lookup"><span data-stu-id="18eca-113">Linux</span></span>

1. <span data-ttu-id="18eca-114">安裝儲存函式庫的 GPG 金鑰</span><span class="sxs-lookup"><span data-stu-id="18eca-114">Install the GPG key for the repository</span></span>

    ```bash
    sudo bash
    # <type your user password when prompted.  this will put you in a root shell>
    # cd to /tmp where this shell has write permission
    cd /tmp
    # now get the key:
    wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    # now install that key
    apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    # now remove the public key file exit the root shell
    rm GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
    exit
    ```

2. <span data-ttu-id="18eca-115">新增 MKL 的 APT 儲存函式庫</span><span class="sxs-lookup"><span data-stu-id="18eca-115">Add the APT Repository for MKL</span></span>

    ```bash
    sudo sh -c 'echo deb https://apt.repos.intel.com/mkl all main > /etc/apt/sources.list.d/intel-mkl.list'
    ```

3. <span data-ttu-id="18eca-116">更新套件</span><span class="sxs-lookup"><span data-stu-id="18eca-116">Update packages</span></span>

    ```bash
    sudo apt-get update
    ```

4. <span data-ttu-id="18eca-117">安裝 MKL</span><span class="sxs-lookup"><span data-stu-id="18eca-117">Install MKL</span></span>

    ```bash
    sudo apt-get install <COMPONENT>-<VERSION>.<UPDATE>-<BUILD_NUMBER>
    ```

    <span data-ttu-id="18eca-118">例如：</span><span class="sxs-lookup"><span data-stu-id="18eca-118">For example:</span></span>

    ```bash
    sudo apt-get install intel-mkl-64bit-2020.0-088
    ```

    <span data-ttu-id="18eca-119">確定位置`libiomp.so`</span><span class="sxs-lookup"><span data-stu-id="18eca-119">Determine the location of `libiomp.so`</span></span>

    ```bash
    find /opt -name "libiomp5.so"
    ```

    <span data-ttu-id="18eca-120">例如：</span><span class="sxs-lookup"><span data-stu-id="18eca-120">For example:</span></span>

    ```output
    /opt/intel/compilers_and_libraries_2020.0.166/linux/compiler/lib/intel64_lin/libiomp5.so
    ```

5. <span data-ttu-id="18eca-121">將這裡加入為負載庫路徑:</span><span class="sxs-lookup"><span data-stu-id="18eca-121">Add this location to the load library path:</span></span>

    ```bash
    sudo ldconfig /opt/intel/compilers_and_libraries_2020.0.166/linux/compiler/lib/intel64_li
    ```

### <a name="mac"></a><span data-ttu-id="18eca-122">Mac</span><span class="sxs-lookup"><span data-stu-id="18eca-122">Mac</span></span>

1. <span data-ttu-id="18eca-123">使用`Homebrew`</span><span class="sxs-lookup"><span data-stu-id="18eca-123">Install the library with `Homebrew`</span></span>

    ```bash
    brew update && brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/f5b1ac99a7fba27c19cee0bc4f036775c889b359/Formula/libomp.rb && brew link libomp --force
    ```