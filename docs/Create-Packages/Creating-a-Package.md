---
title: "如何创建 NuGet 包 | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 12/12/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 456797cb-e3e4-4b88-9b01-8b5153cee802
description: "设计和创建 NuGet 包流程的详细指南，包含文件和版本控制等关键决策点。"
keywords: "NuGet 包创建, 创建包, nuspec 清单, NuGet 包约定, NuGet 包版本"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: e7a2c4d02afb2387161c22fe5bd443eb0991ea8c
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2017
---
# <a name="creating-nuget-packages"></a><span data-ttu-id="9ad17-104">创建 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="9ad17-104">Creating NuGet packages</span></span>

<span data-ttu-id="9ad17-105">无论包是什么用途或者它包含什么代码，均使用 `nuget.exe` 将该功能打包进可以与任意数量的其他开发人员共享且可以由其使用的组件中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-105">No matter what your package does or what code it contains, you use `nuget.exe` to package that functionality into a component that can be shared with and used by any number of other developers.</span></span> <span data-ttu-id="9ad17-106">若要安装 `nuget.exe`，请参阅[安装 NuGet CLI](../guides/Install-NuGet.md#nuget-cli)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-106">To install `nuget.exe`, see [Install NuGet CLI](../guides/Install-NuGet.md#nuget-cli).</span></span> <span data-ttu-id="9ad17-107">请注意，Visual Studio 不会自动包含 `nuget.exe`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-107">Note that Visual Studio does not automatically include `nuget.exe`.</span></span>

<span data-ttu-id="9ad17-108">从技术角度来讲，NuGet 包仅仅是一个使用 `.nupkg` 扩展重命名且其中的内容与特定约定相匹配的 ZIP 文件。</span><span class="sxs-lookup"><span data-stu-id="9ad17-108">Technically speaking, a NuGet package is just a ZIP file that's been renamed with the `.nupkg` extension and whose contents match certain conventions.</span></span> <span data-ttu-id="9ad17-109">本主题介绍创建满足这些约定的包的详细流程。</span><span class="sxs-lookup"><span data-stu-id="9ad17-109">This topic describes the detailed process of creating a package that meets those conventions.</span></span> <span data-ttu-id="9ad17-110">有关重点演练，请参阅[创建和发布包快速入门](../quickstart/create-and-publish-a-package.md)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-110">For a focused walkthrough, refer to [Create and Publish a Package Quickstart](../quickstart/create-and-publish-a-package.md).</span></span>

<span data-ttu-id="9ad17-111">包以编译的代码（程序集）、符号和/或需要作为包传送的其他文件开头（请参阅[概述和工作流](Overview-and-Workflow.md)）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-111">Packaging begins with the compiled code (assemblies), symbols, and/or other files that you want to deliver as a package (see [Overview and workflow](Overview-and-Workflow.md)).</span></span> <span data-ttu-id="9ad17-112">尽管可以使用项目文件中信息的描述保持编译程序集和包的同步，但此流程独立于编译或生成进入包的文件。</span><span class="sxs-lookup"><span data-stu-id="9ad17-112">This process is independent from compiling or otherwise generating the files that go into the package, although you can use draw from information in a project file to keep the compiled assemblines and packages in sync.</span></span>

<span data-ttu-id="9ad17-113">在本主题中：</span><span class="sxs-lookup"><span data-stu-id="9ad17-113">In this topic:</span></span>

- [<span data-ttu-id="9ad17-114">确定要打包的程序集</span><span class="sxs-lookup"><span data-stu-id="9ad17-114">Deciding which assemblies to package</span></span>](#deciding-which-assemblies-to-package)
- [<span data-ttu-id="9ad17-115">`.nuspec` 文件的角色和结构</span><span class="sxs-lookup"><span data-stu-id="9ad17-115">The role and structure of the `.nuspec` file</span></span>](#the-role-and-structure-of-the-nuspec-file)
- <span data-ttu-id="9ad17-116">[创建 `.nuspec` 文件](#creating-the-nuspec-file)，从：</span><span class="sxs-lookup"><span data-stu-id="9ad17-116">[Creating the `.nuspec` file](#creating-the-nuspec-file) from:</span></span>
    - [<span data-ttu-id="9ad17-117">基于约定的工作目录</span><span class="sxs-lookup"><span data-stu-id="9ad17-117">A convention-based working directory</span></span>](#from-a-convention-based-working-directory)
    - [<span data-ttu-id="9ad17-118">程序集 DLL</span><span class="sxs-lookup"><span data-stu-id="9ad17-118">An assembly DLL</span></span>](#from-an-assembly-dll)
    - [<span data-ttu-id="9ad17-119">Visual Studio 项目</span><span class="sxs-lookup"><span data-stu-id="9ad17-119">A Visual Studio project</span></span>](#from-a-visual-studio-project)
    - [<span data-ttu-id="9ad17-120">包含默认值的新文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-120">New file with default values</span></span>](#new-file-with-default-values)    
- [<span data-ttu-id="9ad17-121">选择唯一的包标识符并设置版本号</span><span class="sxs-lookup"><span data-stu-id="9ad17-121">Choosing a unique package identifier and setting the version number</span></span>](#choosing-a-unique-package-identifier-and-setting-the-version-number)
- <span data-ttu-id="9ad17-122">[设置包类型](#setting-a-package-type)（NuGet 3.5 以及更高版本）</span><span class="sxs-lookup"><span data-stu-id="9ad17-122">[Setting a package type](#setting-a-package-type) (NuGet 3.5 and later)</span></span>
- [<span data-ttu-id="9ad17-123">添加自述文件和其他文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-123">Adding a readme and other files</span></span>](#adding-a-readme-and-other-files)
- [<span data-ttu-id="9ad17-124">将 MSBuild 属性和目标包含到包中</span><span class="sxs-lookup"><span data-stu-id="9ad17-124">Including MSBuild props and targets in a package</span></span>](#including-msbuild-props-and-targets-in-a-package)
- [<span data-ttu-id="9ad17-125">创作包含 COM 互操作程序集的包</span><span class="sxs-lookup"><span data-stu-id="9ad17-125">Authoring packages that contain COM interop assemblies</span></span>](#authoring-packages-with-com-interop-assemblies)
- [<span data-ttu-id="9ad17-126">运行 NuGet 包生成 .nupkg 文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-126">Running nuget pack to generate the .nupkg file</span></span>](#running-nuget-pack-to-generate-the-nupkg-file)

<span data-ttu-id="9ad17-127">完成这些核心步骤以后，可以集成多种其他功能（如本文档其他部分所述）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-127">After these core steps, you can incorporate a variety of other features as described elsewhere in this documentation.</span></span> <span data-ttu-id="9ad17-128">请参阅以下[后续步骤](#next-steps)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-128">See [Next steps](#next-steps) below.</span></span>

> [!Note]
> <span data-ttu-id="9ad17-129">本主题适用于除使用 Visual Studio 2017 和 NuGet 4.0+ 的 .NET Core 项目以外的项目类型。</span><span class="sxs-lookup"><span data-stu-id="9ad17-129">This topic applies to project types other than .NET Core projects using Visual Studio 2017 and NuGet 4.0+.</span></span> <span data-ttu-id="9ad17-130">在这些 .NET Core 项目中，NuGet 直接使用 `.csproj` 文件中的信息。</span><span class="sxs-lookup"><span data-stu-id="9ad17-130">In those .NET Core projects, NuGet uses information in the `.csproj` file directly.</span></span> <span data-ttu-id="9ad17-131">有关详细信息，请参阅[使用 Visual Studio 2017 创建 .NET Standard 包](../guides/create-net-standard-packages-vs2017.md)和[ NuGet 包和作为 MSBuild 目标还原](../schema/msbuild-targets.md)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-131">For details, see [Create .NET Standard Packages with Visual Studio 2017](../guides/create-net-standard-packages-vs2017.md) and [NuGet pack and restore as MSBuild targets](../schema/msbuild-targets.md).</span></span>

## <a name="deciding-which-assemblies-to-package"></a><span data-ttu-id="9ad17-132">确定要打包的程序集</span><span class="sxs-lookup"><span data-stu-id="9ad17-132">Deciding which assemblies to package</span></span>

<span data-ttu-id="9ad17-133">大多数通用包包含一个或多个其他开发人员可在其自己项目中使用的程序集。</span><span class="sxs-lookup"><span data-stu-id="9ad17-133">Most general-purpose packages contain one or more assemblies that other developers can use in their own projects.</span></span>

- <span data-ttu-id="9ad17-134">通常情况下，最好每个 NuGet 包有一个程序集，前提是每个程序集可独立正常运行。</span><span class="sxs-lookup"><span data-stu-id="9ad17-134">In general, it's best to have one assembly per NuGet package, provided that each assembly is independently useful.</span></span> <span data-ttu-id="9ad17-135">例如，如果有一个 `Utilities.dll` 依赖于 `Parser.dll` 且 `Parser.dll` 可独立正常运行，则为每一个创建一个包。</span><span class="sxs-lookup"><span data-stu-id="9ad17-135">For example, if you have a `Utilities.dll` that depends on `Parser.dll`, and `Parser.dll` is useful on its own, then create one package for each.</span></span> <span data-ttu-id="9ad17-136">这样做使得开发人员能够独立于 `Utilities.dll` 使用 `Parser.dll`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-136">Doing so allows developers to use `Parser.dll` independently of `Utilities.dll`.</span></span>

- <span data-ttu-id="9ad17-137">如果库由多个不能独立正常运行的程序集构成，那么可以将它们合并到一个包中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-137">If your library is composed of multiple assemblies that aren't independently useful, then it's fine to combine them into one package.</span></span> <span data-ttu-id="9ad17-138">使用上面的示例，如果 `Parser.dll` 包含仅由 `Utilities.dll` 使用的代码，那么可以将 `Parser.dll` 保留在同一包中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-138">Using the previous example, if `Parser.dll` contains code that's used only by `Utilities.dll`, then it's fine to keep `Parser.dll` in the same package.</span></span>
    - <span data-ttu-id="9ad17-139">同样，如果 `Utilities.dll` 依赖于 `Utilities.resources.dll` 并且后者不能独立正常运行，则将两者放入同一包中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-139">Similarly, if `Utilities.dll` depends on `Utilities.resources.dll`, where again the latter is not useful on its own, then put both in the same package.</span></span>

<span data-ttu-id="9ad17-140">事实上，资源是一种特殊情况。</span><span class="sxs-lookup"><span data-stu-id="9ad17-140">Resources are, in fact, a special case.</span></span> <span data-ttu-id="9ad17-141">当包安装到项目中时，NuGet 自动将程序集引用添加到包的 DLL，不包含命名为 `.resources.dll` 的内容，因为它们被假定为本地化的附属程序集（请参阅[创建本地化包](creating-localized-packages.md)）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-141">When a package is installed into a project, NuGet automatically adds assembly references to the package's DLLs, *excluding* those that are named `.resources.dll` because they are assumed to be localized satellite assemblies (see [Creating localized packages](creating-localized-packages.md)).</span></span> <span data-ttu-id="9ad17-142">为此，请避免对包含基本包代码的文件使用 `.resources.dll`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-142">For this reason, avoid using `.resources.dll` for files that otherwise contain essential package code.</span></span>

<span data-ttu-id="9ad17-143">如果库包含 COM 互操作程序集，请遵循[使用 COM 互操作程序集创作包](#authoring-packages-with-com-interop-assemblies)中的其他准则。</span><span class="sxs-lookup"><span data-stu-id="9ad17-143">If your library contains COM interop assemblies, follow additional the guidelines in [Authoring packages with COM interop assemblies](#authoring-packages-with-com-interop-assemblies).</span></span>

## <a name="the-role-and-structure-of-the-nuspec-file"></a><span data-ttu-id="9ad17-144">.nuspec 文件的角色和结构</span><span class="sxs-lookup"><span data-stu-id="9ad17-144">The role and structure of the .nuspec file</span></span>

<span data-ttu-id="9ad17-145">一旦知道需要打包的文件后，下一步是在 `.nuspec` XML 文件中创建包清单。</span><span class="sxs-lookup"><span data-stu-id="9ad17-145">Once you know what files you want to package, the next step is creating a package manifest in a `.nuspec` XML file.</span></span>

<span data-ttu-id="9ad17-146">清单：</span><span class="sxs-lookup"><span data-stu-id="9ad17-146">The manifest:</span></span>

1. <span data-ttu-id="9ad17-147">介绍包的内容和自己是否包含在包中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-147">Describes the package's contents and is itself included in the package.</span></span>
1. <span data-ttu-id="9ad17-148">驱动包的创建并且指示 NuGet 如何将包安装到项目。</span><span class="sxs-lookup"><span data-stu-id="9ad17-148">Drives both the creation of the package and instructs NuGet on how to install the package into a project.</span></span> <span data-ttu-id="9ad17-149">例如，清单识别其他包依赖项，这样，NuGet 还可以在安装主包时安装这些依赖项。</span><span class="sxs-lookup"><span data-stu-id="9ad17-149">For example, the manifest identifies other package dependencies such that NuGet can also install those dependencies when the main package is installed.</span></span>
1. <span data-ttu-id="9ad17-150">如下所示，包含所需属性和可选属性。</span><span class="sxs-lookup"><span data-stu-id="9ad17-150">Contains both required and optional properties as described below.</span></span> <span data-ttu-id="9ad17-151">有关确切的详细信息（包括此处未提及的其他属性），请参阅 [.nuspec 引用](../schema/nuspec.md)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-151">For exact details, including other properties not mentioned here, see  the [.nuspec reference](../schema/nuspec.md).</span></span>

<span data-ttu-id="9ad17-152">所需属性：</span><span class="sxs-lookup"><span data-stu-id="9ad17-152">Required properties:</span></span>

- <span data-ttu-id="9ad17-153">包标识符在承载包的库中必须是唯一的。</span><span class="sxs-lookup"><span data-stu-id="9ad17-153">The package identifier, which must be unique across the gallery that hosts the package.</span></span>
- <span data-ttu-id="9ad17-154">窗体 Major.Minor.Patch[-Suffix] 中特定的版本号，其中 -Suffix 标识[预发布版本](Prerelease-Packages.md)</span><span class="sxs-lookup"><span data-stu-id="9ad17-154">A specific version number in the form *Major.Minor.Patch[-Suffix]* where *-Suffix* identifies [pre-release versions](Prerelease-Packages.md)</span></span>
- <span data-ttu-id="9ad17-155">包标题应出现在主机上（例如 nuget.org）</span><span class="sxs-lookup"><span data-stu-id="9ad17-155">The package title as it should appears on the host (like nuget.org)</span></span>
- <span data-ttu-id="9ad17-156">创建者和所有者信息。</span><span class="sxs-lookup"><span data-stu-id="9ad17-156">Author and owner information.</span></span>
- <span data-ttu-id="9ad17-157">对包的详细说明。</span><span class="sxs-lookup"><span data-stu-id="9ad17-157">A long description of the package.</span></span>

<span data-ttu-id="9ad17-158">常见可选属性：</span><span class="sxs-lookup"><span data-stu-id="9ad17-158">Common optional properties:</span></span>

- <span data-ttu-id="9ad17-159">发行说明</span><span class="sxs-lookup"><span data-stu-id="9ad17-159">Release notes</span></span>
- <span data-ttu-id="9ad17-160">版权信息</span><span class="sxs-lookup"><span data-stu-id="9ad17-160">Copyright information</span></span>
- <span data-ttu-id="9ad17-161">对 [Visual Studio 中的包管理器 UI](../Tools/Package-Manager-UI.md) 的简短说明</span><span class="sxs-lookup"><span data-stu-id="9ad17-161">A short description for the [Package Manager UI in Visual Studio](../Tools/Package-Manager-UI.md)</span></span>
- <span data-ttu-id="9ad17-162">区域设置 ID</span><span class="sxs-lookup"><span data-stu-id="9ad17-162">A locale ID</span></span>
- <span data-ttu-id="9ad17-163">主页和许可证 URL</span><span class="sxs-lookup"><span data-stu-id="9ad17-163">Home page and license URLs</span></span>
- <span data-ttu-id="9ad17-164">图标 URL</span><span class="sxs-lookup"><span data-stu-id="9ad17-164">An icon URL</span></span>
- <span data-ttu-id="9ad17-165">依赖项和引用的列表</span><span class="sxs-lookup"><span data-stu-id="9ad17-165">Lists of dependencies and references</span></span>
- <span data-ttu-id="9ad17-166">协助库搜索的标记</span><span class="sxs-lookup"><span data-stu-id="9ad17-166">Tags that assist in gallery searches</span></span>

<span data-ttu-id="9ad17-167">以下是典型（但虚构）的 `.nuspec` 文件，其中注释介绍属性：</span><span class="sxs-lookup"><span data-stu-id="9ad17-167">The following is a typical (but fictitious) `.nuspec` file, with comments describing the properties:</span></span>

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
    <metadata>
    <!-- The identifier that must be unique within the hosting gallery -->
    <id>Contoso.Utility.UsefulStuff</id>

    <!-- The package version number that is used when resolving dependencies -->
    <version>1.8.3-beta</version>

    <!-- Authors contain text that appears directly on the gallery -->
    <authors>Dejana Tesic, Rajeev Dey</authors>

    <!-- Owners are typically nuget.org identities that allow gallery
            users to easily find other packages by the same owners.  -->
    <owners>dejanatc, rjdey</owners>

    <!-- License and project URLs provide links for the gallery -->
    <licenseUrl>http://opensource.org/licenses/MS-PL</licenseUrl>
    <projectUrl>http://github.com/contoso/UsefulStuff</projectUrl>

    <!-- The icon is used in Visual Studio's package manager UI -->
    <iconUrl>http://github.com/contoso/UsefulStuff/nuget_icon.png</iconUrl>

    <!-- If true, this value prompts the user to accept the license when
            installing the package. -->
    <requireLicenseAcceptance>false</requireLicenseAcceptance>

    <!-- Any details about this particular release -->
    <releaseNotes>Bug fixes and performance improvements</releaseNotes>

    <!-- The description can be used in package manager UI. Note that the
            nuget.org gallery uses information you add in the portal. -->
    <description>Core utility functions for web applications</description>

    <!-- Copyright information -->
    <copyright>Copyright ©2016 Contoso Corporation</copyright>

    <!-- Tags appear in the gallery and can be used for tag searches -->
    <tags>web utility http json url parsing</tags>

    <!-- Dependencies are automatically installed when the package is installed -->
    <dependencies>
        <dependency id="Newtonsoft.Json" version="9.0" />
    </dependencies>
    </metadata>

    <!-- A readme.txt to display when the package is installed -->
    <files>
        <file src="readme.txt" target="" />
    </files>
</package>
```

<span data-ttu-id="9ad17-168">有关声明依赖项并指定版本号的详细信息，请参阅[包版本控制](../reference/package-versioning.md)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-168">For details on declaring dependencies and specifying version numbers, see [Package versioning](../reference/package-versioning.md).</span></span> <span data-ttu-id="9ad17-169">还可以使用 `dependency` 元素上的 `include` 和 `exclude` 特性直接在包中结合依赖项的资产。</span><span class="sxs-lookup"><span data-stu-id="9ad17-169">It is also possible to surface assets from dependencies directly in the package by using the `include` and `exclude` attributes on the `dependency` element.</span></span> <span data-ttu-id="9ad17-170">请参阅 [.nuspec 引用 - 依赖项](../Schema/nuspec.md#dependencies)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-170">See [.nuspec Reference - Dependencies](../Schema/nuspec.md#dependencies).</span></span>

<span data-ttu-id="9ad17-171">因为清单包含在从清单创建的包中，所以可以通过检查现有包查找任意数目的其他示例。</span><span class="sxs-lookup"><span data-stu-id="9ad17-171">Because the manifest is included in the package created from it, you can find any number of additional examples by examining existing packages.</span></span> <span data-ttu-id="9ad17-172">计算机上的全局包缓存是一个不错的源，其位置由以下命令返回：</span><span class="sxs-lookup"><span data-stu-id="9ad17-172">A good source is the global package cache on your machine, the location of which is returned by the following command:</span></span>

```
nuget locals -list global-packages
```

<span data-ttu-id="9ad17-173">转到任意 package\version 文件夹，将 `.nupkg` 文件复制到 `.zip` 文件，然后打开该 `.zip` 文件并检查其中的 `.nuspec`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-173">Go into any *package\version* folder, copy the `.nupkg` file to a `.zip` file, then open that `.zip` file and examine the `.nuspec` within it.</span></span>

> [!Note]
> <span data-ttu-id="9ad17-174">从 Visual Studio 项目创建 `.nuspec` 时，清单包含生成包时被项目信息替换的令牌。</span><span class="sxs-lookup"><span data-stu-id="9ad17-174">When creating a `.nuspec` from a Visual Studio project, the manifest contains tokens that are be replaced with information from the project when the package is built.</span></span> <span data-ttu-id="9ad17-175">请参阅[从 Visual Studio 项目创建 .nuspec](#from-a-visual-studio-project)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-175">See [Creating the .nuspec from a Visual Studio project](#from-a-visual-studio-project).</span></span>

## <a name="creating-the-nuspec-file"></a><span data-ttu-id="9ad17-176">创建 .nuspec 文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-176">Creating the .nuspec file</span></span>

<span data-ttu-id="9ad17-177">创建完整的清单通常以通过以下其中一种方法生成的基础 `.nuspec` 文件开始：</span><span class="sxs-lookup"><span data-stu-id="9ad17-177">Creating a complete manifest typically begins with a basic `.nuspec` file generated through one of the following methods:</span></span>

- [<span data-ttu-id="9ad17-178">基于约定的工作目录</span><span class="sxs-lookup"><span data-stu-id="9ad17-178">A convention-based working directory</span></span>](#from-a-convention-based-working-directory)
- [<span data-ttu-id="9ad17-179">程序集 DLL</span><span class="sxs-lookup"><span data-stu-id="9ad17-179">An assembly DLL</span></span>](#from-an-assembly-dll)
- [<span data-ttu-id="9ad17-180">Visual Studio 项目</span><span class="sxs-lookup"><span data-stu-id="9ad17-180">A Visual Studio project</span></span>](#from-a-visual-studio-project)    
- [<span data-ttu-id="9ad17-181">包含默认值的新文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-181">New file with default values</span></span>](#new-file-with-default-values)

<span data-ttu-id="9ad17-182">然后手动编辑文件，使它可以在最终包中描述所需的确切内容。</span><span class="sxs-lookup"><span data-stu-id="9ad17-182">You then edit the file by hand so that it describes the exact content you want in the final package.</span></span>

> [!Important]
> <span data-ttu-id="9ad17-183">生成 `.nuspec` 文件包含的占位符必须在使用 `nuget pack` 命令创建包前修改。</span><span class="sxs-lookup"><span data-stu-id="9ad17-183">Generated `.nuspec` files contain placeholders that must be modified before creating the package with the `nuget pack` command.</span></span> <span data-ttu-id="9ad17-184">如果 `.nuspec` 包含任意占位符，则命令失败。</span><span class="sxs-lookup"><span data-stu-id="9ad17-184">That command fails if the `.nuspec` contains any placeholders.</span></span>

### <a name="from-a-convention-based-working-directory"></a><span data-ttu-id="9ad17-185">来自基于约定的工作目录</span><span class="sxs-lookup"><span data-stu-id="9ad17-185">From a convention-based working directory</span></span>

<span data-ttu-id="9ad17-186">因为 NuGet 包仅仅是使用 `.nupkg` 扩展名重命名的 ZIP 文件，所以在文件系统上创建所需文件夹结构，然后从该结构直接创建 `.nuspec` 文件通常最简单。</span><span class="sxs-lookup"><span data-stu-id="9ad17-186">Because a NuGet package is just a ZIP file that's been renamed with the `.nupkg` extension, its often easiest to create the folder structure you want on the file system, then create the `.nuspec` file directly from that structure.</span></span> <span data-ttu-id="9ad17-187">然后，`nuget pack` 命令自动将所有文件添加到该文件夹结构（不包含以 `.` 开头的文件夹，从而在同一结构中保留专用文件）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-187">The `nuget pack` command then automatically adds all files in that folder structure (excluding any folders that begin with `.`, allowing you keep private files in the same structure).</span></span>

<span data-ttu-id="9ad17-188">此方法的优势是无需在清单中指定需要包含在包中的文件（如本主题后面所述）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-188">The advantage to this approach is that you don't need to specify in the manifest which files you want to include in the package (as explained later in this topic).</span></span> <span data-ttu-id="9ad17-189">可以轻松使得生成进程生成进入包中的确切文件夹结构，并且可以轻松包含不是项目一部分的其他文件：</span><span class="sxs-lookup"><span data-stu-id="9ad17-189">You can simply have your build process produce the exact folder structure that goes into the package, and you can easily include other files that might not be part of a project otherwise:</span></span>

- <span data-ttu-id="9ad17-190">应注入目标项目的内容和源代码。</span><span class="sxs-lookup"><span data-stu-id="9ad17-190">Content and source code that should be injected into the target project.</span></span>
- <span data-ttu-id="9ad17-191">PowerShell 脚本（NuGet 2.x 中使用的包也可以包含安装脚本，这在 NuGet 3.x 以及更高版本中不受支持）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-191">PowerShell scripts (packages used in NuGet 2.x can include installation scripts as well, which is not supported in NuGet 3.x and later).</span></span>
- <span data-ttu-id="9ad17-192">转换到现有配置和项目中的源代码文件。</span><span class="sxs-lookup"><span data-stu-id="9ad17-192">Transformations to existing configuration and source code files in a project.</span></span>

<span data-ttu-id="9ad17-193">文件夹约定如下所示：</span><span class="sxs-lookup"><span data-stu-id="9ad17-193">The folder conventions are as follows:</span></span>

| <span data-ttu-id="9ad17-194">文件夹</span><span class="sxs-lookup"><span data-stu-id="9ad17-194">Folder</span></span> | <span data-ttu-id="9ad17-195">描述</span><span class="sxs-lookup"><span data-stu-id="9ad17-195">Description</span></span> | <span data-ttu-id="9ad17-196">包安装时的操作</span><span class="sxs-lookup"><span data-stu-id="9ad17-196">Action upon package install</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9ad17-197">（根）</span><span class="sxs-lookup"><span data-stu-id="9ad17-197">(root)</span></span> | <span data-ttu-id="9ad17-198">readme.txt 的位置</span><span class="sxs-lookup"><span data-stu-id="9ad17-198">Location for readme.txt</span></span> | <span data-ttu-id="9ad17-199">包安装时，Visual Studio 在包根目录中显示 readme.txt 文件。</span><span class="sxs-lookup"><span data-stu-id="9ad17-199">Visual Studio displays a readme.txt file in the package root when the package is installed.</span></span> |
| <span data-ttu-id="9ad17-200">lib/{tfm}</span><span class="sxs-lookup"><span data-stu-id="9ad17-200">lib/{tfm}</span></span> | <span data-ttu-id="9ad17-201">特定目标框架名字对象 (TFM) 的程序集 (`.dll`)、文档 (`.xml`) 和符号 (`.pdb`) 文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-201">Assembly (`.dll`), documentation (`.xml`), and symbol (`.pdb`) files for the given Target Framework Moniker (TFM)</span></span> | <span data-ttu-id="9ad17-202">程序集添加为引用；`.xml` 和 `.pdb` 复制到项目文件夹中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-202">Assemblies are added as references; `.xml` and `.pdb` copied into project folders.</span></span> <span data-ttu-id="9ad17-203">有关创建框架特定目标子文件夹的详细信息，请参阅[支持多个目标框架](Supporting-Multiple-Target-Frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-203">See [Supporting multiple target frameworks](Supporting-Multiple-Target-Frameworks.md) for creating framework target-specific sub-folders.</span></span> |
| <span data-ttu-id="9ad17-204">runtimes</span><span class="sxs-lookup"><span data-stu-id="9ad17-204">runtimes</span></span> | <span data-ttu-id="9ad17-205">特定于体系结构的程序集 (`.dll`)、符号 (`.pdb`) 和本机源 (`.pri`) 文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-205">Architecture-specific assembly (`.dll`), symbol (`.pdb`), and native resource (`.pri`) files</span></span> | <span data-ttu-id="9ad17-206">程序集添加为引用；其他文件复制到项目文件夹中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-206">Assemblies are added as references; other files are copied into project folders.</span></span> <span data-ttu-id="9ad17-207">请参阅[支持多个目标框架](Supporting-Multiple-Target-Frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-207">See [Supporting multiple target frameworks](Supporting-Multiple-Target-Frameworks.md).</span></span> |
| <span data-ttu-id="9ad17-208">内容</span><span class="sxs-lookup"><span data-stu-id="9ad17-208">content</span></span> | <span data-ttu-id="9ad17-209">任意文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-209">Arbitrary files</span></span> | <span data-ttu-id="9ad17-210">内容复制到项目根目录。</span><span class="sxs-lookup"><span data-stu-id="9ad17-210">Contents are copied to the project root.</span></span> <span data-ttu-id="9ad17-211">将“内容”文件夹视为最终使用包的目标应用程序的根目录。</span><span class="sxs-lookup"><span data-stu-id="9ad17-211">Think of the **content** folder as the root of the target application that ultimately consumes the package.</span></span> <span data-ttu-id="9ad17-212">若要使包在应用程序的 /images 文件夹中添加图片，请将其置于包的 content/images 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-212">To have the package add an image in the application's */images* folder, place it in the package's *content/images* folder.</span></span> |
| <span data-ttu-id="9ad17-213">生成</span><span class="sxs-lookup"><span data-stu-id="9ad17-213">build</span></span> | <span data-ttu-id="9ad17-214">MSBuild `.targets` 和 `.props` 文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-214">MSBuild `.targets` and `.props` files</span></span> | <span data-ttu-id="9ad17-215">自动插入到项目文件 (NuGet 2.x) 或 `project.lock.json` (NuGet 3.x+)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-215">Automatically inserted into the project file (NuGet 2.x) or `project.lock.json` (NuGet 3.x+).</span></span> |
| <span data-ttu-id="9ad17-216">工具</span><span class="sxs-lookup"><span data-stu-id="9ad17-216">tools</span></span> | <span data-ttu-id="9ad17-217">可从包管理器控制台访问 Powershell 脚本和程序</span><span class="sxs-lookup"><span data-stu-id="9ad17-217">Powershell scripts and programs accessible from the Package Manager Console</span></span> | <span data-ttu-id="9ad17-218">`tools` 文件夹添加到仅适用于包管理器控制台的 `PATH` 环境变量（尤其是不作为 MSBuild 集在生成项目时添加到 `PATH`）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-218">The `tools` folder is added to the `PATH` environment variable for the Package Manager Console only (Specifically, *not* to the `PATH` as set for MSBuild when building the project).</span></span> |

<span data-ttu-id="9ad17-219">因为文件夹结构可能包含任意数量的目标框架的任意数量的程序集，所以此方法在创建支持多个框架的包时是必要的</span><span class="sxs-lookup"><span data-stu-id="9ad17-219">Because your folder structure can contain any number of assemblies for any number of target frameworks, this method is necessary when creating packages that support multiple frameworks</span></span> 

<span data-ttu-id="9ad17-220">在任何情况下，一旦准备好所需文件夹结构，则在该文件夹中运行以下命令以创建 `.nuspec` 文件：</span><span class="sxs-lookup"><span data-stu-id="9ad17-220">In any case, once you have the desired folder structure in place, run the following command in that folder to create the `.nuspec` file:</span></span>

```
nuget spec
```

<span data-ttu-id="9ad17-221">同样，生成的 `.nuspec` 不包含文件夹结构中的文件的显式引用。</span><span class="sxs-lookup"><span data-stu-id="9ad17-221">Again, the generated `.nuspec` contains no explicit references to files in the folder structure.</span></span> <span data-ttu-id="9ad17-222">包创建时，NuGet 自动包含所有文件。</span><span class="sxs-lookup"><span data-stu-id="9ad17-222">NuGet automatically includes all files when the package is created.</span></span> <span data-ttu-id="9ad17-223">但是，还需在清单的其他部分中编辑占位符值。</span><span class="sxs-lookup"><span data-stu-id="9ad17-223">You still need to edit placeholder values in other parts of the manifest, however.</span></span>

### <a name="from-an-assembly-dll"></a><span data-ttu-id="9ad17-224">来自程序集 DLL</span><span class="sxs-lookup"><span data-stu-id="9ad17-224">From an assembly DLL</span></span>

<span data-ttu-id="9ad17-225">在从程序集创建包的简单情况下，可以使用以下命令从程序集中的元数据生成 `.nuspec` 文件：</span><span class="sxs-lookup"><span data-stu-id="9ad17-225">In the simple case of creating a package from an assembly, you can generate a `.nuspec` file from the metadata in the assembly using the following command:</span></span>

```
nuget spec <assembly-name>.dll
```

<span data-ttu-id="9ad17-226">使用此窗体将清单中的一些占位符替换为程序集的特定值。</span><span class="sxs-lookup"><span data-stu-id="9ad17-226">Using this form replaces a few placeholders in the manifest with specific values from the assembly.</span></span> <span data-ttu-id="9ad17-227">例如，`<id>` 属性设为程序集名称，`<version>` 设为程序集版本。</span><span class="sxs-lookup"><span data-stu-id="9ad17-227">For example, the `<id>` property is set to the assembly name, and `<version>` is set to the assembly version.</span></span> <span data-ttu-id="9ad17-228">但是，清单中的其他属性在程序集中没有匹配值，因此仍包含占位符。</span><span class="sxs-lookup"><span data-stu-id="9ad17-228">Other properties in the manifest, however, don't have matching values in the assembly and thus still contain placeholders.</span></span>

### <a name="from-a-visual-studio-project"></a><span data-ttu-id="9ad17-229">来自 Visual Studio 项目</span><span class="sxs-lookup"><span data-stu-id="9ad17-229">From a Visual Studio project</span></span>

<span data-ttu-id="9ad17-230">从 `.csproj` 或 `.vbproj` 文件创建 `.nuspec` 较为方便，因为其他已安装到这些项目的包会自动引用为依赖项。</span><span class="sxs-lookup"><span data-stu-id="9ad17-230">Creating a `.nuspec` from a `.csproj` or `.vbproj` file is convenient because other packages that have been installed into those project are automatically referenced as dependencies.</span></span> <span data-ttu-id="9ad17-231">只需使用与项目文件相同的文件夹中的命令：</span><span class="sxs-lookup"><span data-stu-id="9ad17-231">Simply use the following command in the same folder as the project file:</span></span>

```
# Use in a folder containing a project file <project-name>.csproj or <project-name>.vbproj
nuget spec
```

<span data-ttu-id="9ad17-232">生成的 `<project-name>.nuspec` 文件包含在打包时替换为项目值的令牌，包含对所有其他已安装的包的引用。</span><span class="sxs-lookup"><span data-stu-id="9ad17-232">The resulting `<project-name>.nuspec` file contains *tokens* that are replaced at packaging time with values from the project, including references to any other packages that have already been installed.</span></span>

<span data-ttu-id="9ad17-233">令牌在项目属性的两侧由 `$` 符号分隔。</span><span class="sxs-lookup"><span data-stu-id="9ad17-233">A token is delimited by `$` symbols on both sides of the project property.</span></span> <span data-ttu-id="9ad17-234">例如，通过此方法生成的清单中的 `<id>` 值通常如下所示：</span><span class="sxs-lookup"><span data-stu-id="9ad17-234">For example, the `<id>` value in a manifest generated in this way typically appears as follows:</span></span>

```xml
<id>$id$</id>
```

<span data-ttu-id="9ad17-235">此令牌在打包时替换为项目文件的 `AssemblyName` 值。</span><span class="sxs-lookup"><span data-stu-id="9ad17-235">This token is replaced with the `AssemblyName` value from the project file at packing time.</span></span> <span data-ttu-id="9ad17-236">有关项目值到 `.nuspec` 令牌的确切映射，请参阅[替换令牌引用](../schema/nuspec.md#replacement-tokens)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-236">For the exact mapping of project values to `.nuspec` tokens, see the [Replacement Tokens reference](../schema/nuspec.md#replacement-tokens).</span></span>

<span data-ttu-id="9ad17-237">借助令牌，更新项目时可以不再需要更新关键值，例如 `.nuspec` 中的版本号。</span><span class="sxs-lookup"><span data-stu-id="9ad17-237">Tokens relieve you from needing to update crucial values like the version number in the `.nuspec` as you update the project.</span></span> <span data-ttu-id="9ad17-238">（如果需要，随时可以使用文本值替换令牌）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-238">(You can always replace the tokens with literal values, if desired).</span></span> 

<span data-ttu-id="9ad17-239">注意在 Visual Studio 项目中工作时，有多个可用的其他打包选项，如稍后的[运行 NuGet 包生成 .nupkg 文件](#running-nuget-pack-to-generate-the-nupkg-file)所述。</span><span class="sxs-lookup"><span data-stu-id="9ad17-239">Note that there are several additional packaging options available when working from a Visual Studio project, as described in [Running nuget pack to generate the .nupkg file](#running-nuget-pack-to-generate-the-nupkg-file) later on.</span></span>

#### <a name="solution-level-packages"></a><span data-ttu-id="9ad17-240">解决方案级别包</span><span class="sxs-lookup"><span data-stu-id="9ad17-240">Solution-level packages</span></span>

<span data-ttu-id="9ad17-241">*仅适用于 NuGet 2.x。在 NuGet 3.0+ 中不可用。*</span><span class="sxs-lookup"><span data-stu-id="9ad17-241">*NuGet 2.x only. Not available in NuGet 3.0+.*</span></span>

<span data-ttu-id="9ad17-242">NuGet 2.x 支持为包管理器控制台（`tools` 文件夹的内容）安装工具或其他命令的解决方案级别包的概念，但是不会将引用、内容或生成自定义添加到解决方案中的任何项目中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-242">NuGet 2.x supported the notion of a solution-level package that installs tools or additional commands for the Package Manager Console (the contents of the `tools` folder), but does not add references, content, or build customizations to any projects in the solution.</span></span> <span data-ttu-id="9ad17-243">该包在其直接 `lib`、`content` 或 `build` 文件夹中未包含文件，并且它的依赖项各自的 `lib`、`content` 或 `build` 文件夹中也没有文件。</span><span class="sxs-lookup"><span data-stu-id="9ad17-243">Such packages contain no files in its direct `lib`, `content`, or `build` folders, and none of its dependencies have files in their respective `lib`, `content`, or `build` folders.</span></span> 

<span data-ttu-id="9ad17-244">NuGet 在 `.nuget` 文件夹的 `packages.config` 文件（而不是项目的 `packages.config` 文件）中，跟踪安装的解决方案级别包。</span><span class="sxs-lookup"><span data-stu-id="9ad17-244">NuGet tracks installed solution-level packages in a `packages.config` file in the `.nuget` folder, rather than the project's `packages.config` file.</span></span>

### <a name="new-file-with-default-values"></a><span data-ttu-id="9ad17-245">包含默认值的新文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-245">New file with default values</span></span>

<span data-ttu-id="9ad17-246">以下命令使用占位符创建默认清单，这可确保一开始就使用正确的文件结构：</span><span class="sxs-lookup"><span data-stu-id="9ad17-246">The following command creates a default manifest with placeholders, which ensures you start with the proper file structure:</span></span>

```
nuget spec [<package-name>]
```

<span data-ttu-id="9ad17-247">如果忽略 \<package-name\>，生成的文件是 `Package.nuspec`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-247">If you omit \<package-name\>, the resulting file is `Package.nuspec`.</span></span> <span data-ttu-id="9ad17-248">如果提供 `Contoso.Utility.UsefulStuff` 等名称，则文件是 `Contoso.Utility.UsefulStuff.nuspec`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-248">If you provide a name such as `Contoso.Utility.UsefulStuff`, the file is `Contoso.Utility.UsefulStuff.nuspec`.</span></span>

<span data-ttu-id="9ad17-249">生成的 `.nuspec` 包含值的占位符（例如 `projectUrl`）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-249">The resulting `.nuspec` contains placeholders for values like the `projectUrl`.</span></span> <span data-ttu-id="9ad17-250">请确保在使用文件创建最终 `.nupkg` 文件前编辑该文件。</span><span class="sxs-lookup"><span data-stu-id="9ad17-250">Be sure to edit the file before using it to create the final `.nupkg` file.</span></span>

## <a name="choosing-a-unique-package-identifier-and-setting-the-version-number"></a><span data-ttu-id="9ad17-251">选择唯一包标识符并设置版本号</span><span class="sxs-lookup"><span data-stu-id="9ad17-251">Choosing a unique package identifier and setting the version number</span></span>

<span data-ttu-id="9ad17-252">包标识符（`<id>` 元素）和版本号（`<version>` 元素）是清单中最重要的两个值，因为它们唯一标识包中包含的确切代码。</span><span class="sxs-lookup"><span data-stu-id="9ad17-252">The package identifier (`<id>` element) and the version number (`<version>` element) are the two most important values in the manifest because they uniquely identify the exact code that's contained in the package.</span></span>

<span data-ttu-id="9ad17-253">**针对包标识符的最佳做法：**</span><span class="sxs-lookup"><span data-stu-id="9ad17-253">**Best practices for the package identifier:**</span></span>

- <span data-ttu-id="9ad17-254">**唯一性**：标识符在 nuget.org 或承载包的任意库中必须是唯一的。</span><span class="sxs-lookup"><span data-stu-id="9ad17-254">**Uniqueness**: The identifier must be unique across nuget.org or whatever gallery hosts the package.</span></span> <span data-ttu-id="9ad17-255">确定标识符之前，请搜索适用库以检查该名称是否已使用。</span><span class="sxs-lookup"><span data-stu-id="9ad17-255">Before deciding on an identifier, search the applicable gallery to check if the name is already in use.</span></span> <span data-ttu-id="9ad17-256">为了避免冲突，最好使用公司名作为标识符的第一部分（例如 `Contoso.`）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-256">To avoid conflicts, a good pattern is to use your company name as the first part of the identifier, such as `Contoso.`.</span></span>
- <span data-ttu-id="9ad17-257">**类似命名空间的名称**：遵循类似于 .NET 中命名空间的模式，使用点表示法而不是连字符。</span><span class="sxs-lookup"><span data-stu-id="9ad17-257">**Namespace-like names**: Follow a pattern similar to namespaces in .NET, using dot notation instead of hyphens.</span></span> <span data-ttu-id="9ad17-258">例如，使用 `Contoso.Utility.UsefulStuff` 而不是 `Contoso-Utility-UsefulStuff` 或 `Contoso_Utility_UsefulStuff`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-258">For example, use `Contoso.Utility.UsefulStuff` rather than `Contoso-Utility-UsefulStuff` or `Contoso_Utility_UsefulStuff`.</span></span> <span data-ttu-id="9ad17-259">当包标识符与代码中使用的命名空间相匹配时，这个方法也很有用。</span><span class="sxs-lookup"><span data-stu-id="9ad17-259">Consumers also find it helpful when the package identifier matches the namespaces used in the code.</span></span>
- <span data-ttu-id="9ad17-260">**示例包**：如果生成演示如何使用另一个包的示例代码的包，则附加 `.Sample` 作为标识符的后缀，与 `Contoso.Utility.UsefulStuff.Sample` 中相似。</span><span class="sxs-lookup"><span data-stu-id="9ad17-260">**Sample Packages**: If you produce a package of sample code that demonstrates how to use another package, attach `.Sample` as a suffix to the identifier, as in `Contoso.Utility.UsefulStuff.Sample`.</span></span> <span data-ttu-id="9ad17-261">（当然，示例包会在其他包上有依赖项。）创建示例包时，使用前面介绍的基于约定的工作目录方法。</span><span class="sxs-lookup"><span data-stu-id="9ad17-261">(The sample package would of course have a dependency on the other package.) When creating a sample package, use the convention-based working directory method described earlier.</span></span> <span data-ttu-id="9ad17-262">在 `content` 文件夹中，在名为 `\Samples\<identifier>` 的文件夹中排列示例代码，与 `\Samples\Contoso.Utility.UsefulStuff.Sample` 中相似。</span><span class="sxs-lookup"><span data-stu-id="9ad17-262">In the `content` folder, arrange the sample code in a folder called `\Samples\<identifier>` as in `\Samples\Contoso.Utility.UsefulStuff.Sample`.</span></span>

<span data-ttu-id="9ad17-263">**针对包版本的最佳做法：**</span><span class="sxs-lookup"><span data-stu-id="9ad17-263">**Best practices for the package version:**</span></span>

- <span data-ttu-id="9ad17-264">一般情况下，将包的版本设置为与库相匹配，但这不是必须的。</span><span class="sxs-lookup"><span data-stu-id="9ad17-264">In general, set the version of the package to match the library, though this is not strictly required.</span></span> <span data-ttu-id="9ad17-265">如之前在[确定要打包的程序集](#deciding-which-assemblies-to-package)中所述，将包限制到单个程序集是一个简单的问题。</span><span class="sxs-lookup"><span data-stu-id="9ad17-265">This is a simple matter when you limit a package to a single assembly, as described earlier in [Deciding which assemblies to package](#deciding-which-assemblies-to-package).</span></span> <span data-ttu-id="9ad17-266">总的来说，请记住解析依赖项时，NuGet 自己处理包版本而不是程序集版本。</span><span class="sxs-lookup"><span data-stu-id="9ad17-266">Overall, remember that NuGet itself deals with package versions when resolving dependencies, not assembly versions.</span></span>
- <span data-ttu-id="9ad17-267">使用非标准版本方案时，请确保考虑使用 NuGet 版本控制规则，如[包版本控制](../reference/package-versioning.md)中所述。</span><span class="sxs-lookup"><span data-stu-id="9ad17-267">When using a non-standard version scheme, be sure to consider the NuGet versioning rules as explained in [Package versioning](../reference/package-versioning.md).</span></span>

> <span data-ttu-id="9ad17-268">以下一系列简短博客文章也有助于理解版本控制：</span><span class="sxs-lookup"><span data-stu-id="9ad17-268">The following series of brief blog posts are also helpful to understand versioning:</span></span>
>
> - [<span data-ttu-id="9ad17-269">第 1 部分：解决 DLL Hell</span><span class="sxs-lookup"><span data-stu-id="9ad17-269">Part 1: Taking on DLL Hell</span></span>](http://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)
> - [<span data-ttu-id="9ad17-270">第 2 部分：核心算法</span><span class="sxs-lookup"><span data-stu-id="9ad17-270">Part 2: The core algorithm</span></span>](http://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)
> - [<span data-ttu-id="9ad17-271">第 3 部分：通过绑定重定向统一</span><span class="sxs-lookup"><span data-stu-id="9ad17-271">Part 3: Unification via Binding Redirects</span></span>](http://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)

## <a name="setting-a-package-type"></a><span data-ttu-id="9ad17-272">设置包类型</span><span class="sxs-lookup"><span data-stu-id="9ad17-272">Setting a package type</span></span>

<span data-ttu-id="9ad17-273">通过 NuGet 3.5+ 可以使用特定的包类型标记包以指示其预期用途。</span><span class="sxs-lookup"><span data-stu-id="9ad17-273">With NuGet 3.5+, packages can be marked with a specific *package type* to indicate its intended use.</span></span> <span data-ttu-id="9ad17-274">未标记类型的包（包含使用更早版本的 NuGet 创建的所有包）默认为 `Dependency` 类型。</span><span class="sxs-lookup"><span data-stu-id="9ad17-274">Packages not marked with a type, including all packages created with earlier versions of NuGet, default to the `Dependency` type.</span></span>

- <span data-ttu-id="9ad17-275">`Dependency` 类型包将生成或运行时资产添加到库和应用程序，并且可以在任何项目类型中安装（假设它们相互兼容）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-275">`Dependency` type packages add build- or run-time assets to libraries and applications, and can be installed in any project type (assuming they are compatible).</span></span>

- <span data-ttu-id="9ad17-276">`DotnetCliTool` 类型包是 [.NET CLI](https://docs.microsoft.com/dotnet/articles/core/tools/index) 的扩展，并且从命令行中调用。</span><span class="sxs-lookup"><span data-stu-id="9ad17-276">`DotnetCliTool` type packages are extensions to the [.NET CLI](https://docs.microsoft.com/dotnet/articles/core/tools/index) and are invoked from the command line.</span></span> <span data-ttu-id="9ad17-277">该包仅在 .NET Core 项目中安装并且不影响还原操作。</span><span class="sxs-lookup"><span data-stu-id="9ad17-277">Such packages can be installed only in .NET Core projects and have no effect on restore operations.</span></span> <span data-ttu-id="9ad17-278">[.NET Core 扩展性](https://docs.microsoft.com/dotnet/articles/core/tools/extensibility#per-project-based-extensibility)文档中提供更多有关这些项目扩展的详细信息。</span><span class="sxs-lookup"><span data-stu-id="9ad17-278">More details about these per-project extensions are available in the  [.NET Core extensibility](https://docs.microsoft.com/dotnet/articles/core/tools/extensibility#per-project-based-extensibility) documentation.</span></span>

    <span data-ttu-id="9ad17-279">安装 DotnetCliTool 包时，Visual Studio 将包置于 `project.json` `tools` 节点而不是 `dependencies` 节点中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-279">When a DotnetCliTool package is installed, Visual Studio places the package in the `project.json` `tools` node instead of the `dependencies` node.</span></span>

- <span data-ttu-id="9ad17-280">自定义类型包使用与包 ID 遵守相同格式规则的任意类型标识符。</span><span class="sxs-lookup"><span data-stu-id="9ad17-280">Custom type packages use an arbitrary type identifier that conforms to the same format rules as package IDs.</span></span> <span data-ttu-id="9ad17-281">但是，任何不是 `Dependency` 和 `DotnetCliTool` 的类型不会被 Visual Studio 中的 NuGet 包管理器识别。</span><span class="sxs-lookup"><span data-stu-id="9ad17-281">Any type other than `Dependency` and `DotnetCliTool`, however, are not recognized by the NuGet Package Manager in Visual Studio.</span></span>

<span data-ttu-id="9ad17-282">包类型在 `.nuspec` 文件或 `project.json` 中设置。</span><span class="sxs-lookup"><span data-stu-id="9ad17-282">Package types are set either in the `.nuspec` file or in `project.json`.</span></span> <span data-ttu-id="9ad17-283">在这两种情况下，后向兼容最好不显式设置 `Dependency` 类型，而是依赖 NuGet 在没有指定类型时假设此类型。</span><span class="sxs-lookup"><span data-stu-id="9ad17-283">In both cases, it's best for backwards compatibility to *not* explicitly set the `Dependency` type and to instead rely on NuGet assuming this type when no type is specified.</span></span>

- <span data-ttu-id="9ad17-284">`.nuspec`：指示 `<metadata>` 元素的 `packageTypes\packageType` 节点中的包类型：</span><span class="sxs-lookup"><span data-stu-id="9ad17-284">`.nuspec`: Indicate the package type within a `packageTypes\packageType` node under the `<metadata>` element:</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
        <metadata>
        <!-- ... -->
        <packageTypes>
            <packageType name="DotnetCliTool" />
        </packageTypes>
        </metadata>
    </package>
    ```

- <span data-ttu-id="9ad17-285">`project.json`：指示 `packOptions.packageType` 属性 json 中的包类型：</span><span class="sxs-lookup"><span data-stu-id="9ad17-285">`project.json`: Indicate the package type within a `packOptions.packageType` property json:</span></span>

    ```json
    {
        // ...
        "packOptions": {
        "packageType": "DotnetCliTool"
        }
    }
    ```

## <a name="adding-a-readme-and-other-files"></a><span data-ttu-id="9ad17-286">添加自述文件和其他文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-286">Adding a readme and other files</span></span>

<span data-ttu-id="9ad17-287">若要直接指定要包含在包中的文件，请使用 `.nuspec` 中的 `<files>` 节点，其遵循 `<metadata>` 标记：</span><span class="sxs-lookup"><span data-stu-id="9ad17-287">To directly specify files to include in the package, use the `<files>` node in the `.nuspec` file, which *follows* the `<metadata>` tag:</span></span>

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
    <metadata>
    <!-- ... -->
    </metadata>
    <files>
    <!-- Add a readme -->
    <file src="readme.txt" target="" />

    <!-- Add files from an arbitrary folder that's not necessarily in the project -->
    <file src="..\..\SomeRoot\**\*.*" target="" />
    </files>
</package>
```

> [!Tip]
> <span data-ttu-id="9ad17-288">使用基于约定的工作目录方法时，可以将 readme.txt 置于包根目录中并将其他内容置于 `content` 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-288">When using the convention-based working directory approach, you can place the readme.txt in the package root and other content in the `content` folder.</span></span> <span data-ttu-id="9ad17-289">清单中不需要 `<file>` 元素。</span><span class="sxs-lookup"><span data-stu-id="9ad17-289">No `<file>` elements are necessary in the manifest.</span></span>

<span data-ttu-id="9ad17-290">如果包根目录中包含名为 `readme.txt` 的文件，Visual Studio 在直接安装包之后立即将该文件的内容显示为纯文本。</span><span class="sxs-lookup"><span data-stu-id="9ad17-290">When you include a file named `readme.txt` in the package root, Visual Studio displays the contents of that file as plain text immediately after installing the package directly.</span></span> <span data-ttu-id="9ad17-291">（对于安装为依赖项的包，不会显示自述文件）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-291">(Readme files are not displayed for packages installed as dependencies).</span></span> <span data-ttu-id="9ad17-292">例如，下面是 HtmlAgilityPack 包的自述文件的显示方式：</span><span class="sxs-lookup"><span data-stu-id="9ad17-292">For example, here's how the readme for the HtmlAgilityPack package appears:</span></span>

![安装时 NuGet 包的自述文件的显示](media/Create_01-ShowReadme.png)

> [!Note]
> <span data-ttu-id="9ad17-294">如果 `.nuspec` 文件中包含空 `<files>` 节点，则 NuGet 不会在包中包含除 `lib` 文件夹中的内容以外的任何其他内容。</span><span class="sxs-lookup"><span data-stu-id="9ad17-294">If you include an empty `<files>` node in the `.nuspec` file, NuGet doesn't include any other content in the package other than what's in the `lib` folder.</span></span>

## <a name="including-msbuild-props-and-targets-in-a-package"></a><span data-ttu-id="9ad17-295">包含包中的 MSBuild 属性和目标</span><span class="sxs-lookup"><span data-stu-id="9ad17-295">Including MSBuild props and targets in a package</span></span>

<span data-ttu-id="9ad17-296">在某些情况下，可能需要在使用包的项目中添加自定义生成目标或属性，例如生成期间运行自定义工具或进程。</span><span class="sxs-lookup"><span data-stu-id="9ad17-296">In some cases, you might want to add custom build targets or properties in projects that consume your package, such as running a custom tool or process during build.</span></span> <span data-ttu-id="9ad17-297">通过将文件置于项目的 `\build` 文件夹中的窗体 `<package_id>.targets` 或 `<package_id>.props`（例如 `Contoso.Utility.UsefulStuff.targets`）中执行此操作。</span><span class="sxs-lookup"><span data-stu-id="9ad17-297">You do this by placing files in the form `<package_id>.targets` or `<package_id>.props` (such as `Contoso.Utility.UsefulStuff.targets`) within the `\build` folder of the project.</span></span>

<span data-ttu-id="9ad17-298">根 `\build` 文件夹中的文件被视为适用于所有目标框架。</span><span class="sxs-lookup"><span data-stu-id="9ad17-298">Files in the root `\build` folder are considered suitable for all target frameworks.</span></span> <span data-ttu-id="9ad17-299">若要提供特定于框架的文件，首先将其放置到合适的子文件夹中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9ad17-299">To provide framework-specific files, first place those within appropriate subfolders, such as the following:</span></span>

    \build
        \netstandard1.4
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
        \net462
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets

<span data-ttu-id="9ad17-300">然后在 `.nuspec` 文件中，确保参阅 `<files>` 节点中的这些文件：</span><span class="sxs-lookup"><span data-stu-id="9ad17-300">Then in the `.nuspec` file, be sure to refer to these files in the `<files>` node:</span></span>

```xml
<?xml version="1.0"?>
<package >
    <metadata>
    <!-- ... -->
    </metadata>
    <files>
    <!-- Include everything in \build -->
    <file src="build\**" target="build" />

    <!-- Other files -->
    <!-- ... -->
    </files>
</package>
```

<span data-ttu-id="9ad17-301">当 NuGet 2.x 使用 `\build` 文件安装包时，它在指向 `.targets` 和 `.props` 文件的项目文件中添加 MSBuild `<Import>` 元素。</span><span class="sxs-lookup"><span data-stu-id="9ad17-301">When NuGet 2.x installs a package with `\build` files, it adds an MSBuild `<Import>` elements in the project file pointing to the `.targets` and `.props` files.</span></span> <span data-ttu-id="9ad17-302">（`.props` 添加到项目文件顶部；`.targets` 添加到底部。）</span><span class="sxs-lookup"><span data-stu-id="9ad17-302">(`.props` is added at the top of the project file; `.targets` is added at the bottom.)</span></span>

<span data-ttu-id="9ad17-303">对于 NuGet 3.x，目标不添加到项目，而是通过 `project.lock.json` 提供。</span><span class="sxs-lookup"><span data-stu-id="9ad17-303">With NuGet 3.x, targets are not added to the project but are instead made available through the `project.lock.json`.</span></span>

## <a name="authoring-packages-with-com-interop-assemblies"></a><span data-ttu-id="9ad17-304">使用 COM 互操作程序集创作包</span><span class="sxs-lookup"><span data-stu-id="9ad17-304">Authoring packages with COM interop assemblies</span></span>

<span data-ttu-id="9ad17-305">包含 COM 互操作程序集的包必须包含合适的[目标文件](#including-msbuild-props-and-targets-in-a-package)，使正确的 `EmbedInteropTypes` 元数据使用 PackageReference 格式添加到项目。</span><span class="sxs-lookup"><span data-stu-id="9ad17-305">Packages that contain COM interop assemblies must include an appropriate [targets file](#including-msbuild-props-and-targets-in-a-package) so that the correct `EmbedInteropTypes` metadata is added to projects using the PackageReference format.</span></span> <span data-ttu-id="9ad17-306">默认情况下，使用 PackageReference 时，`EmbedInteropTypes` 元数据对所有程序集一直为 false，所以目标文件显式添加此元数据。</span><span class="sxs-lookup"><span data-stu-id="9ad17-306">By default, the `EmbedInteropTypes` metadata is always false for all assemblies when PackageReference is used, so the targets file adds this metadata explicitly.</span></span> <span data-ttu-id="9ad17-307">为了避免冲突，目标名称必须是唯一的；理想情况下，使用包名称和嵌入程序集的组合，使用此值替换下例中的 `{InteropAssemblyName}`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-307">To avoid conflicts, the target name should be unique; ideally, use a combination of your package name and the assembly being embedded, replacing the `{InteropAssemblyName}` in the example below with that value.</span></span> <span data-ttu-id="9ad17-308">（有关示例，另请参阅 [NuGet.Samples.Interop](https://github.com/NuGet/Samples/tree/master/NuGet.Samples.Interop)。）</span><span class="sxs-lookup"><span data-stu-id="9ad17-308">(Also see [NuGet.Samples.Interop](https://github.com/NuGet/Samples/tree/master/NuGet.Samples.Interop) for an example.)</span></span>

```xml      
<Target Name="EmbeddingAssemblyNameFromPackageId" AfterTargets="ResolveReferences" BeforeTargets="FindReferenceAssembliesForReferences">
  <PropertyGroup>
    <_InteropAssemblyFileName>{InteropAssemblyName}</_InteropAssemblyFileName>
  </PropertyGroup>
  <ItemGroup>
    <ReferencePath Condition=" '%(FileName)' == '$(_InteropAssemblyFileName)' AND '%(ReferencePath.NuGetPackageId)' == '$(MSBuildThisFileName)' ">
      <EmbedInteropTypes>true</EmbedInteropTypes>
    </ReferencePath>
  </ItemGroup>
</Target>
```

<span data-ttu-id="9ad17-309">请注意，在使用 `packages.config` 引用格式时，将引用从包添加到程序集会导致 NuGet 和 Visual Studio 检查 COM 互操作程序集并在项目文件中将 `EmbedInteropTypes` 设为 true。</span><span class="sxs-lookup"><span data-stu-id="9ad17-309">Note that when using the `packages.config` reference format, adding references to the assemblies from the packages causes NuGet and Visual Studio to check for COM interop assemblies and set the `EmbedInteropTypes` to true in the project file.</span></span> <span data-ttu-id="9ad17-310">在这种情况下，目标被替代。</span><span class="sxs-lookup"><span data-stu-id="9ad17-310">In this case the targets are overriden.</span></span>

<span data-ttu-id="9ad17-311">此外，默认情况下，[生成资产不以可传递的方式流动](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-311">Additionally, by default the [build assets do not flow transitively](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets).</span></span> <span data-ttu-id="9ad17-312">当此处所述的创作的包作为可传递的依赖项从项目拉取到项目引用时，它们会以不同的方式工作。</span><span class="sxs-lookup"><span data-stu-id="9ad17-312">Packages authored as described here work differently when they are pulled as a transitive dependency from a project to project reference.</span></span> <span data-ttu-id="9ad17-313">包使用者通过将 PrivateAssets 默认值修改为不包含生成使其流动。</span><span class="sxs-lookup"><span data-stu-id="9ad17-313">The package consumer can allow them to flow by modifying the PrivateAssets default value to not include build.</span></span>  

<a name="creating-the-package"></a>

## <a name="running-nuget-pack-to-generate-the-nupkg-file"></a><span data-ttu-id="9ad17-314">运行 NuGet 包生成 .nupkg 文件</span><span class="sxs-lookup"><span data-stu-id="9ad17-314">Running nuget pack to generate the .nupkg file</span></span>

<span data-ttu-id="9ad17-315">使用程序集或基于约定的工作目录时，通过使用 `.nuspec` 文件运行 `nuget pack` 创建包，使用特定的文件名替换 `<manifest-name>`：</span><span class="sxs-lookup"><span data-stu-id="9ad17-315">When using an assembly or the convention-based working directory, create a package by running `nuget pack` with your `.nuspec` file, replacing `<manifest-name>` with your specific filename:</span></span>

```
nuget pack <project-name>.nuspec
```

<span data-ttu-id="9ad17-316">使用 Visual Studio 项目时，使用项目文件运行 `nuget pack`，该项目文件自动加载项目的 `.nuspec` 文件并使用项目文件中的值替换其中的任意令牌：</span><span class="sxs-lookup"><span data-stu-id="9ad17-316">When using a Visual Studio project, run `nuget pack` with your project file, which automatically loads the project's `.nuspec` file and replaces any tokens within it using values in the project file:</span></span>

```
nuget pack <project-name>.csproj
```

> [!Note]
> <span data-ttu-id="9ad17-317">令牌替换需要直接使用项目文件，因为项目是令牌值的源。</span><span class="sxs-lookup"><span data-stu-id="9ad17-317">Using the project file directly is necessary for token replacement because the project is the source of the token values.</span></span> <span data-ttu-id="9ad17-318">如果将 `nuget pack` 用于 `.nuspec` 文件，不会发生令牌替换。</span><span class="sxs-lookup"><span data-stu-id="9ad17-318">Token replacement does not happen if you use `nuget pack` with a `.nuspec` file.</span></span>

<span data-ttu-id="9ad17-319">在所有情况下，`nuget pack` 不包含句点开头的文件夹，例如 `.git` 或 `.hg`。</span><span class="sxs-lookup"><span data-stu-id="9ad17-319">In all cases, `nuget pack` excludes folders that start with a period, such as `.git` or `.hg`.</span></span>

<span data-ttu-id="9ad17-320">NuGet 指示需要更正的 `.nuspec` 文件中是否有错误，例如忘记更改清单中的占位符值。</span><span class="sxs-lookup"><span data-stu-id="9ad17-320">NuGet indicates if there are any errors in the `.nuspec` file that need correcting, such as forgetting to change placeholder values in the manifest.</span></span>

<span data-ttu-id="9ad17-321">一旦 `nuget pack` 成功，就有一个可以发布到合适库的 `.nupkg` 文件，如[发布包](../create-packages/publish-a-package.md)中所述。</span><span class="sxs-lookup"><span data-stu-id="9ad17-321">Once `nuget pack` succeeds, you have a `.nupkg` file that you can publish to a suitable gallery as described in [Publishing a Package](../create-packages/publish-a-package.md).</span></span>

> [!Tip]
> <span data-ttu-id="9ad17-322">一个检查创建后的包的有用方法是在[包资源管理器](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer)工具中打开它。</span><span class="sxs-lookup"><span data-stu-id="9ad17-322">A helpful way to examine a package after creating it is to open it in the [Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) tool.</span></span> <span data-ttu-id="9ad17-323">这会为你提供包内容及其清单的图形视图。</span><span class="sxs-lookup"><span data-stu-id="9ad17-323">This gives you a graphical view of the package contents and its manifest.</span></span> <span data-ttu-id="9ad17-324">还可以将生成的 `.nupkg` 文件重命名为 `.zip` 文件并直接浏览其内容。</span><span class="sxs-lookup"><span data-stu-id="9ad17-324">You can also rename the resulting `.nupkg` file to a `.zip` file and explore its contents directly.</span></span>

### <a name="additional-options"></a><span data-ttu-id="9ad17-325">附加选项</span><span class="sxs-lookup"><span data-stu-id="9ad17-325">Additional options</span></span>

<span data-ttu-id="9ad17-326">可通过 `nuget pack` 使用多种命令行开关，以排除文件、替代清单中的版本号和更改输出文件夹等。</span><span class="sxs-lookup"><span data-stu-id="9ad17-326">You can use various command-line switches with `nuget pack` to exclude files, override the version number in the manifest, and change the output folder, among other features.</span></span> <span data-ttu-id="9ad17-327">有关完整列表，请参考[包命令引用](../tools/cli-ref-pack.md)。</span><span class="sxs-lookup"><span data-stu-id="9ad17-327">For a complete list, refer to the [pack command reference](../tools/cli-ref-pack.md).</span></span>

<span data-ttu-id="9ad17-328">以下是 Visual Studio 项目中的一些常见选项：</span><span class="sxs-lookup"><span data-stu-id="9ad17-328">The following options are a few that are common with Visual Studio projects:</span></span>

- <span data-ttu-id="9ad17-329">**引用项目**：如果项目引用其他项目，通过使用 `-IncludeReferencedProjects` 选项，可以将引用项目添加为包的一部分或依赖项：</span><span class="sxs-lookup"><span data-stu-id="9ad17-329">**Referenced projects**: If the project references other projects, you can add the referenced projects as part of the package, or as dependencies, by using the `-IncludeReferencedProjects` option:</span></span>

    ```
    nuget pack MyProject.csproj -IncludeReferencedProjects
    ```

    <span data-ttu-id="9ad17-330">此包含过程是递归的，所以如果 `MyProject.csproj` 引用项目 B 和 C，且这些项目引用 D、E 和 F，那么 B、C、D、E 和 F 中的文件包含在包中。</span><span class="sxs-lookup"><span data-stu-id="9ad17-330">This inclusion process is recursive, so if `MyProject.csproj` references projects B and C, and those projects reference D, E, and F, then files from B, C, D, E, and F are included in the package.</span></span>

    <span data-ttu-id="9ad17-331">如果引用的项目包含其自身的 `.nuspec` 文件，那么 NuGet 将该引用的项目添加为依赖项。</span><span class="sxs-lookup"><span data-stu-id="9ad17-331">If a referenced project includes a `.nuspec` file of its own, then NuGet adds that referenced project as a dependency instead.</span></span>  <span data-ttu-id="9ad17-332">需要单独打包和发布该项目。</span><span class="sxs-lookup"><span data-stu-id="9ad17-332">You need to package and publish that project separately.</span></span>

- <span data-ttu-id="9ad17-333">**生成配置**：默认情况下，NuGet 使用项目文件中设置的默认生成配置，通常是“调试”。</span><span class="sxs-lookup"><span data-stu-id="9ad17-333">**Build configuration**: By default, NuGet uses the default build configuration set in the project file, typically *Debug*.</span></span> <span data-ttu-id="9ad17-334">若要从不同的生成配置（例如“发布”）中打包文件，使用包含以下配置的 `-properties` 选项：</span><span class="sxs-lookup"><span data-stu-id="9ad17-334">To pack files from a different build configuration, such as *Release*, use the `-properties` option with the configuration:</span></span>

    ```
    nuget pack MyProject.csproj -properties Configuration=Release
    ```

- <span data-ttu-id="9ad17-335">**符号**：若要包含允许使用者在调试器中单步执行包代码的符号，请使用 `-Symbols` 选项：</span><span class="sxs-lookup"><span data-stu-id="9ad17-335">**Symbols**: to include symbols that allow consumers to step through your package code in the debugger, use the `-Symbols` option:</span></span>

    ```
    nuget pack MyProject.csproj -symbols
    ```

### <a name="testing-package-installation"></a><span data-ttu-id="9ad17-336">测试包安装</span><span class="sxs-lookup"><span data-stu-id="9ad17-336">Testing package installation</span></span>

<span data-ttu-id="9ad17-337">发布包前，通常需要测试将包安装到项目的过程。</span><span class="sxs-lookup"><span data-stu-id="9ad17-337">Before publishing a package, you typically want to test the process of installing a package into a project.</span></span> <span data-ttu-id="9ad17-338">测试确保所有文件一定在项目中正确的位置结束。</span><span class="sxs-lookup"><span data-stu-id="9ad17-338">The tests make sure that the necessarily files all end up in their correct places in the project.</span></span>

<span data-ttu-id="9ad17-339">可以在 Visual Studio 中手动测试安装，或是使用常规[包安装步骤](../Quickstart/Use-a-Package.md)在命令行上测试。</span><span class="sxs-lookup"><span data-stu-id="9ad17-339">You can test installations manually in Visual Studio or on the command line using the normal [package installation steps](../Quickstart/Use-a-Package.md).</span></span>

<span data-ttu-id="9ad17-340">对于自动测试，基本流程如下所示：</span><span class="sxs-lookup"><span data-stu-id="9ad17-340">For automated testing, the basic process is as follows:</span></span>

1. <span data-ttu-id="9ad17-341">将 `.nupkg` 文件复制到本地文件夹。</span><span class="sxs-lookup"><span data-stu-id="9ad17-341">Copy the `.nupkg` file to a local folder.</span></span>
1. <span data-ttu-id="9ad17-342">使用 `nuget sources -name <name> -source <path>` 命令将文件夹添加到包源（请参阅 [nuget 源](../tools/cli-ref-sources.md)）。</span><span class="sxs-lookup"><span data-stu-id="9ad17-342">Add the folder to your package sources using the `nuget sources -name <name> -source <path>` command (see [nuget sources](../tools/cli-ref-sources.md)).</span></span> <span data-ttu-id="9ad17-343">请注意，只需在任意给定计算机上设置一次此本地源。</span><span class="sxs-lookup"><span data-stu-id="9ad17-343">Note that you need only set this local source once on any given computer.</span></span>
1. <span data-ttu-id="9ad17-344">使用 `nuget install <packageID> -source <name>`（其中 `<name>` 与提供给 `nuget sources` 的源名称相匹配）从该源安装包。</span><span class="sxs-lookup"><span data-stu-id="9ad17-344">Install the package from that source using `nuget install <packageID> -source <name>` where `<name>` matches the name of your source as given to `nuget sources`.</span></span> <span data-ttu-id="9ad17-345">指定源可确保包从该源中单独安装。</span><span class="sxs-lookup"><span data-stu-id="9ad17-345">Specifying the source ensures that the package is installed from that source alone.</span></span>
1. <span data-ttu-id="9ad17-346">检查文件系统以检查文件是否正确安装。</span><span class="sxs-lookup"><span data-stu-id="9ad17-346">Examine the file system to check that files are installed correctly.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ad17-347">后续步骤</span><span class="sxs-lookup"><span data-stu-id="9ad17-347">Next Steps</span></span>

<span data-ttu-id="9ad17-348">创建包（`.nupkg` 文件）后，可以将其发布到选择的库，如[发布包](../create-packages/publish-a-package.md)中所述。</span><span class="sxs-lookup"><span data-stu-id="9ad17-348">Once you've created a package, which is a `.nupkg` file, you can publish it to the gallery of your choice as described on [Publishing a Package](../create-packages/publish-a-package.md).</span></span>

<span data-ttu-id="9ad17-349">你可能还希望扩展包的功能，或者支持其他方案，如以下主题所述：</span><span class="sxs-lookup"><span data-stu-id="9ad17-349">You might also want to extend the capabilities of your package or otherwise support other scenarios as described in the following topics:</span></span>

- [<span data-ttu-id="9ad17-350">包版本控制</span><span class="sxs-lookup"><span data-stu-id="9ad17-350">Package versioning</span></span>](../reference/package-versioning.md)
- [<span data-ttu-id="9ad17-351">支持多个目标框架</span><span class="sxs-lookup"><span data-stu-id="9ad17-351">Supporting multiple target frameworks</span></span>](../create-packages/supporting-multiple-target-frameworks.md)
- [<span data-ttu-id="9ad17-352">源和配置文件的转换</span><span class="sxs-lookup"><span data-stu-id="9ad17-352">Transformations of source and configuration files</span></span>](../create-packages/source-and-config-file-transformations.md)
- [<span data-ttu-id="9ad17-353">本地化</span><span class="sxs-lookup"><span data-stu-id="9ad17-353">Localization</span></span>](../create-packages/creating-localized-packages.md)
- [<span data-ttu-id="9ad17-354">预发布版本</span><span class="sxs-lookup"><span data-stu-id="9ad17-354">Pre-release versions</span></span>](../create-packages/prerelease-packages.md)

<span data-ttu-id="9ad17-355">最后，有需要注意的其他包类型：</span><span class="sxs-lookup"><span data-stu-id="9ad17-355">Finally, there are additional package types to be aware of:</span></span>

- [<span data-ttu-id="9ad17-356">本机包</span><span class="sxs-lookup"><span data-stu-id="9ad17-356">Native Packages</span></span>](../create-packages/native-packages.md)
- [<span data-ttu-id="9ad17-357">符号包</span><span class="sxs-lookup"><span data-stu-id="9ad17-357">Symbol Packages</span></span>](../create-packages/symbol-packages.md)