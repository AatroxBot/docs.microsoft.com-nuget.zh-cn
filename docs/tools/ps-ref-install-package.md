---
title: NuGet 安装包 PowerShell 参考
description: 在 Visual Studio 中的 NuGet 包管理器控制台的安装包 PowerShell 命令参考。
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 06/01/2017
ms.topic: reference
ms.openlocfilehash: 5adfbcae0affcaa402f7981c12e108490d546511
ms.sourcegitcommit: 3eab9c4dd41ea7ccd2c28bb5ab16f6fbbec13708
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="install-package-package-manager-console-in-visual-studio"></a><span data-ttu-id="dbe5c-103">Install-Package （Visual Studio 中的程序包管理器控制台）</span><span class="sxs-lookup"><span data-stu-id="dbe5c-103">Install-Package (Package Manager Console in Visual Studio)</span></span>

<span data-ttu-id="dbe5c-104">*本主题介绍中的命令[NuGet 包管理器控制台](package-manager-console.md)Windows 上的 Visual Studio 中。泛型的 PowerShell Install-package 命令，请参阅[PowerShell PackageManagement 引用](/powershell/module/packagemanagement/?view=powershell-6)。*</span><span class="sxs-lookup"><span data-stu-id="dbe5c-104">*This topic describes the command within the [NuGet Package Manager Console](package-manager-console.md) in Visual Studio on Windows. For the generic PowerShell Install-Package command, see the [PowerShell PackageManagement reference](/powershell/module/packagemanagement/?view=powershell-6).*</span></span>

<span data-ttu-id="dbe5c-105">到项目中安装包及其依赖项。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-105">Installs a package and its dependencies into a project.</span></span>

## <a name="syntax"></a><span data-ttu-id="dbe5c-106">语法</span><span class="sxs-lookup"><span data-stu-id="dbe5c-106">Syntax</span></span>

```ps
Install-Package [-Id] <string> [-IgnoreDependencies] [-ProjectName <string>] [[-Source] <string>] 
    [[-Version] <string>] [-IncludePrerelease] [-FileConflictAction] [-DependencyVersion]
    [-WhatIf] [<CommonParameters>]
```

<span data-ttu-id="dbe5c-107">在 NuGet 2.8 +`Install-Package`将你的项目中的现有包降级。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-107">In NuGet 2.8+, `Install-Package` can downgrade an existing package in your project.</span></span> <span data-ttu-id="dbe5c-108">例如，如果你有 Microsoft.AspNet.MVC 5.1.0-rc1 安装，以下命令会将它降级到 5.0.0:</span><span class="sxs-lookup"><span data-stu-id="dbe5c-108">For example, if you have Microsoft.AspNet.MVC 5.1.0-rc1 installed, the following command would downgrade it to 5.0.0:</span></span>

```ps
Install-Package Microsoft.AspNet.MVC -Version 5.0.0.
```

## <a name="parameters"></a><span data-ttu-id="dbe5c-109">参数</span><span class="sxs-lookup"><span data-stu-id="dbe5c-109">Parameters</span></span>

