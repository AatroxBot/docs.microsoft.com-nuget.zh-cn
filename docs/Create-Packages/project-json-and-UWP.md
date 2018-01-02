---
title: "NuGet project.json 文件和 UWP 项目 | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/17/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 37caf4d7-dabd-4a78-aad2-7d445f818457
description: "介绍如何使用 project.json 文件来跟踪通用 Windows 平台 (UWP) 项目中的 NuGet 依赖项。"
keywords: "NuGet 依赖项, NuGet 和 UWP, UWP 和 project.json, NuGet project.json 文件"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 40507e541997cea368052c373a4124d9c4a00a51
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2017
---
# <a name="projectjson-and-uwp"></a><span data-ttu-id="89485-104">project.json 和 UWP</span><span class="sxs-lookup"><span data-stu-id="89485-104">project.json and UWP</span></span>

<span data-ttu-id="89485-105">本文档介绍使用 NuGet 3+（Visual Studio 2015 及更高版本）中功能的包结构。</span><span class="sxs-lookup"><span data-stu-id="89485-105">This document describes the package structure that employs features in NuGet 3+ (Visual Studio 2015 and later).</span></span> <span data-ttu-id="89485-106">可以通过将 `.nuspec` 的 `minClientVersion` 设置为 3.1 来声明你需要此处介绍的功能。</span><span class="sxs-lookup"><span data-stu-id="89485-106">The `minClientVersion` property of your `.nuspec` can be used to state that you require the features described here by setting it to 3.1.</span></span>

## <a name="adding-uwp-support-to-an-existing-package"></a><span data-ttu-id="89485-107">向现有包添加 UWP 支持</span><span class="sxs-lookup"><span data-stu-id="89485-107">Adding UWP support to an existing package</span></span>

<span data-ttu-id="89485-108">如果有一个现有包，并且想添加对 UWP 应用程序的支持，则不需要采用此处介绍的打包格式。</span><span class="sxs-lookup"><span data-stu-id="89485-108">If you have an existing package and you want to add support for UWP applications, then you don’t need to adopt the  packaging format described here.</span></span> <span data-ttu-id="89485-109">如果需要此处介绍的功能并且只愿意使用已更新到 3+ 版 NuGet 的客户端，只需采用此格式即可。</span><span class="sxs-lookup"><span data-stu-id="89485-109">You only need to adopt this format if you require the features it describes and are willing to  work only with clients that have updated to version 3+ of the NuGet client.</span></span>

## <a name="i-already-target-netcore45"></a><span data-ttu-id="89485-110">已面向 netcore45</span><span class="sxs-lookup"><span data-stu-id="89485-110">I already target netcore45</span></span>

<span data-ttu-id="89485-111">如果已面向 `netcore45`，并且不需要利用此处介绍的功能，则无需执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="89485-111">If you target `netcore45` already, and you don’t need to take advantage of the features here, no action is needed.</span></span> <span data-ttu-id="89485-112">UWP 应用程序可以使用 `netcore45` 包。</span><span class="sxs-lookup"><span data-stu-id="89485-112">`netcore45` packages can be consumed by UWP applications.</span></span>

## <a name="i-want-to-take-advantage-of-windows-10-specific-apis"></a><span data-ttu-id="89485-113">希望利用 Windows 10 特定的 API</span><span class="sxs-lookup"><span data-stu-id="89485-113">I want to take advantage of Windows 10 specific APIs</span></span>

<span data-ttu-id="89485-114">在这种情况下，需要将 `uap10.0` 目标框架名字对象（TFM 或 TxM）添加到包。</span><span class="sxs-lookup"><span data-stu-id="89485-114">In this case you need to add the `uap10.0` target framework moniker (TFM or TxM) to your package.</span></span> <span data-ttu-id="89485-115">在包中创建一个新文件夹，并将已编译为与 Windows 10 一起使用的程序集添加到该文件夹中。</span><span class="sxs-lookup"><span data-stu-id="89485-115">Create a new folder in your package and add the assembly that has been compiled to work with Windows 10 to that folder.</span></span>

