---
title: 在 SLES-.NET Core 上安裝 .NET Core
description: 示範在 SLES 上安裝 .NET Core SDK 和 .NET Core 執行時間的各種方式。
author: adegeo
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: 8f64efcc8206b47855871104e5b6914570c06da0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619412"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-sles"></a>在 SLES 上安裝 .NET Core SDK 或 .NET Core 執行時間

SLES 支援 .NET Core。 本文說明如何在 SLES 上安裝 .NET Core。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="supported-distributions"></a>支援的發行版本

下表是 SLES 12 SP2 和 SLES 15 上目前支援的 .NET Core 版本清單。 在[.Net Core 版本達到終止支援](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或不再支援 SLES 版本之前，會持續支援這些版本。

- ✔️表示仍然支援 SLES 或 .NET Core 的版本。
- A ❌ 表示該 sles 版本不支援 sles 或 .Net Core 的版本。
- 當 SLES 版本和 .NET Core 版本都✔️時，就會支援該作業系統和 .NET 組合。

| SLES                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5 Preview （僅限手動安裝） |
|------------------------|---------------|---------------|----------------|
| ✔️ [15](#sles-15-)     | ✔️2。1        | ✔️3。1        | ✔️ 5.0 Preview |
| ✔️ [12 SP2](#sles-12-) | ✔️2。1        | ✔️3。1        | ✔️ 5.0 Preview |

已不再支援下列 .NET Core 版本。 這些下載仍會保持發佈：

- 3.0
- 2.2
- 2.0

## <a name="how-to-install-other-versions"></a>如何安裝其他版本

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="sles-15-"></a>SLES 15 ✔️

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/15/packages-microsoft-prod.rpm
```

目前，SLES 15 Microsoft 存放庫安裝套件會將*Microsoft*存放庫檔案安裝至錯誤的目錄，以防止 zypper 尋找 .net Core 套件。 若要修正此問題，請在正確的目錄中建立符號連結。

```bash
sudo ln -s /etc/yum.repos.d/microsoft-prod.repo /etc/zypp/repos.d/microsoft-prod.repo
```

[!INCLUDE [linux-zyp-install-31](includes/linux-install-31-zyp.md)]

## <a name="sles-12-"></a>SLES 12 ✔️

.NET Core 需要使用 SP2 作為 SLES 12 系列的最小值。

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm
```

[!INCLUDE [linux-zyp-install-31](includes/linux-install-31-zyp.md)]

## <a name="troubleshoot-the-package-manager"></a>針對套件管理員進行疑難排解

本節提供使用封裝管理員安裝 .NET Core 時，可能會收到的常見錯誤資訊。

### <a name="failed-to-fetch"></a>無法提取

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="dependencies"></a>相依性

當您使用套件管理員進行安裝時，系統會為您安裝這些程式庫。 但是，如果您手動安裝 .NET Core 或發行獨立的應用程式，您必須確定已安裝這些程式庫：

- krb5
- libicu
- libopenssl1_1

如果目標執行時間環境的 OpenSSL 版本是1.1 或更新版本，您將需要安裝**相容性 compat-openssl10**。

如需相依性的詳細資訊，請參閱[獨立的 Linux 應用程式](https://github.com/dotnet/core/blob/master/Documentation/self-contained-linux-apps.md)。

針對使用*system.web*元件的 .net Core 應用程式，您也需要下列相依性：

- [libgdiplus （6.0.1 版或更新版本）](https://www.mono-project.com/docs/gui/libgdiplus/)

  > [!WARNING]
  > 您可以藉由將 Mono 存放庫新增至您的系統，來安裝最新版本的*libgdiplus* 。 如需詳細資訊，請參閱 <https://www.mono-project.com/download/stable/> 。

## <a name="scripted-install"></a>腳本式安裝

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>手動安裝

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>後續步驟

- [教學課程：使用 Visual Studio Code 建立具有 .NET Core SDK 的主控台應用程式](../tutorials/with-visual-studio-code.md)
