---
title: "NuGet CLI 验证命令 |Microsoft 文档"
author: dtivel
ms.author: dtivel
manager: doronm
ms.date: 03/06/2018
ms.topic: reference
ms.prod: nuget
ms.technology: 
description: "参考 nuget.exe 验证命令"
keywords: "nuget 验证引用，验证命令"
ms.reviewer:
- karann
- rmpablos
ms.openlocfilehash: 096c79670267d9b602dd6ad30640e832441c31c5
ms.sourcegitcommit: 74c21b406302288c158e8ae26057132b12960be8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="verify-command-nuget-cli"></a><span data-ttu-id="3cb5e-104">验证命令 (NuGet CLI)</span><span class="sxs-lookup"><span data-stu-id="3cb5e-104">verify command (NuGet CLI)</span></span>

<span data-ttu-id="3cb5e-105">**适用于：**打包消耗&bullet;**受支持的版本：** 4.6 +</span><span class="sxs-lookup"><span data-stu-id="3cb5e-105">**Applies to:** package consumption &bullet; **Supported versions:** 4.6+</span></span>

<span data-ttu-id="3cb5e-106">验证包。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-106">Verifies a package.</span></span>

## <a name="usage"></a><span data-ttu-id="3cb5e-107">用法</span><span class="sxs-lookup"><span data-stu-id="3cb5e-107">Usage</span></span>

```cli
nuget verify <package(s)> [options]
```

<span data-ttu-id="3cb5e-108">其中`<package(s)>`一个或多个`.nupkg`文件。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-108">where `<package(s)>` is one or more `.nupkg` files.</span></span>

## <a name="options"></a><span data-ttu-id="3cb5e-109">选项</span><span class="sxs-lookup"><span data-stu-id="3cb5e-109">Options</span></span>

| <span data-ttu-id="3cb5e-110">选项</span><span class="sxs-lookup"><span data-stu-id="3cb5e-110">Option</span></span> | <span data-ttu-id="3cb5e-111">描述</span><span class="sxs-lookup"><span data-stu-id="3cb5e-111">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3cb5e-112">全部</span><span class="sxs-lookup"><span data-stu-id="3cb5e-112">All</span></span> | <span data-ttu-id="3cb5e-113">指定所有验证可能，应都执行的程序包。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-113">Specifies that all verifications possible should be performed on the package(s).</span></span> |
| <span data-ttu-id="3cb5e-114">CertificateFingerprint</span><span class="sxs-lookup"><span data-stu-id="3cb5e-114">CertificateFingerprint</span></span> | <span data-ttu-id="3cb5e-115">指定一个或多个 sha-256 证书指纹的证书 (s) 必须使用签名的已签名的软件包。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-115">Specifies one or more SHA-256 certificate fingerprints of certificates(s) which signed packages must be signed with.</span></span> <span data-ttu-id="3cb5e-116">Sha-256 证书指纹是该证书的 sha-256 哈希。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-116">A certificate SHA-256 fingerprint is a SHA-256 hash of the certificate.</span></span> <span data-ttu-id="3cb5e-117">多个输入应为以分号分隔。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-117">Multiple inputs should be semicolon separated.</span></span> |
| <span data-ttu-id="3cb5e-118">ConfigFile</span><span class="sxs-lookup"><span data-stu-id="3cb5e-118">ConfigFile</span></span> | <span data-ttu-id="3cb5e-119">要应用的 NuGet 配置文件。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-119">The NuGet configuration file to apply.</span></span> <span data-ttu-id="3cb5e-120">如果未指定， `%AppData%\NuGet\NuGet.Config` (Windows) 或`~/.nuget/NuGet/NuGet.Config`使用 (Mac/Linux)。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-120">If not specified, `%AppData%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux) is used.</span></span>|
| <span data-ttu-id="3cb5e-121">ForceEnglishOutput</span><span class="sxs-lookup"><span data-stu-id="3cb5e-121">ForceEnglishOutput</span></span> | <span data-ttu-id="3cb5e-122">强制 nuget.exe 运行使用固定的、 基于英语的区域性。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-122">Forces nuget.exe to run using an invariant, English-based culture.</span></span> |
| <span data-ttu-id="3cb5e-123">帮助</span><span class="sxs-lookup"><span data-stu-id="3cb5e-123">Help</span></span> | <span data-ttu-id="3cb5e-124">显示的帮助命令的信息。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-124">Displays help information for the command.</span></span> |
| <span data-ttu-id="3cb5e-125">NonInteractive</span><span class="sxs-lookup"><span data-stu-id="3cb5e-125">NonInteractive</span></span> | <span data-ttu-id="3cb5e-126">取消显示提示用户输入或确认。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-126">Suppresses prompts for user input or confirmations.</span></span> |
| <span data-ttu-id="3cb5e-127">签名</span><span class="sxs-lookup"><span data-stu-id="3cb5e-127">Signatures</span></span> | <span data-ttu-id="3cb5e-128">指定应执行包签名验证。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-128">Specifies that package signature verification should be performed.</span></span> |
| <span data-ttu-id="3cb5e-129">详细级别</span><span class="sxs-lookup"><span data-stu-id="3cb5e-129">Verbosity</span></span> | <span data-ttu-id="3cb5e-130">指定的输出中显示的详细信息量：*正常*， *quiet*，*详细*。</span><span class="sxs-lookup"><span data-stu-id="3cb5e-130">Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*.</span></span> |

## <a name="examples"></a><span data-ttu-id="3cb5e-131">示例</span><span class="sxs-lookup"><span data-stu-id="3cb5e-131">Examples</span></span>

```cli
nuget verify -Signatures .\..\MyPackage.nupkg -CertificateFingerprint "CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039;5F874AAF47BCB268A19357364E7FBB09D6BF9E8A93E1229909AC5CAC865802E2" -Verbosity detailed

nuget verify -Signatures c:\packages\MyPackage.nupkg -CertificateFingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039

nuget verify -Signatures MyPackage.nupkg -Verbosity quiet

nuget verify -Signatures .\*.nupkg
```