## <a name="i-dont-need-windows-10-specific-apis-but-want-new-net-features-or-dont-have-netcore45-already"></a><span data-ttu-id="89485-116">不需要 Windows 10 特定的 API，但需要新的 .NET 功能或者尚没有 netcore45</span><span class="sxs-lookup"><span data-stu-id="89485-116">I don’t need Windows 10 specific APIs, but want new .NET features or don’t have netcore45 already</span></span>

<span data-ttu-id="89485-117">在这种情况下，需要将 `dotnet` TxM 添加到包。</span><span class="sxs-lookup"><span data-stu-id="89485-117">In this case you would add the `dotnet` TxM to your package.</span></span> <span data-ttu-id="89485-118">与其他 TxM 不同，`dotnet` 并不意味着外围应用或平台。</span><span class="sxs-lookup"><span data-stu-id="89485-118">Unlike other TxMs, `dotnet` doesn't imply a surface area or platform.</span></span> <span data-ttu-id="89485-119">也就是说，包适用于依赖项适用的任何平台。</span><span class="sxs-lookup"><span data-stu-id="89485-119">It is stating that your package works on any platform that your dependencies work on.</span></span> <span data-ttu-id="89485-120">使用 `dotnet` TxM 生成包时，`.nuspec` 中可能存在许多 TxM 特定的依赖项，因为需要定义所依赖的 BCL 包，如 `System.Text`、`System.Xml` 等。这些依赖项适用的位置即是包的工作位置。</span><span class="sxs-lookup"><span data-stu-id="89485-120">When building a package with the `dotnet` TxM, you are likely to have many more TxM-specific dependencies in your `.nuspec`, as you need to define the BCL packages you depend on, such `System.Text`, `System.Xml`, etc. The locations that those dependencies work on define where your package works.</span></span>

### <a name="how-do-i-find-out-my-dependencies"></a><span data-ttu-id="89485-121">如何查找依赖项</span><span class="sxs-lookup"><span data-stu-id="89485-121">How do I find out my dependencies</span></span>

<span data-ttu-id="89485-122">有两种方法可以查找要列出的依赖项：</span><span class="sxs-lookup"><span data-stu-id="89485-122">There are two ways to figure out which dependencies to list:</span></span>

