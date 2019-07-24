---
title: NuGet CLI 签名命令
description: 针对 nuget.exe sign 命令的参考
author: dtivel
ms.author: dtivel
ms.date: 03/06/2018
ms.topic: reference
ms.reviewer: rmpablos
ms.openlocfilehash: e941a9f34058f5ebed13a8f68c8cfa23ba5fb6d1
ms.sourcegitcommit: efc18d484fdf0c7a8979b564dcb191c030601bb4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/18/2019
ms.locfileid: "68327584"
---
# <a name="sign-command-nuget-cli"></a><span data-ttu-id="46e93-103">sign 命令 (NuGet CLI)</span><span class="sxs-lookup"><span data-stu-id="46e93-103">sign command (NuGet CLI)</span></span>

<span data-ttu-id="46e93-104">**适用于:** 创建&bullet;包的**支持版本:** 4.6 +</span><span class="sxs-lookup"><span data-stu-id="46e93-104">**Applies to:** package creation &bullet; **Supported versions:** 4.6+</span></span>

<span data-ttu-id="46e93-105">使用证书对匹配第一个参数的所有包进行签名。</span><span class="sxs-lookup"><span data-stu-id="46e93-105">Signs all the packages matching the first argument with a certificate.</span></span> <span data-ttu-id="46e93-106">可以通过提供使用者名称或指纹, 从文件或证书存储中安装的证书获取带有私钥的证书。</span><span class="sxs-lookup"><span data-stu-id="46e93-106">The certificate with the private key can be obtained from a file or from a certificate installed in a certificate store by providing a subject name or a thumbprint.</span></span>

<span data-ttu-id="46e93-107">.NET Core、Mono 或非 Windows 平台上尚不支持包签名。</span><span class="sxs-lookup"><span data-stu-id="46e93-107">Package signing is not yet supported in .NET Core, under Mono, or on non-Windows platforms.</span></span>

## <a name="usage"></a><span data-ttu-id="46e93-108">用法</span><span class="sxs-lookup"><span data-stu-id="46e93-108">Usage</span></span>

```cli
nuget sign <package(s)> [options]
```

<span data-ttu-id="46e93-109">其中`<package(s)>` , 是一个或`.nupkg`多个文件。</span><span class="sxs-lookup"><span data-stu-id="46e93-109">where `<package(s)>` is one or more `.nupkg` files.</span></span>

## <a name="options"></a><span data-ttu-id="46e93-110">选项</span><span class="sxs-lookup"><span data-stu-id="46e93-110">Options</span></span>

