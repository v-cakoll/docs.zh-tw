---
title: dotnet new 命令
description: dotnet new 命令會根據指定的範本建立新的 .NET Core 專案。
no-loc:
- Blazor
- WebAssembly
ms.date: 04/10/2020
ms.openlocfilehash: ec41b3b79ed5eded7c9124d3e4d95c658ee39580
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86173116"
---
# <a name="dotnet-new"></a>dotnet new

**本文適用于：** ✔️ .net CORE 2.0 SDK 和更新版本

## <a name="name"></a>名稱

`dotnet new` - 根據指定的範本建立新的專案、組態檔或方案。

## <a name="synopsis"></a>概要

```dotnetcli
dotnet new <TEMPLATE> [--dry-run] [--force] [-i|--install {PATH|NUGET_ID}]
    [-lang|--language {"C#"|"F#"|VB}] [-n|--name <OUTPUT_NAME>]
    [--nuget-source <SOURCE>] [-o|--output <OUTPUT_DIRECTORY>]
    [-u|--uninstall] [--update-apply] [--update-check] [Template options]

dotnet new <TEMPLATE> [-l|--list] [--type <TYPE>]

dotnet new -h|--help
```

## <a name="description"></a>描述

`dotnet new`命令會根據範本建立 .Net Core 專案或其他成品。