1. <span data-ttu-id="89485-123">使用 [NuSpec 依赖项生成器](https://github.com/onovotny/ReferenceGenerator)第三方工具。</span><span class="sxs-lookup"><span data-stu-id="89485-123">Use the [NuSpec Dependency Generator](https://github.com/onovotny/ReferenceGenerator) **3rd party** tool.</span></span> <span data-ttu-id="89485-124">该工具自动执行该过程，并使用生成时依赖的包更新 `.nuspec` 文件。</span><span class="sxs-lookup"><span data-stu-id="89485-124">The tool automates the process and updates your `.nuspec` file with the dependant packages on build.</span></span> <span data-ttu-id="89485-125">该工具通过 NuGet 包 [NuSpec.ReferenceGenerator](https://www.nuget.org/packages/NuSpec.ReferenceGenerator/) 提供。</span><span class="sxs-lookup"><span data-stu-id="89485-125">It is available via a NuGet package, [NuSpec.ReferenceGenerator](https://www.nuget.org/packages/NuSpec.ReferenceGenerator/).</span></span>
2. <span data-ttu-id="89485-126">（复杂方法）使用 `ILDasm` 检查 `.dll`，查看运行时实际需要的程序集。</span><span class="sxs-lookup"><span data-stu-id="89485-126">(The hard way) Use `ILDasm` to look at your `.dll` to see what assemblies are actually needed at runtime.</span></span> <span data-ttu-id="89485-127">然后确定这些程序集分别来自哪个 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="89485-127">Then determine which NuGet package they each come from.</span></span>

<span data-ttu-id="89485-128">请参阅 [`project.json`](../Schema/project-json.md) 主题，详细了解可帮助创建支持 `dotnet` TxM 的包的功能。</span><span class="sxs-lookup"><span data-stu-id="89485-128">See the [`project.json`](../Schema/project-json.md) topic for details on features that help in the creation of a package that supports the `dotnet` TxM.</span></span>

> [!Important]
> <span data-ttu-id="89485-129">如果打算将包用于 PCL 项目，强烈建议创建一个 `dotnet` 文件夹，以避免警告和潜在的兼容性问题。</span><span class="sxs-lookup"><span data-stu-id="89485-129">If your package is intended to work with PCL projects, we highly recommend to create a `dotnet` folder, to avoid warnings and potential compatibility issues.</span></span>


## <a name="directory-structure"></a><span data-ttu-id="89485-130">目录结构</span><span class="sxs-lookup"><span data-stu-id="89485-130">Directory structure</span></span>

<span data-ttu-id="89485-131">使用此格式的 NuGet 包具有以下已知文件夹和行为：</span><span class="sxs-lookup"><span data-stu-id="89485-131">NuGet packages using this format have the following well-known folder and behaviors:</span></span>

| <span data-ttu-id="89485-132">文件夹</span><span class="sxs-lookup"><span data-stu-id="89485-132">Folder</span></span> | <span data-ttu-id="89485-133">行为</span><span class="sxs-lookup"><span data-stu-id="89485-133">Behaviors</span></span> |
| --- | --- |
| <span data-ttu-id="89485-134">生成</span><span class="sxs-lookup"><span data-stu-id="89485-134">Build</span></span> | <span data-ttu-id="89485-135">此文件夹中包含 MSBuild 目标和属性文件将以不同的方式集成到项目中，但除此之外没有更改。</span><span class="sxs-lookup"><span data-stu-id="89485-135">Contains MSBuild targets and props files in this folder are integrated differently into the project, but otherwise there is no change.</span></span> |
| <span data-ttu-id="89485-136">工具</span><span class="sxs-lookup"><span data-stu-id="89485-136">Tools</span></span> | <span data-ttu-id="89485-137">`install.ps1` 和 `uninstall.ps1` 不运行。</span><span class="sxs-lookup"><span data-stu-id="89485-137">`install.ps1` and `uninstall.ps1` are not run.</span></span> <span data-ttu-id="89485-138">`init.ps1` 像往常一样工作。</span><span class="sxs-lookup"><span data-stu-id="89485-138">`init.ps1` works as it always has.</span></span> |
| <span data-ttu-id="89485-139">内容</span><span class="sxs-lookup"><span data-stu-id="89485-139">Content</span></span> | <span data-ttu-id="89485-140">内容不会自动复制到用户的项目中。</span><span class="sxs-lookup"><span data-stu-id="89485-140">Content is not copied automatically into a user's project.</span></span> <span data-ttu-id="89485-141">后续发行版本计划支持将内容包含在项目中。</span><span class="sxs-lookup"><span data-stu-id="89485-141">Support for content inclusion in the project is planned for a later release.</span></span> |
| <span data-ttu-id="89485-142">Lib</span><span class="sxs-lookup"><span data-stu-id="89485-142">Lib</span></span> | <span data-ttu-id="89485-143">对于许多包而言，`lib` 的工作方式与 NuGet 2.x 相同，只不过其中使用的名称具有扩展选项，并且在使用包时具有更好的逻辑来选取正确的子文件夹。</span><span class="sxs-lookup"><span data-stu-id="89485-143">For many packages the `lib` works the same way it does in NuGet 2.x, but with expanded options for what names can be used inside it and better logic for picking the correct sub-folder when consuming packages.</span></span> <span data-ttu-id="89485-144">但是，与 `ref` 一起使用时，`lib` 文件夹包含如下程序集：实现由 `ref` 文件夹中的程序集定义的外围应用。</span><span class="sxs-lookup"><span data-stu-id="89485-144">However, when used in conjunction with `ref`, the `lib` folder contains assemblies that implement the surface area defined by the assemblies in the `ref` folder.</span></span> |
| <span data-ttu-id="89485-145">引用</span><span class="sxs-lookup"><span data-stu-id="89485-145">Ref</span></span> | <span data-ttu-id="89485-146">`ref` 是一个可选文件夹，其中包含 .NET 程序集，用于定义应用程序编译的公共外围（公共类型和方法）。</span><span class="sxs-lookup"><span data-stu-id="89485-146">`ref` is an optional folder that contains .NET assemblies defining the public surface (public types and methods) for an application to compile against.</span></span> <span data-ttu-id="89485-147">此文件夹中的程序集可能没有实现，只是用于为编译器定义外围应用。</span><span class="sxs-lookup"><span data-stu-id="89485-147">The assemblies in this folder may have no implementation, they are purely used to define surface area for the compiler.</span></span> <span data-ttu-id="89485-148">如果包没有 `ref` 文件夹，那么 `lib` 同时为引用程序集和实现程序集。</span><span class="sxs-lookup"><span data-stu-id="89485-148">If the package has no `ref` folder, then the `lib` is both the reference assembly and the implementation assembly.</span></span> |
| <span data-ttu-id="89485-149">运行时</span><span class="sxs-lookup"><span data-stu-id="89485-149">Runtimes</span></span> | <span data-ttu-id="89485-150">`runtimes` 是一个可选文件夹，其中包含操作系统特定的代码，如 CPU 体系结构以及操作系统特定的或依赖于其他平台的二进制文件。</span><span class="sxs-lookup"><span data-stu-id="89485-150">`runtimes` is an optional folder that contains OS specific code, such as CPU architecture and OS specific or otherwise platform-dependent binaries.</span></span> |


## <a name="msbuild-targets-and-props-files-in-packages"></a><span data-ttu-id="89485-151">包中的 MSBuild 目标和属性文件</span><span class="sxs-lookup"><span data-stu-id="89485-151">MSBuild targets and props files in packages</span></span>

<span data-ttu-id="89485-152">NuGet 包可能包含 `.targets` 和 `.props` 文件，这些文件被导入到包安装到的任何 MSBuild 项目中。</span><span class="sxs-lookup"><span data-stu-id="89485-152">NuGet packages can contain `.targets` and `.props` files which are imported into any MSBuild project that the package is installed into.</span></span> <span data-ttu-id="89485-153">在 NuGet 2.x 中，此操作是通过向 `.csproj` 文件注入 `<Import>` 语句而完成的；在 NuGet 3.0 中，没有特定的“安装到项目”操作。</span><span class="sxs-lookup"><span data-stu-id="89485-153">In NuGet 2.x, this was done by injecting `<Import>` statements into the `.csproj` file, in NuGet 3.0 there is no specific "installation to project" action.</span></span> <span data-ttu-id="89485-154">而是包还原过程写入 `[projectname].nuget.props` 和 `[projectname].NuGet.targets` 两个文件。</span><span class="sxs-lookup"><span data-stu-id="89485-154">Instead the package restore process writes two files `[projectname].nuget.props` and `[projectname].NuGet.targets`.</span></span>

<span data-ttu-id="89485-155">MSBuild 知道查找这两个文件，并在项目生成过程开始和结束时自动导入它们。</span><span class="sxs-lookup"><span data-stu-id="89485-155">MSBuild knows to look for these two files and automatically imports them near the beginning and near the end of the project build process.</span></span> <span data-ttu-id="89485-156">此行为与 NuGet 2.x 非常相似，但有一个主要区别：在此情况下，无法保证目标/道属性文件的顺序。</span><span class="sxs-lookup"><span data-stu-id="89485-156">This provides very similar behavior to NuGet 2.x, but with one major difference: *there is no guaranteed order of targets/props files in this case*.</span></span> <span data-ttu-id="89485-157">但是，MSBuild 通过 `<Target>` 定义的 `BeforeTargets` 和 `AfterTargets` 特性（请参阅 [Target 元素 (MSBuild)](https://docs.microsoft.com/visualstudio/msbuild/target-element-msbuild)）提供了对目标进行排序的方法。</span><span class="sxs-lookup"><span data-stu-id="89485-157">However, MSBuild does provide ways to order targets through the `BeforeTargets` and `AfterTargets` attributes of the `<Target>` definition (see [Target Element (MSBuild)](https://docs.microsoft.com/visualstudio/msbuild/target-element-msbuild).</span></span>


## <a name="lib-and-ref"></a><span data-ttu-id="89485-158">Lib 和引用</span><span class="sxs-lookup"><span data-stu-id="89485-158">Lib and Ref</span></span>

<span data-ttu-id="89485-159">在 NuGet v3 中，`lib` 文件夹的行为没有重大变化。</span><span class="sxs-lookup"><span data-stu-id="89485-159">The behavior of the `lib` folder hasn't changed significantly in NuGet v3.</span></span> <span data-ttu-id="89485-160">但是，所有程序集都必须位于以 TxM 命名的子文件夹内，不再直接放置在 `lib` 文件夹下。</span><span class="sxs-lookup"><span data-stu-id="89485-160">However, all assemblies must be within sub-folders named after a TxM, and can no longer be placed directly under the `lib` folder.</span></span> <span data-ttu-id="89485-161">TxM 是包中给定资产适用的平台名称。</span><span class="sxs-lookup"><span data-stu-id="89485-161">A TxM is the name of a platform that a given asset in a package is supposed to work for.</span></span> <span data-ttu-id="89485-162">逻辑上，这些是目标框架名字对象 (TFM) 的扩展，例如，`net45`、`net46`、`netcore50` 和 `dnxcore50` 都是 TxM 的示例（请参阅[目标框架](../Schema/Target-Frameworks.md)）。</span><span class="sxs-lookup"><span data-stu-id="89485-162">Logically these are an extension of the Target Framework Monikers (TFM) e.g. `net45`, `net46`, `netcore50`, and `dnxcore50` are all examples of TxMs (see [Target Frameworks](../Schema/Target-Frameworks.md).</span></span> <span data-ttu-id="89485-163">TxM 可以指框架 (TFM) 以及其他平台特定的外围应用。</span><span class="sxs-lookup"><span data-stu-id="89485-163">TxM can refer to a framework (TFM) as well as other platform-specific surface areas.</span></span> <span data-ttu-id="89485-164">例如，UWP TxM (`uap10.0`) 表示 .NET 外围应用以及适用于 UWP 应用程序的 Windows 外围应用。</span><span class="sxs-lookup"><span data-stu-id="89485-164">For example the UWP TxM (`uap10.0`) represents the .NET surface area as well as the Windows surface area for UWP applications.</span></span>

<span data-ttu-id="89485-165">Lib 结构示例：</span><span class="sxs-lookup"><span data-stu-id="89485-165">An example lib structure:</span></span>

    lib
    ├───net40
    │       MyLibrary.dll
    └───wp81
            MyLibrary.dll

<span data-ttu-id="89485-166">`lib` 文件夹包含在运行时使用的程序集。</span><span class="sxs-lookup"><span data-stu-id="89485-166">The `lib` folder contains assemblies that are used at runtime.</span></span> <span data-ttu-id="89485-167">对于大多数包而言，每个目标 TxM 的 `lib` 下的文件夹全都是必需的。</span><span class="sxs-lookup"><span data-stu-id="89485-167">For most packages a folder under `lib` for each of the target TxMs is all that is required.</span></span>

## <a name="ref"></a><span data-ttu-id="89485-168">引用</span><span class="sxs-lookup"><span data-stu-id="89485-168">Ref</span></span>

<span data-ttu-id="89485-169">有时在编译期间应使用不同的程序集（如 .NET 引用程序集）。</span><span class="sxs-lookup"><span data-stu-id="89485-169">There are sometimes cases where a different assembly should be used during compilation (.NET Reference Assemblies do this today).</span></span> <span data-ttu-id="89485-170">对于这些情况，请使用名为 `ref`（“引用程序集”的缩写）的顶层文件夹。</span><span class="sxs-lookup"><span data-stu-id="89485-170">For those cases, use a top-level folder called `ref` (short for "Reference Assemblies").</span></span>

<span data-ttu-id="89485-171">大多数包创建者都不需要 `ref` 文件夹。</span><span class="sxs-lookup"><span data-stu-id="89485-171">Most package authors don't require the `ref` folder.</span></span> <span data-ttu-id="89485-172">这对以下包非常有用：需要为编译和 IntelliSense 提供一致的外围应用，但对不同的 TxM 具有不同的实现。</span><span class="sxs-lookup"><span data-stu-id="89485-172">It is useful for packages that need to provide a consistent surface area for compilation and IntelliSense but then have different implementation for different TxMs.</span></span> <span data-ttu-id="89485-173">最大的用例是 `System.*` 包，这些包是在 NuGet 上提供 .NET Core 的过程中生成的。</span><span class="sxs-lookup"><span data-stu-id="89485-173">The biggest use case of this are the `System.*` packages that are being produced as part of shipping .NET Core on NuGet.</span></span> <span data-ttu-id="89485-174">这些包具有各种实现，这些实现由一组一致的引用程序集统一。</span><span class="sxs-lookup"><span data-stu-id="89485-174">These packages have various implementations that are being unified by a consistent set of ref assemblies.</span></span>

<span data-ttu-id="89485-175">从表面上讲，`ref` 文件夹中包含的程序集是传递给编译器的引用程序集。</span><span class="sxs-lookup"><span data-stu-id="89485-175">Mechanically, the assemblies included in the `ref` folder are the reference assemblies being passed to the compiler.</span></span> <span data-ttu-id="89485-176">对于使用过 csc.exe 的用户来说，这些是我们传递给 [C# /reference 选项](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/compiler-options/reference-compiler-option) 开关的程序集。</span><span class="sxs-lookup"><span data-stu-id="89485-176">For those of you who have used csc.exe these are the assemblies we are passing to the [C# /reference option](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/compiler-options/reference-compiler-option) switch.</span></span>

<span data-ttu-id="89485-177">`ref` 文件夹的结构与 `lib` 的结构相同，例如：</span><span class="sxs-lookup"><span data-stu-id="89485-177">The structure of the `ref` folder is the same as `lib`, for example:</span></span>

    └───MyImageProcessingLib
         ├───lib
         │   ├───net40
         │   │       MyImageProcessingLibrary.dll
         │   │
         │   ├───net451
         │   │       MyImageProcessingLibrary.dll
         │   │
         │   └───win81
         │           MyImageProcessingLibrary.dll
         │
         └───ref
             ├───net40
             │       MyImageProcessingLibrary.dll
             │
             └───portable-net451-win81
                     MyImageProcessingLibrary.dll


<span data-ttu-id="89485-178">在本例中，`ref` 目录中的程序集是相同的。</span><span class="sxs-lookup"><span data-stu-id="89485-178">In this example the assemblies in the `ref` directories would all be identical.</span></span>


## <a name="runtimes"></a><span data-ttu-id="89485-179">运行时</span><span class="sxs-lookup"><span data-stu-id="89485-179">Runtimes</span></span>

<span data-ttu-id="89485-180">运行时文件夹包含在特定“运行时”上运行所需的程序集和本机库，通常由操作系统和 CPU 体系结构定义。</span><span class="sxs-lookup"><span data-stu-id="89485-180">The runtimes folder contains assemblies and native libraries required to run on specific "runtimes", which are generally defined by Operating System and CPU architecture.</span></span> <span data-ttu-id="89485-181">这些运行时使用[运行时标识符 (RID)](https://docs.microsoft.com/dotnet/core/rid-catalog) 进行标识，如 `win`、`win-x86`、`win7-x86`、`win8-64` 等。</span><span class="sxs-lookup"><span data-stu-id="89485-181">These runtimes are identified using [Runtime Identifiers (RIDs)](https://docs.microsoft.com/dotnet/core/rid-catalog) such as `win`, `win-x86`, `win7-x86`, `win8-64`, etc.</span></span>

## <a name="native-light-up"></a><span data-ttu-id="89485-182">本机启动</span><span class="sxs-lookup"><span data-stu-id="89485-182">Native light-up</span></span>

<span data-ttu-id="89485-183">以下示例显示这样的包：拥有适用于多个平台的纯托管实现，但可在 Windows 8 上使用本机帮助程序以调用 Windows 8 特定的本机 API。</span><span class="sxs-lookup"><span data-stu-id="89485-183">The following example shows a package that has a purely managed implementation for several platforms, but uses native helpers on Windows 8 where it can call into Windows 8-specific native APIs.</span></span>

    └───MyLibrary
         ├───lib
         │   └───net40
         │           MyLibrary.dll
         │
         └───runtimes
             ├───win8-x64
             │   ├───lib
             │   │   └───net40
             │   │           MyLibrary.dll
             │   │
             │   └───native
             │           MyNativeLibrary.dll
             │
             └───win8-x86
                 ├───lib
                 │   └───net40
                 │           MyLibrary.dll
                 │
                 └───native
                         MyNativeLibrary.dll

<span data-ttu-id="89485-184">提供上述包时，将发生以下情况：</span><span class="sxs-lookup"><span data-stu-id="89485-184">Given the above package the following things happen:</span></span>

- <span data-ttu-id="89485-185">当不在 Windows 8 上，将使用 `lib/net40/MyLibrary.dll` 程序集。</span><span class="sxs-lookup"><span data-stu-id="89485-185">When not on Windows 8 the `lib/net40/MyLibrary.dll` assembly is used.</span></span>

- <span data-ttu-id="89485-186">在 Windows 8 上时，将使用 `runtimes/win8-<architecture>/lib/MyLibrary.dll`，并且 `native/MyNativeHelper.dll` 将复制到生成的输出。</span><span class="sxs-lookup"><span data-stu-id="89485-186">When on Windows 8 the `runtimes/win8-<architecture>/lib/MyLibrary.dll` is used and the `native/MyNativeHelper.dll` is copied to the output of your build.</span></span>

<span data-ttu-id="89485-187">在上面的示例中，`lib/net40` 程序集是纯托管代码，而运行时文件夹中的程序集将平台调用到本机帮助程序程序集中，以调用特定于 Windows 8 的 API。</span><span class="sxs-lookup"><span data-stu-id="89485-187">In the example above the `lib/net40` assembly is purely managed code, whilst the assemblies in the runtimes folder will p/invoke into the native helper assembly to call APIs specific to Windows 8.</span></span>

<span data-ttu-id="89485-188">始终仅选取单个 `lib` 文件夹，因此如果有运行时特定的文件夹，将选择通过非运行时特定的 `lib`。</span><span class="sxs-lookup"><span data-stu-id="89485-188">Only a single `lib` folder is ever be picked, so if there is a runtime specific folder it is chosen over non-runtime specific `lib`.</span></span> <span data-ttu-id="89485-189">本机文件夹是累加式的，如果存在，将被复制到生成的输出。</span><span class="sxs-lookup"><span data-stu-id="89485-189">The native folder is additive, if it exists it's copied to the output of the build.</span></span>

## <a name="managed-wrapper"></a><span data-ttu-id="89485-190">托管包装器</span><span class="sxs-lookup"><span data-stu-id="89485-190">Managed wrapper</span></span>

<span data-ttu-id="89485-191">使用运行时的另一种方式是将属于纯托管包装器的包提供到本机程序集上。</span><span class="sxs-lookup"><span data-stu-id="89485-191">Another way to use runtimes is to ship a package that is purely a managed wrapper over a native assembly.</span></span> <span data-ttu-id="89485-192">在此方案中，将创建如下所示的包：</span><span class="sxs-lookup"><span data-stu-id="89485-192">In this scenario you create a package like the following:</span></span>

    └───MyLibrary
         └───runtimes
             ├───win8-x64
             │   ├───lib
             │   │   └───net451
             │   │           MyLibrary.dll
             │   │
             │   └───native
             │           MyImplementation.dll
             │
             └───win8-x86
                 ├───lib
                 │   └───net451
                 │           MyLibrary.dll
                 │
                 └───native
                         MyImplementation.dll

<span data-ttu-id="89485-193">在此情况下，没有顶层 `lib` 文件夹作为该文件夹，因为此包的实现不依赖于相应的本机程序集。</span><span class="sxs-lookup"><span data-stu-id="89485-193">In this case there is no top-level `lib` folder as that folder as there is no implementation of this package that doesn't rely on the corresponding native assembly.</span></span> <span data-ttu-id="89485-194">如果托管程序集 `MyLibrary.dll` 在这两种情况下完全相同，则可将它放在顶层 `lib` 文件夹中；但是，由于缺少本机程序集并不会导致将包安装到非 win-x86 或 win-x64 平台上时失败，因此将使用顶层 lib，但不会复制本机程序集。</span><span class="sxs-lookup"><span data-stu-id="89485-194">If the managed assembly, `MyLibrary.dll`, was exactly the same in both of these cases then we could put it in a top level `lib` folder, but because the lack of a native assembly doesn't cause the package to fail installing if it was installed on a platform that wasn't win-x86 or win-x64 then the top level lib would be used but no native assembly would be copied.</span></span>


## <a name="authoring-packages-for-nuget-2-and-nuget-3"></a><span data-ttu-id="89485-195">创作适用于 NuGet 2 和 NuGet 3 的包</span><span class="sxs-lookup"><span data-stu-id="89485-195">Authoring packages for NuGet 2 and NuGet 3</span></span>

<span data-ttu-id="89485-196">如果想创建一个可供使用 `packages.config` 的项目使用的包以及使用 `project.json` 的包，则以下内容适用：</span><span class="sxs-lookup"><span data-stu-id="89485-196">If you want to create a package that can be consumed by projects using `packages.config` as well as packages using `project.json` then the following apply:</span></span>

- <span data-ttu-id="89485-197">引用和运行时只适用于 NuGet 3。</span><span class="sxs-lookup"><span data-stu-id="89485-197">Ref and runtimes only work on NuGet 3.</span></span> <span data-ttu-id="89485-198">在 NuGet 2 中将被忽略。</span><span class="sxs-lookup"><span data-stu-id="89485-198">They are both ignored by NuGet 2.</span></span>

- <span data-ttu-id="89485-199">不能依靠 `install.ps1` 或 `uninstall.ps1` 来运行。</span><span class="sxs-lookup"><span data-stu-id="89485-199">You cannot rely on `install.ps1` or `uninstall.ps1` to function.</span></span> <span data-ttu-id="89485-200">这些文件在使用 `packages.config` 时执行，但使用 `project.json` 时会忽略。</span><span class="sxs-lookup"><span data-stu-id="89485-200">These files execute when using `packages.config`, but are ignored with `project.json`.</span></span> <span data-ttu-id="89485-201">因此包需要不运行它们时可用。</span><span class="sxs-lookup"><span data-stu-id="89485-201">So your package needs to be usable without them running.</span></span> <span data-ttu-id="89485-202">`init.ps1` 仍在 NuGet 3 上运行。</span><span class="sxs-lookup"><span data-stu-id="89485-202">`init.ps1` still runs on NuGet 3.</span></span>

- <span data-ttu-id="89485-203">目标与属性安装不同，因此请确保包在两个客户端上按预期工作。</span><span class="sxs-lookup"><span data-stu-id="89485-203">Targets and Props installation is different, so make sure that your package works as expected on both clients.</span></span>

- <span data-ttu-id="89485-204">在 NuGet 3 中，lib 的子目录必须是一个 TxM。</span><span class="sxs-lookup"><span data-stu-id="89485-204">Subdirectories of lib must be a TxM in NuGet 3.</span></span> <span data-ttu-id="89485-205">不能将库放置在 `lib` 文件夹的根目录下。</span><span class="sxs-lookup"><span data-stu-id="89485-205">You cannot place libraries at the root of the `lib` folder.</span></span>

- <span data-ttu-id="89485-206">NuGet 3 将不会自动复制内容。</span><span class="sxs-lookup"><span data-stu-id="89485-206">Content is not copied automatically with NuGet 3.</span></span> <span data-ttu-id="89485-207">包的使用者可以自己复制文件，或使用任务运行程序之类的工具来自动复制文件。</span><span class="sxs-lookup"><span data-stu-id="89485-207">Consumers of your package could copy the files themselves or use a tool like a task runner to automate copying the files.</span></span>

- <span data-ttu-id="89485-208">NuGet 3 不会运行源和配置文件转换。</span><span class="sxs-lookup"><span data-stu-id="89485-208">Source and config file transforms are not run by NuGet 3.</span></span>

<span data-ttu-id="89485-209">如果支持 NuGet 2 和 3，`minClientVersion` 应该是包适用的最低版本的 NuGet 2 客户端。</span><span class="sxs-lookup"><span data-stu-id="89485-209">If you are supporting NuGet 2 and 3 then your `minClientVersion` should be the lowest version of NuGet 2 client that your package works on.</span></span> <span data-ttu-id="89485-210">如果存在现有包，则不需要更改。</span><span class="sxs-lookup"><span data-stu-id="89485-210">In the case of an existing package it shouldn't need to change.</span></span>