| <span data-ttu-id="46e93-111">选项</span><span class="sxs-lookup"><span data-stu-id="46e93-111">Option</span></span> | <span data-ttu-id="46e93-112">描述</span><span class="sxs-lookup"><span data-stu-id="46e93-112">Description</span></span> |
| --- | --- |
| <span data-ttu-id="46e93-113">CertificateFingerprint</span><span class="sxs-lookup"><span data-stu-id="46e93-113">CertificateFingerprint</span></span> | <span data-ttu-id="46e93-114">指定用于搜索证书的本地证书存储区的证书的 SHA-1 指纹。</span><span class="sxs-lookup"><span data-stu-id="46e93-114">Specifies the SHA-1 fingerprint of the certificate used to search a local certificate store for the certificate.</span></span> |
| <span data-ttu-id="46e93-115">CertificatePassword</span><span class="sxs-lookup"><span data-stu-id="46e93-115">CertificatePassword</span></span> | <span data-ttu-id="46e93-116">如果需要, 指定证书密码。</span><span class="sxs-lookup"><span data-stu-id="46e93-116">Specifies the certificate password, if needed.</span></span> <span data-ttu-id="46e93-117">如果证书受密码保护, 但未提供密码, 则该命令将在运行时提示输入密码, 除非传递了-非交互式选项。</span><span class="sxs-lookup"><span data-stu-id="46e93-117">If a certificate is password protected but no password is provided, the command will prompt for a password at run time, unless the -NonInteractive option is passed.</span></span> |
| <span data-ttu-id="46e93-118">CertificatePath</span><span class="sxs-lookup"><span data-stu-id="46e93-118">CertificatePath</span></span> | <span data-ttu-id="46e93-119">指定用于对包进行签名的证书的文件路径。</span><span class="sxs-lookup"><span data-stu-id="46e93-119">Specifies the file path to the certificate to be used in signing the package.</span></span> |
| <span data-ttu-id="46e93-120">CertificateStoreLocation</span><span class="sxs-lookup"><span data-stu-id="46e93-120">CertificateStoreLocation</span></span> | <span data-ttu-id="46e93-121">指定用于搜索证书的 x.509 证书存储区的名称。</span><span class="sxs-lookup"><span data-stu-id="46e93-121">Specifies the name of the X.509 certificate store use to search for the certificate.</span></span> <span data-ttu-id="46e93-122">默认为 "CurrentUser", 当前用户使用的 x.509 证书存储。</span><span class="sxs-lookup"><span data-stu-id="46e93-122">Defaults to "CurrentUser", the X.509 certificate store used by the current user.</span></span> <span data-ttu-id="46e93-123">当通过-CertificateSubjectName 或-CertificateFingerprint 选项指定证书时, 应使用此选项。</span><span class="sxs-lookup"><span data-stu-id="46e93-123">This option should be used when specifying the certificate via -CertificateSubjectName or -CertificateFingerprint options.</span></span> |
| <span data-ttu-id="46e93-124">CertificateStoreName</span><span class="sxs-lookup"><span data-stu-id="46e93-124">CertificateStoreName</span></span> | <span data-ttu-id="46e93-125">指定用于搜索证书的 x.509 证书存储区的名称。</span><span class="sxs-lookup"><span data-stu-id="46e93-125">Specifies the name of the X.509 certificate store to use to search for the certificate.</span></span> <span data-ttu-id="46e93-126">默认值为 "My", 适用于个人证书的 x.509 证书存储。</span><span class="sxs-lookup"><span data-stu-id="46e93-126">Defaults to "My", the X.509 certificate store for personal certificates.</span></span> <span data-ttu-id="46e93-127">当通过-CertificateSubjectName 或-CertificateFingerprint 选项指定证书时, 应使用此选项。</span><span class="sxs-lookup"><span data-stu-id="46e93-127">This option should be used when specifying the certificate via -CertificateSubjectName or -CertificateFingerprint options.</span></span> |
| <span data-ttu-id="46e93-128">CertificateSubjectName</span><span class="sxs-lookup"><span data-stu-id="46e93-128">CertificateSubjectName</span></span> | <span data-ttu-id="46e93-129">指定用于在本地证书存储区中搜索证书的证书的使用者名称。</span><span class="sxs-lookup"><span data-stu-id="46e93-129">Specifies the subject name of the certificate used to search a local certificate store for the certificate.</span></span>  <span data-ttu-id="46e93-130">搜索是使用提供的值进行区分大小写的字符串比较, 它将查找使用者名称包含该字符串的所有证书, 而与其他使用者值无关。</span><span class="sxs-lookup"><span data-stu-id="46e93-130">The search is a case-insensitive string comparison using the supplied value, which will find all certificates with the subject name containing that string, regardless of other subject values.</span></span>  <span data-ttu-id="46e93-131">可以通过-CertificateStoreName 和-CertificateStoreLocation 选项指定证书存储区。</span><span class="sxs-lookup"><span data-stu-id="46e93-131">The certificate store can be specified by -CertificateStoreName and -CertificateStoreLocation options.</span></span> |
| <span data-ttu-id="46e93-132">ConfigFile</span><span class="sxs-lookup"><span data-stu-id="46e93-132">ConfigFile</span></span> | <span data-ttu-id="46e93-133">要应用的 NuGet 配置文件。</span><span class="sxs-lookup"><span data-stu-id="46e93-133">The NuGet configuration file to apply.</span></span> <span data-ttu-id="46e93-134">如果未指定, `%AppData%\NuGet\NuGet.Config`则使用 (Windows `~/.nuget/NuGet/NuGet.Config` ) 或 (Mac/Linux)。</span><span class="sxs-lookup"><span data-stu-id="46e93-134">If not specified, `%AppData%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux) is used.</span></span>|
| <span data-ttu-id="46e93-135">ForceEnglishOutput</span><span class="sxs-lookup"><span data-stu-id="46e93-135">ForceEnglishOutput</span></span> | <span data-ttu-id="46e93-136">使用固定的、基于英语的区域性强制执行 nuget.exe。</span><span class="sxs-lookup"><span data-stu-id="46e93-136">Forces nuget.exe to run using an invariant, English-based culture.</span></span> |
| <span data-ttu-id="46e93-137">HashAlgorithm</span><span class="sxs-lookup"><span data-stu-id="46e93-137">HashAlgorithm</span></span> | <span data-ttu-id="46e93-138">用于对包进行签名的哈希算法。</span><span class="sxs-lookup"><span data-stu-id="46e93-138">Hash algorithm to be used to sign the package.</span></span> <span data-ttu-id="46e93-139">默认值为 SHA256。</span><span class="sxs-lookup"><span data-stu-id="46e93-139">Defaults to SHA256.</span></span> |
| <span data-ttu-id="46e93-140">Help</span><span class="sxs-lookup"><span data-stu-id="46e93-140">Help</span></span> | <span data-ttu-id="46e93-141">显示命令的帮助信息。</span><span class="sxs-lookup"><span data-stu-id="46e93-141">Displays help information for the command.</span></span> |
| <span data-ttu-id="46e93-142">NonInteractive</span><span class="sxs-lookup"><span data-stu-id="46e93-142">NonInteractive</span></span> | <span data-ttu-id="46e93-143">取消显示提示用户输入或确认。</span><span class="sxs-lookup"><span data-stu-id="46e93-143">Suppresses prompts for user input or confirmations.</span></span> |
| <span data-ttu-id="46e93-144">OutputDirectory</span><span class="sxs-lookup"><span data-stu-id="46e93-144">OutputDirectory</span></span> | <span data-ttu-id="46e93-145">指定应将已签名的包保存到的目录。</span><span class="sxs-lookup"><span data-stu-id="46e93-145">Specifies the directory where the signed package should be saved.</span></span> <span data-ttu-id="46e93-146">默认情况下, 已签名的包将覆盖原始包。</span><span class="sxs-lookup"><span data-stu-id="46e93-146">By default the original package is overwritten by the signed package.</span></span> |
| <span data-ttu-id="46e93-147">Overwrite</span><span class="sxs-lookup"><span data-stu-id="46e93-147">Overwrite</span></span> | <span data-ttu-id="46e93-148">切换以指示是否应覆盖当前签名。</span><span class="sxs-lookup"><span data-stu-id="46e93-148">Switch to indicate if the current signature should be overwritten.</span></span> <span data-ttu-id="46e93-149">默认情况下, 如果包已有签名, 则该命令将失败。</span><span class="sxs-lookup"><span data-stu-id="46e93-149">By default the command will fail if the package already has a signature.</span></span> |
| <span data-ttu-id="46e93-150">Timestamper</span><span class="sxs-lookup"><span data-stu-id="46e93-150">Timestamper</span></span> | <span data-ttu-id="46e93-151">RFC 3161 时间戳服务器的 URL。</span><span class="sxs-lookup"><span data-stu-id="46e93-151">URL to an RFC 3161 timestamping server.</span></span> |
| <span data-ttu-id="46e93-152">TimestampHashAlgorithm</span><span class="sxs-lookup"><span data-stu-id="46e93-152">TimestampHashAlgorithm</span></span> | <span data-ttu-id="46e93-153">RFC 3161 时间戳服务器使用的哈希算法。</span><span class="sxs-lookup"><span data-stu-id="46e93-153">Hash algorithm to be used by the RFC 3161 timestamp server.</span></span> <span data-ttu-id="46e93-154">默认值为 SHA256。</span><span class="sxs-lookup"><span data-stu-id="46e93-154">Defaults to SHA256.</span></span> |
| <span data-ttu-id="46e93-155">Verbosity</span><span class="sxs-lookup"><span data-stu-id="46e93-155">Verbosity</span></span> | <span data-ttu-id="46e93-156">指定在输出中显示的详细信息量: "*正常*"、"*静默*"、"*详细*"。</span><span class="sxs-lookup"><span data-stu-id="46e93-156">Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*.</span></span> |

## <a name="examples"></a><span data-ttu-id="46e93-157">示例</span><span class="sxs-lookup"><span data-stu-id="46e93-157">Examples</span></span>

```cli
nuget sign MyPackage.nupkg -Timestamper http://timestamp.test

nuget sign .\..\MyPackage.nupkg -Timestamper http://timestamp.test -OutputDirectory .\..\Signed
```