命令會呼叫[範本引擎](https://github.com/dotnet/templating)，以根據指定的範本和選項在磁碟上建立成品。

### <a name="implicit-restore"></a>隱含還原

[!INCLUDE[dotnet restore note](~/includes/dotnet-restore-note.md)]

## <a name="arguments"></a>引數

- **`TEMPLATE`**

  要在叫用命令時具現化的範本。 每個範本可能會有您可以傳遞的特定選項。 如需詳細資訊，請參閱[範本選項](#template-options)。

  您可以執行 `dotnet new --list` 或 `dotnet new -l` 來查看所有已安裝的範本清單。 如果 `TEMPLATE` 值不完全符合所傳回資料表之 [樣板**Templates** ] 或 [**簡短名稱**] 資料行中的文字，則會在這兩個數據行上執行子字串相符。

  從 .NET Core 3.0 SDK 開始，當您 `dotnet new` 在下列情況下叫用命令時，CLI 會在 NuGet.org 中搜尋範本：

  - 如果 CLI 在叫用時找不到相符的範本 `dotnet new` ，甚至不是部分。
  - 如果有較新版本的範本可用，則為。 在此情況下，會建立專案或成品，但 CLI 會警告您有關範本的更新版本。

  下表顯示隨 .NET Core SDK 預先安裝的範本。 範本的預設語言會顯示在方括號內。 按一下 [簡短名稱] 連結，以查看特定的範本選項。

| 範本                                    | 簡短名稱                      | 語言     | Tags                                  | 引導 |
|----------------------------------------------|---------------------------------|--------------|---------------------------------------|------------|
| 主控台應用程式                          | [控制](#console)             | [C#], F#, VB | 通用/主控台                        | 1.0        |
| 類別庫                                | [classlib](#classlib)           | [C#], F#, VB | 通用/程式庫                        | 1.0        |
| WPF 應用程式                              | [wpf](#wpf)                     | [C#]         | Common/WPF                            | 3.0        |
| WPF 類別庫                            | [wpflib](#wpf)                  | [C#]         | Common/WPF                            | 3.0        |
| WPF 自訂控制項程式庫                   | [wpfcustomcontrollib](#wpf)     | [C#]         | Common/WPF                            | 3.0        |
| WPF 使用者控制項程式庫                     | [wpfusercontrollib](#wpf)       | [C#]         | Common/WPF                            | 3.0        |
| Windows Forms (WinForms) 應用程式         | [winforms](#winforms)           | [C#]         | Common/WinForms                       | 3.0        |
| Windows Forms (WinForms) 類別庫       | [winformslib](#winforms)        | [C#]         | Common/WinForms                       | 3.0        |
| 背景工作服務                               | [工作](#web-others)           | [C#]         | 一般/背景工作/Web                     | 3.0        |
| 單元測試專案                            | [mstest.exe](#test)                 | [C#], F#, VB | 測試/MSTest                           | 1.0        |
| NUnit 3 測試專案                         | [nunit](#nunit)                  | [C#], F#, VB | 測試/NUnit                            | 2.1.400    |
| NUnit 3 測試項目                            | `nunit-test`                    | [C#], F#, VB | 測試/NUnit                            | 2.2        |
| xUnit 測試專案                           | [xunit](#test)                  | [C#], F#, VB | 測試/XUnit                            | 1.0        |
| Razor 元件                              | `razorcomponent`                | [C#]         | Web/ASP.NET                           | 3.0        |
| Razor 頁面                                   | [本頁](#page)                   | [C#]         | Web/ASP.NET                           | 2.0        |
| MVC ViewImports                              | [viewimports](#namespace)       | [C#]         | Web/ASP.NET                           | 2.0        |
| MVC ViewStart                                | `viewstart`                     | [C#]         | Web/ASP.NET                           | 2.0        |
| Blazor伺服器應用程式                            | [blazorserver](#blazorserver)   | [C#]         | 網路Blazor                            | 3.0        |
| BlazorWebAssembly應用程式                       | `blazorwasm`                    | [C#]         | 網路Blazor/WebAssembly                            | 3.1.300    |
| 空的 ASP.NET Core                           | [網路](#web)                     | [C#]、F#     | Web/空白                             | 1.0        |
| ASP.NET Core Web 應用程式 (模型檢視控制器) | [mvc](#web-options)             | [C#]、F#     | Web/MVC                               | 1.0        |
| ASP.NET Core Web 應用程式                         | [webapp，razor](#web-options)   | [C#]         | Web/MVC/Razor 頁面                   | 2.2、2。0   |
| ASP.NET Core 與 Angular                    | [angular](#spa)                 | [C#]         | Web/MVC/SPA                           | 2.0        |
| ASP.NET Core 與 React.js                   | [反應](#spa)                   | [C#]         | Web/MVC/SPA                           | 2.0        |
| ASP.NET Core 與 React.js 和 Redux         | [reactredux](#reactredux)       | [C#]         | Web/MVC/SPA                           | 2.0        |
| Razor 類別庫                          | [razorclasslib](#razorclasslib) | [C#]         | Web/Razor/程式庫/Razor 類別庫 | 2.1        |
| ASP.NET Core Web API                         | [webapi](#webapi)               | [C#]、F#     | Web/WebAPI                            | 1.0        |
| ASP.NET Core gRPC 服務                    | [grpc](#web-others)             | [C#]         | Web/gRPC                              | 3.0        |
| dotnet .gitignore 檔                        | `gitignore`                     |              | Config                                | 3.0        |
| global.json 檔案                             | [globaljson](#globaljson)       |              | Config                                | 2.0        |
| NuGet 組態                                 | `nugetconfig`                   |              | Config                                | 1.0        |
| Dotnet 本機工具資訊清單檔              | `tool-manifest`                 |              | Config                                | 3.0        |
| Web 組態                                   | `webconfig`                     |              | Config                                | 1.0        |
| 方案檔                                | `sln`                           |              | 解決方案                              | 1.0        |
| 通訊協定緩衝區檔案                         | [proto](#namespace)             |              | Web/gRPC                              | 3.0        |

## <a name="options"></a>選項

- **`--dry-run`**

  若指定的命令會導致建立範本，則顯示執行時會發生的情況摘要。 自 .NET Core 2.2 SDK 起提供。

- **`--force`**

  強制產生內容，即使它會變更現有的檔案。 當選擇的範本會覆寫輸出目錄中的現有檔案時，這是必要的。

- **`-h|--help`**

  印出命令的說明。 您可以針對 `dotnet new` 命令本身或任何範本叫用它。 例如，`dotnet new mvc --help`。

- **`-i|--install <PATH|NUGET_ID>`**

  從或提供的安裝範本 `PATH` 套件 `NUGET_ID` 。 若您想要安裝預先發行版本的範本套件，就必須以 `<package-name>::<package-version>` 的格式指定版本。 根據預設，會 `dotnet new` 傳遞 \* 代表最新穩定封裝版本的版本。 請參閱[範例](#examples)一節中的範例。
  
  當您執行此命令時，如果已安裝某個版本的範本，此範本將會更新為指定的版本，或如果未指定任何版本，則為最新的穩定版本。

  如需建立自訂範本的資訊，請參閱 [dotnet new的自訂範本](custom-templates.md)。

- **`-l|--list`**

  列出包含指定名稱的範本。 如果未指定名稱，則會列出所有範本。

- **`-lang|--language {C#|F#|VB}`**

  要建立的範本語言。 接受的語言會因範本而有所不同 (請參閱[引數](#arguments)一節中的預設值)。 並非所有範本都適用。

  > [!NOTE]
  > 某些 Shell 會將 `#` 解譯為特殊字元。 在這些情況下，請以引號括住語言參數值。 例如，`dotnet new console -lang "F#"`。

- **`-n|--name <OUTPUT_NAME>`**

  所建立輸出的名稱。 如果未指定名稱，則會使用目前目錄的名稱。

- **`--nuget-source <SOURCE>`**

  請指定安裝期間所要使用的 NuGet 來源。 自 .NET Core 2.1 SDK 起提供。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  放置所產生輸出的位置。 預設值是目前的目錄。

- **`--type <TYPE>`**

  根據可用的類型篩選範本。 預先定義的值為 `project` 、 `item` 和 `other` 。

- **`-u|--uninstall [PATH|NUGET_ID]`**

  在或提供的上卸載範本套件 `PATH` `NUGET_ID` 。 `<PATH|NUGET_ID>`未指定值時，會顯示所有目前已安裝的範本套件及其關聯的範本。 指定時 `NUGET_ID` ，請勿包含版本號碼。

  如果您未指定此選項的參數，此命令會列出已安裝的範本和其相關詳細資料。

  > [!NOTE]
  > 若要使用 `PATH` 將範本解除安裝，您需要使路徑成為完整路徑。 例如， *C：/Users/ \<USER> /Documents/Templates/GarciaSoftware.ConsoleTemplate.CSharp*可以使用，但包含資料夾中的 *./garciasoftware.consoletemplate.csharp*則不會。
  > 請勿在您的範本路徑上包含最後終止的目錄斜線。

- **`--update-apply`**

  檢查目前已安裝的範本套件是否有可用的更新，並加以安裝。 自 .NET Core 3.0 SDK 起提供。

- **`--update-check`**

  檢查目前是否已安裝的範本套件是否有可用的更新。 自 .NET Core 3.0 SDK 起提供。

## <a name="template-options"></a>範本選項

每個專案範本都可能會有其他可用的選項。 核心範本有下列額外選項：

### <a name="console"></a>console

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 自 .NET Core 3.0 SDK 起提供。

  下表根據您所使用的 SDK 版本號碼，列出預設值：

  | SDK 版本 | 預設值   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`--langVersion <VERSION_NUMBER>`**

  `LangVersion`在建立的專案檔中設定屬性。 例如，使用 `--langVersion 7.3` 可使用 C# 7.3。 不支援 F#。 自 .NET Core 2.2 SDK 起提供。

  如需預設 c # 版本的清單，請參閱[預設值](../../csharp/language-reference/configure-language-version.md#defaults)。

- **`--no-restore`**

  若已指定，則不會在專案建立期間執行隱含還原。 自 .NET Core 2.2 SDK 起提供。

***

### <a name="classlib"></a>classlib

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 值：`netcoreapp<version>` 建立 .NET Core 類別庫或 `netstandard<version>` 建立 .NET Standard 類別庫。 預設值是 `netstandard2.0`。

- **`--langVersion <VERSION_NUMBER>`**

  `LangVersion`在建立的專案檔中設定屬性。 例如，使用 `--langVersion 7.3` 可使用 C# 7.3。 不支援 F#。 自 .NET Core 2.2 SDK 起提供。

  如需預設 c # 版本的清單，請參閱[預設值](../../csharp/language-reference/configure-language-version.md#defaults)。

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

***

### <a name="wpf-wpflib-wpfcustomcontrollib-wpfusercontrollib"></a><a name="wpf"></a>wpf、wpflib、wpfcustomcontrollib、wpfusercontrollib

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 預設值是 `netcoreapp3.1`。 自 .NET Core 3.1 SDK 起提供。

- **`--langVersion <VERSION_NUMBER>`**

  `LangVersion`在建立的專案檔中設定屬性。 例如，使用 `--langVersion 7.3` 可使用 C# 7.3。

  如需預設 c # 版本的清單，請參閱[預設值](../../csharp/language-reference/configure-language-version.md#defaults)。

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

***

### <a name="winforms-winformslib"></a><a name="winforms"></a>winforms、winformslib

- **`--langVersion <VERSION_NUMBER>`**

  `LangVersion`在建立的專案檔中設定屬性。 例如，使用 `--langVersion 7.3` 可使用 C# 7.3。

  如需預設 c # 版本的清單，請參閱[預設值](../../csharp/language-reference/configure-language-version.md#defaults)。

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

***

### <a name="worker-grpc"></a><a name="web-others"></a>worker、grpc

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 預設值是 `netcoreapp3.1`。 自 .NET Core 3.1 SDK 起提供。

- **`--exclude-launch-settings`**

  從產生的範本中排除*launchSettings.js* 。

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

***

### <a name="mstest-xunit"></a><a name="test"></a>mstest、xunit

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 自 .NET Core 3.0 SDK 起可用的選項。

  下表根據您所使用的 SDK 版本號碼，列出預設值：

  | SDK 版本 | 預設值   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`-p|--enable-pack`**

  使用[dotnet pack](dotnet-pack.md)啟用專案的封裝。

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

***

### <a name="nunit"></a>nunit

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。

  下表根據您所使用的 SDK 版本號碼，列出預設值：

  | SDK 版本 | 預設值   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.2         | `netcoreapp2.2` |
  | 2.1         | `netcoreapp2.1` |

- **`-p|--enable-pack`**

  使用[dotnet pack](dotnet-pack.md)啟用專案的封裝。

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

***

### <a name="page"></a>頁面

- **`-na|--namespace <NAMESPACE_NAME>`**

  產生之程式碼的命名空間。 預設值是 `MyApp.Namespace`。

- **`-np|--no-pagemodel`**

  建立不含 PageModel 的頁面。

***

### <a name="viewimports-proto"></a><a name="namespace"></a>viewimports.cshtml，proto

- **`-na|--namespace <NAMESPACE_NAME>`**

  產生之程式碼的命名空間。 預設值是 `MyApp.Namespace`。

***

### <a name="blazorserver"></a>blazorserver

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的驗證類型。 可能的值包括：

  - `None` - 無驗證 (預設值)。
  - `Individual` - 個別驗證。
  - `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
  - `SingleOrg` - 單一租用戶的組織驗證。
  - `MultiOrg` - 多個租用戶的組織驗證。
  - `Windows` - Windows 驗證。

- **`--aad-b2c-instance <INSTANCE>`**

  要連接的 Azure Active Directory B2C 實例。 搭配 `IndividualB2C` 驗證使用。 預設值是 `https://login.microsoftonline.com/tfp/`。

- **`-ssp|--susi-policy-id <ID>`**

  此專案的登入和註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

- **`-rp|--reset-password-policy-id <ID>`**

  此專案的重設密碼原則識別碼。 搭配 `IndividualB2C` 驗證使用。

- **`-ep|--edit-profile-policy-id <ID>`**

  此專案的編輯設定檔原則識別碼。 搭配 `IndividualB2C` 驗證使用。

- **`--aad-instance <INSTANCE>`**

  要連接的 Azure Active Directory 實例。 搭配 `SingleOrg` 或 `MultiOrg` 驗證使用。 預設值是 `https://login.microsoftonline.com/`。

- **`--client-id <ID>`**

  此專案的用戶端識別碼。 搭配 `IndividualB2C`、`SingleOrg` 或 `MultiOrg` 驗證使用。 預設值是 `11111111-1111-1111-11111111111111111`。

- **`--domain <DOMAIN>`**

  目錄租使用者的網域。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值是 `qualified.domain.name`。

- **`--tenant-id <ID>`**

  要連接之目錄的 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值是 `22222222-2222-2222-2222-222222222222`。

- **`--callback-path <PATH>`**

  應用程式的重新導向 URI 基底路徑中的要求路徑。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值是 `/signin-oidc`。

- **`-r|--org-read-access`**

  允許此應用程式讀取目錄的許可權。 僅適用於 `SingleOrg` 或 `MultiOrg` 驗證。

- **`--exclude-launch-settings`**

  從產生的範本中排除*launchSettings.js* 。

- **`--no-https`**

  關閉 HTTPS。 只有當 `Individual` 、 `IndividualB2C` 、 `SingleOrg` 或不是用於時 `MultiOrg` ， `--auth` 才會套用此選項。

- **`-uld|--use-local-db`**

  指定應該使用 LocalDB，而不是 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

***

### <a name="web"></a>Web

- **`--exclude-launch-settings`**

  從產生的範本中排除*launchSettings.js* 。

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 無法在 .NET Core 2.2 SDK 中使用選項。

  下表根據您所使用的 SDK 版本號碼，列出預設值：

  | SDK 版本 | 預設值   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.1` |

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

- **`--no-https`**

  關閉 HTTPS。

***

### <a name="mvc-webapp"></a><a name="web-options"></a>mvc、webapp

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的驗證類型。 可能的值包括：

  - `None` - 無驗證 (預設值)。
  - `Individual` - 個別驗證。
  - `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
  - `SingleOrg` - 單一租用戶的組織驗證。
  - `MultiOrg` - 多個租用戶的組織驗證。
  - `Windows` - Windows 驗證。

- **`--aad-b2c-instance <INSTANCE>`**

  要連接的 Azure Active Directory B2C 實例。 搭配 `IndividualB2C` 驗證使用。 預設值是 `https://login.microsoftonline.com/tfp/`。

- **`-ssp|--susi-policy-id <ID>`**

  此專案的登入和註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

- **`-rp|--reset-password-policy-id <ID>`**

  此專案的重設密碼原則識別碼。 搭配 `IndividualB2C` 驗證使用。

- **`-ep|--edit-profile-policy-id <ID>`**

  此專案的編輯設定檔原則識別碼。 搭配 `IndividualB2C` 驗證使用。

- **`--aad-instance <INSTANCE>`**

  要連接的 Azure Active Directory 實例。 搭配 `SingleOrg` 或 `MultiOrg` 驗證使用。 預設值是 `https://login.microsoftonline.com/`。

- **`--client-id <ID>`**

  此專案的用戶端識別碼。 搭配 `IndividualB2C`、`SingleOrg` 或 `MultiOrg` 驗證使用。 預設值是 `11111111-1111-1111-11111111111111111`。

- **`--domain <DOMAIN>`**

  目錄租使用者的網域。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值是 `qualified.domain.name`。

- **`--tenant-id <ID>`**

  要連接之目錄的 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值是 `22222222-2222-2222-2222-222222222222`。

- **`--callback-path <PATH>`**

  應用程式的重新導向 URI 基底路徑中的要求路徑。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值是 `/signin-oidc`。

- **`-r|--org-read-access`**

  允許此應用程式讀取目錄的許可權。 僅適用於 `SingleOrg` 或 `MultiOrg` 驗證。

- **`--exclude-launch-settings`**

  從產生的範本中排除*launchSettings.js* 。

- **`--no-https`**

  關閉 HTTPS。 此選項僅適用於未使用 `Individual`、`IndividualB2C`、`SingleOrg` 或 `MultiOrg` 時。

- **`-uld|--use-local-db`**

  指定應該使用 LocalDB，而不是 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 自 .NET Core 3.0 SDK 起可用的選項。

  下表根據您所使用的 SDK 版本號碼，列出預設值：

  | SDK 版本 | 預設值   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

- **`--use-browserlink`**

  在專案中包含 BrowserLink。 無法在 .NET Core 2.2 和 3.1 SDK 中使用選項。

- **`-rrc|--razor-runtime-compilation`**

  判斷專案是否設定為使用 Debug 組建中的[Razor 執行時間編譯](/aspnet/core/mvc/views/view-compilation#runtime-compilation)。 自 .NET Core 3.1.201 SDK 起可用的選項。

***

### <a name="angular-react"></a><a name="spa"></a>角度、反應

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的驗證類型。 自 .NET Core 3.0 SDK 起提供。
  
  可能的值包括：

  - `None` - 無驗證 (預設值)。
  - `Individual` - 個別驗證。

- **`--exclude-launch-settings`**

  從產生的範本中排除*launchSettings.js* 。

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

- **`--no-https`**

  關閉 HTTPS。 只有在驗證為時，才會套用此選項 `None` 。

- **`-uld|--use-local-db`**

  指定應該使用 LocalDB，而不是 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。 自 .NET Core 3.0 SDK 起提供。

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 無法在 .NET Core 2.2 SDK 中使用選項。

  下表根據您所使用的 SDK 版本號碼，列出預設值：

  | SDK 版本 | 預設值   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.0` |

***

### <a name="reactredux"></a>reactredux

- **`--exclude-launch-settings`**

  從產生的範本中排除*launchSettings.js* 。

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 無法在 .NET Core 2.2 SDK 中使用選項。

  下表根據您所使用的 SDK 版本號碼，列出預設值：

  | SDK 版本 | 預設值   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.0` |

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

- **`--no-https`**

  關閉 HTTPS。

***

### <a name="razorclasslib"></a>razorclasslib

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

- **`-s|--support-pages-and-views`**

  支援將傳統的 Razor 頁面和 Views 新增至此程式庫的元件。 自 .NET Core 3.0 SDK 起提供。

***
  
### <a name="webapi"></a>webapi

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的驗證類型。 可能的值包括：

  - `None` - 無驗證 (預設值)。
  - `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
  - `SingleOrg` - 單一租用戶的組織驗證。
  - `Windows` - Windows 驗證。

- **`--aad-b2c-instance <INSTANCE>`**

  要連接的 Azure Active Directory B2C 實例。 搭配 `IndividualB2C` 驗證使用。 預設值是 `https://login.microsoftonline.com/tfp/`。

- **`-ssp|--susi-policy-id <ID>`**

  此專案的登入和註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

- **`--aad-instance <INSTANCE>`**

  要連接的 Azure Active Directory 實例。 搭配 `SingleOrg` 驗證使用。 預設值是 `https://login.microsoftonline.com/`。

- **`--client-id <ID>`**

  此專案的用戶端識別碼。 搭配 `IndividualB2C` 或 `SingleOrg` 驗證使用。 預設值是 `11111111-1111-1111-11111111111111111`。

- **`--domain <DOMAIN>`**

  目錄租使用者的網域。 搭配 `IndividualB2C` 或 `SingleOrg` 驗證使用。 預設值是 `qualified.domain.name`。

- **`--tenant-id <ID>`**

  要連接之目錄的 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值是 `22222222-2222-2222-2222-222222222222`。

- **`-r|--org-read-access`**

  允許此應用程式讀取目錄的許可權。 僅適用于 `SingleOrg` 驗證。

- **`--exclude-launch-settings`**

  從產生的範本中排除*launchSettings.js* 。

- **`--no-https`**

  關閉 HTTPS。 `app.UseHsts` 和 `app.UseHttpsRedirection` 並未新增至 `Startup.Configure`。 只有當 `IndividualB2C` 或未用於驗證時，此選項才適用 `SingleOrg` 。

- **`-uld|--use-local-db`**

  指定應該使用 LocalDB，而不是 SQLite。 僅適用于 `IndividualB2C` 驗證。

- **`-f|--framework <FRAMEWORK>`**

  指定要設為目標的[架構](../../standard/frameworks.md)。 無法在 .NET Core 2.2 SDK 中使用選項。

  下表根據您所使用的 SDK 版本號碼，列出預設值：

  | SDK 版本 | 預設值   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.1` |

- **`--no-restore`**

  在專案建立期間不會執行隱含還原。

***

### <a name="globaljson"></a>globaljson

- **`--sdk-version <VERSION_NUMBER>`**

  指定要在檔案*global.js*中使用的 .NET Core SDK 版本。

***

## <a name="examples"></a>範例

- 藉由指定範本名稱，建立 C# 主控台應用程式專案：

  ```dotnetcli
  dotnet new "Console Application"
  ```

- 在目前的目錄中建立 F# 主控台應用程式專案：

  ```dotnetcli
  dotnet new console -lang "F#"
  ```

- 在指定的目錄中建立 .NET Standard 類別庫專案：

  ```dotnetcli
  dotnet new classlib -lang VB -o MyLibrary
  ```

- 未經驗證即在目前目錄中建立新的 ASP.NET Core C# MVC 專案：

  ```dotnetcli
  dotnet new mvc -au None
  ```

- 建立新的 xUnit 專案：

  ```dotnetcli
  dotnet new xunit
  ```

- 列出適用于單一頁面應用程式的所有範本 (SPA) 範本：

  ```dotnetcli
  dotnet new spa -l
  ```

- 列出所有符合 *we* 子字串的範本。 找不到完全相符的項目，因此會對 [簡短名稱] 和 [名稱] 兩欄執行子字串比對作業。

  ```dotnetcli
  dotnet new we -l
  ```

- 嘗試叫用符合 *ng* 的範本。 如果無法判別單一相符項目，則列出部分相符的範本。

  ```dotnetcli
  dotnet new ng
  ```

- 安裝適用于 ASP.NET Core 的 SPA 範本2.0 版：

  ```dotnetcli
  dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
  ```

- 列出已安裝的範本和其詳細資料，包括如何將它們卸載：

  ```dotnetcli
  dotnet new -u
  ```

- 在目前目錄中建立*global.js* ，將 SDK 版本設定為3.1.101：

  ```dotnetcli
  dotnet new globaljson --sdk-version 3.1.101
  ```

## <a name="see-also"></a>另請參閱

- [dotnet new 的自訂範本](custom-templates.md)
- [建立適用於 dotnet new 的自訂範本](../tutorials/cli-templates-create-item-template.md)
- [dotnet/dotnet-template-samples GitHub repo](https://github.com/dotnet/dotnet-template-samples) (dotnet/dotnet-template-samples GitHub 存放庫)
- [dotnet new 的可用範本](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new)