| <span data-ttu-id="dbe5c-110">参数</span><span class="sxs-lookup"><span data-stu-id="dbe5c-110">Parameter</span></span> | <span data-ttu-id="dbe5c-111">描述</span><span class="sxs-lookup"><span data-stu-id="dbe5c-111">Description</span></span> |
| --- | --- |
| <span data-ttu-id="dbe5c-112">Id</span><span class="sxs-lookup"><span data-stu-id="dbe5c-112">Id</span></span> | <span data-ttu-id="dbe5c-113">（必需）要安装的包的标识符。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-113">(Required) The identifier of the package to install.</span></span> <span data-ttu-id="dbe5c-114">(*3.0 +*) 路径或 URL，该标识符可以是`packages.config`文件或`.nupkg`文件。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-114">(*3.0+*) The identifier can be a path or URL of a `packages.config` file or a `.nupkg` file.</span></span> <span data-ttu-id="dbe5c-115">-Id 开关本身是可选的。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-115">The -Id switch itself is optional.</span></span> |
| <span data-ttu-id="dbe5c-116">IgnoreDependencies</span><span class="sxs-lookup"><span data-stu-id="dbe5c-116">IgnoreDependencies</span></span> | <span data-ttu-id="dbe5c-117">安装仅此包不及其依赖项。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-117">Install only this package and not its dependencies.</span></span> |
| <span data-ttu-id="dbe5c-118">ProjectName</span><span class="sxs-lookup"><span data-stu-id="dbe5c-118">ProjectName</span></span> | <span data-ttu-id="dbe5c-119">要安装包，默认为默认项目到项目。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-119">The project into which to install the package, defaulting to the default project.</span></span> |
| <span data-ttu-id="dbe5c-120">源</span><span class="sxs-lookup"><span data-stu-id="dbe5c-120">Source</span></span> | <span data-ttu-id="dbe5c-121">要搜索的程序包源 URL 或文件夹路径。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-121">The URL or folder path for the package source to search.</span></span> <span data-ttu-id="dbe5c-122">本地文件夹路径可以是绝对的或相对于当前文件夹。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-122">Local folder paths can be absolute, or relative to the current folder.</span></span> <span data-ttu-id="dbe5c-123">如果省略，`Install-Package`搜索当前选定的程序包源。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-123">If omitted, `Install-Package` searches the currently selected package source.</span></span> |
| <span data-ttu-id="dbe5c-124">版本</span><span class="sxs-lookup"><span data-stu-id="dbe5c-124">Version</span></span> | <span data-ttu-id="dbe5c-125">若要安装，包的版本默认为最新版本。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-125">The version of the package to install, defaulting to the latest version.</span></span> |
| <span data-ttu-id="dbe5c-126">IncludePrerelease</span><span class="sxs-lookup"><span data-stu-id="dbe5c-126">IncludePrerelease</span></span> | <span data-ttu-id="dbe5c-127">将安装的预发行程序包。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-127">Considers prerelease packages for the install.</span></span> <span data-ttu-id="dbe5c-128">如果省略，则考虑仅稳定的程序包。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-128">If omitted, only stable packages are considered.</span></span> |
| <span data-ttu-id="dbe5c-129">FileConflictAction</span><span class="sxs-lookup"><span data-stu-id="dbe5c-129">FileConflictAction</span></span> | <span data-ttu-id="dbe5c-130">当系统询问是覆盖还是忽略所引用的项目的现有文件时要执行操作。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-130">The action to take when asked to overwrite or ignore existing files referenced by the project.</span></span> <span data-ttu-id="dbe5c-131">可能的值为*覆盖，忽略，无、 OverwriteAll*，和 *（3.0 +）* *IgnoreAll*。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-131">Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *(3.0+)* *IgnoreAll*.</span></span> |
| <span data-ttu-id="dbe5c-132">DependencyVersion</span><span class="sxs-lookup"><span data-stu-id="dbe5c-132">DependencyVersion</span></span> | <span data-ttu-id="dbe5c-133">版本的依赖项包若要使用，可以是下列项之一：</span><span class="sxs-lookup"><span data-stu-id="dbe5c-133">The version of the dependency packages to use, which can be one of the following:</span></span><br/><ul><li><span data-ttu-id="dbe5c-134">*最低*（默认值）： 最低版本</span><span class="sxs-lookup"><span data-stu-id="dbe5c-134">*Lowest* (default): the lowest version</span></span></li><li><span data-ttu-id="dbe5c-135">*HighestPatch*： 具有的最低主要、 越小越小、 最高的修补程序版本</span><span class="sxs-lookup"><span data-stu-id="dbe5c-135">*HighestPatch*: the version with the lowest major, lowest minor, highest patch</span></span></li><li><span data-ttu-id="dbe5c-136">*HighestMinor*： 具有最低主要版本、 最高次，最高的修补程序</span><span class="sxs-lookup"><span data-stu-id="dbe5c-136">*HighestMinor*: the version with the lowest major, highest minor, highest patch</span></span></li><li><span data-ttu-id="dbe5c-137">*最高*（默认值为更新包不带任何参数）： 最高的版本</span><span class="sxs-lookup"><span data-stu-id="dbe5c-137">*Highest* (default for Update-Package with no parameters): the highest version</span></span></li></ul><span data-ttu-id="dbe5c-138">你可以设置默认值使用[ `dependencyVersion` ](../reference/nuget-config-file.md#config-section)中设置`Nuget.Config`文件。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-138">You can set the default value using the [`dependencyVersion`](../reference/nuget-config-file.md#config-section) setting in the `Nuget.Config` file.</span></span> |
| <span data-ttu-id="dbe5c-139">WhatIf</span><span class="sxs-lookup"><span data-stu-id="dbe5c-139">WhatIf</span></span> | <span data-ttu-id="dbe5c-140">显示运行命令而不实际执行安装时，会发生什么情况。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-140">Shows what would happen when running the command without actually performing the install.</span></span> |

<span data-ttu-id="dbe5c-141">任何这些参数接受管道输入或通配符字符。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-141">None of these parameters accept pipeline input or wildcard characters.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="dbe5c-142">通用参数</span><span class="sxs-lookup"><span data-stu-id="dbe5c-142">Common Parameters</span></span>

<span data-ttu-id="dbe5c-143">`Install-Package` 支持以下[常见的 PowerShell 参数](http://go.microsoft.com/fwlink/?LinkID=113216)： 调试、 错误操作、 ErrorVariable、 OutBuffer、 OutVariable、 PipelineVariable、 Verbose、 WarningAction 和 WarningVariable。</span><span class="sxs-lookup"><span data-stu-id="dbe5c-143">`Install-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.</span></span>

## <a name="examples"></a><span data-ttu-id="dbe5c-144">示例</span><span class="sxs-lookup"><span data-stu-id="dbe5c-144">Examples</span></span>

```ps
# Installs the latest version of Elmah from the current source into the default project
Install-Package Elmah

# Installs Glimpse 1.0.0 into the MvcApplication1 project
Install-Package Glimpse -Version 1.0.0 -Project MvcApplication1

# Installs Ninject.Mvc3 but not its dependencies from c:\temp\packages
Install-Package Ninject.Mvc3 -IgnoreDependencies -Source c:\temp\packages

# Installs the package listed on the online packages.config into the current project
Install-package https://raw.githubusercontent.com/json-ld.net/master/src/JsonLD/packages.config

# Installs jquery 1.10.2 package, using the .nupkg file under local path of c:\temp\packages
Install-package c:\temp\packages\jQuery.1.10.2.nupkg

# Installs the specific online package
Install-package https://az320820.vo.msecnd.net/packages/microsoft.aspnet.mvc.5.2.3.nupkg
```