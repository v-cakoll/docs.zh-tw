---
ms.openlocfilehash: e92cbb7decb5e530bbf611cec26ce03a05e06eb3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622230"
---
### <a name="rsacng-and-dsacng-are-once-again-usable-in-partial-trust-scenarios"></a>RSACng 和 DSACng 再次可在部分信任案例中使用

#### <a name="details"></a>詳細資料

CngLightup (用於數個較高層級的加密 API，例如 <xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=nameWithType>) 和 <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType> 在某些情況下依賴完全信任。 這些包括沒有主張 <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode?displayProperty=nameWithType> 權限的 P/Invoke，以及 <xref:System.Security.Cryptography.CngKey?displayProperty=nameWithType> 具有 <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode?displayProperty=nameWithType> 之權限要求的程式碼路徑。 從 .NET Framework 4.6.2 開始，盡量使用 CngLightup 來切換至 <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType>。 因此，成功使用 <xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=nameWithType> 的部分信任應用程式開始失敗，並擲回 <xref:System.Security.SecurityException> 例外狀況。這項變更將新增所需的主張，讓所有使用 CngLightup 的函式具有必要權限。

#### <a name="suggestion"></a>建議

如果 .NET Framework 4.6.2 中的這項變更已對部分信任應用程式產生負面影響，請升級至 .NET Framework 4.7.1。

| 名稱    | 值       |
|:--------|:------------|
| 影響範圍   |Edge|
|版本|4.6.2|
|類型|執行階段

#### <a name="affected-apis"></a>受影響的 API

-<xref:System.Security.Cryptography.DSACng.%23ctor(System.Security.Cryptography.CngKey)></li><li><xref:System.Security.Cryptography.DSACng.Key?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.LegalKeySizes?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.CreateSignature(System.Byte[])?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.VerifySignature(System.Byte[],System.Byte[])?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.%23ctor(System.Security.Cryptography.CngKey)></li><li><xref:System.Security.Cryptography.RSACng.Key?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.Decrypt(System.Byte[],System.Security.Cryptography.RSAEncryptionPadding)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.SignHash(System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)?displayProperty=nameWithType></li></ul>|
