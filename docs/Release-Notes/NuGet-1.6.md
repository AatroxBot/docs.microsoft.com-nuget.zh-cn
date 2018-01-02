---
title: "NuGet 1.6 发行说明 |Microsoft 文档"
author: karann-msft
ms.author: karann-msft
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: ed433790-99bf-4b71-92a8-17314bd49867
description: "包括已知的问题、 bug 修复、 增加的功能，以及 DCRs NuGet 1.6 的发行说明。"
keywords: "NuGet 1.6 发行说明，bug 修复的已知问题，添加了一些功能，DCRs"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 7824d62cb73c54205175ec742cfc26d1ca3aa741
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2017
---
 # <a name="nuget-16-release-notes"></a><span data-ttu-id="eaf2d-104">NuGet 1.6 发行说明</span><span class="sxs-lookup"><span data-stu-id="eaf2d-104">NuGet 1.6 Release Notes</span></span>

<span data-ttu-id="eaf2d-105">[NuGet 1.5 发行说明](../release-notes/nuget-1.5.md) | [NuGet 1.7 发行说明](../release-notes/nuget-1.7.md)</span><span class="sxs-lookup"><span data-stu-id="eaf2d-105">[NuGet 1.5 Release Notes](../release-notes/nuget-1.5.md) | [NuGet 1.7 Release Notes](../release-notes/nuget-1.7.md)</span></span>

<span data-ttu-id="eaf2d-106">NuGet 1.6 已于 2011 年 12 月 13 日发布。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-106">NuGet 1.6 was released on December 13, 2011.</span></span>

## <a name="known-installation-issue"></a><span data-ttu-id="eaf2d-107">已知的安装问题</span><span class="sxs-lookup"><span data-stu-id="eaf2d-107">Known Installation Issue</span></span>
<span data-ttu-id="eaf2d-108">如果你运行的 VS 2010 SP1，你可能尝试升级 NuGet，如果安装了较旧版本时遇到安装错误。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-108">If you are running VS 2010 SP1, you might run into an installation error when attempting to upgrade NuGet if you have an older version installed.</span></span>

<span data-ttu-id="eaf2d-109">解决方法是只需卸载 NuGet，然后从 VS 扩展库安装它。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-109">The workaround is to simply uninstall NuGet and then install it from the VS Extension Gallery.</span></span>  <span data-ttu-id="eaf2d-110">请参阅[http://support.microsoft.com/kb/2581019](http://support.microsoft.com/kb/2581019)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-110">See [http://support.microsoft.com/kb/2581019](http://support.microsoft.com/kb/2581019) for more information.</span></span>

<span data-ttu-id="eaf2d-111">注意： 如果 Visual Studio 不会使您可以卸载的扩展 （卸载按钮为禁用），然后你可能需要重新启动 Visual Studio 中使用"以管理员身份运行"。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-111">Note: If Visual Studio won't allow you to uninstall the extension (the Uninstall button is disabled), then you likely need to restart Visual Studio using "Run as Administrator."</span></span>

## <a name="features"></a><span data-ttu-id="eaf2d-112">功能</span><span class="sxs-lookup"><span data-stu-id="eaf2d-112">Features</span></span>

### <a name="support-for-semantic-versioning-and-prerelease-packages"></a><span data-ttu-id="eaf2d-113">支持语义版本控制和预发行程序包</span><span class="sxs-lookup"><span data-stu-id="eaf2d-113">Support for Semantic Versioning and Prerelease Packages</span></span>
<span data-ttu-id="eaf2d-114">NuGet 1.6 引入了对语义版本控制 (SemVer) 支持。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-114">NuGet 1.6 introduces support for Semantic Versioning (SemVer).</span></span> <span data-ttu-id="eaf2d-115">关于如何使用 SemVer 的详细信息，请参阅[版本控制文档](../create-packages/prerelease-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-115">For more details on how it uses SemVer, read the [Versioning documentation](../create-packages/prerelease-packages.md).</span></span>

### <a name="using-nuget-without-checking-in-packages-package-restore"></a><span data-ttu-id="eaf2d-116">使用 NuGet，而无需签入包 （包还原）</span><span class="sxs-lookup"><span data-stu-id="eaf2d-116">Using NuGet Without Checking In Packages (Package Restore)</span></span>
<span data-ttu-id="eaf2d-117">NuGet 1.6 现在具有工作流中的 NuGet 包不会添加到源代码管理，但改为会还原在生成时如果缺少第一类支持。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-117">NuGet 1.6 now has first class support for the workflow in which NuGet packages are not added to source control, but instead are restored at build time if missing.</span></span> <span data-ttu-id="eaf2d-118">有关详细信息，请阅读[不提交到源代码管理包的情况下使用 NuGet](../consume-packages/packages-and-source-control.md)主题。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-118">For more details, read the [Using NuGet without committing packages to source control](../consume-packages/packages-and-source-control.md) topic.</span></span>

### <a name="item-templates-that-install-nuget-packages"></a><span data-ttu-id="eaf2d-119">安装 NuGet 包的项模板</span><span class="sxs-lookup"><span data-stu-id="eaf2d-119">Item Templates That Install NuGet Packages</span></span>
<span data-ttu-id="eaf2d-120">生成的工作以支持 Visual Studio 项目模板的预安装的 NuGet 程序包，NuGet 1.6 还将添加对 Visual Studio 项模板的支持。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-120">Building on the work to support preinstalled NuGet package to Visual Studio project templates, NuGet 1.6 also adds support for Visual Studio item templates.</span></span> <span data-ttu-id="eaf2d-121">项模板可以具有关联中的模板调用时安装的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-121">Item templates can have associated NuGet packages that are installed when the template in invoked.</span></span>

<span data-ttu-id="eaf2d-122">有关如何更改要安装 NuGet 包的项目服务/项目模板的详细信息，请参阅[Visual Studio 模板中的包](../visual-studio-extensibility/visual-studio-templates.md)主题。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-122">For more details on how to change a project/item template to install NuGet packages, read the [Packages in Visual Studio Templates](../visual-studio-extensibility/visual-studio-templates.md) topic.</span></span>

### <a name="support-for-disabling-package-sources"></a><span data-ttu-id="eaf2d-123">禁用包源的支持</span><span class="sxs-lookup"><span data-stu-id="eaf2d-123">Support for disabling package sources</span></span>
<span data-ttu-id="eaf2d-124">当配置多个包源时，NuGet 将查找每个包的包及其依赖项的安装过程。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-124">When multiple package sources are configured, NuGet will look in each one for packages during installation of a package and its dependencies.</span></span> <span data-ttu-id="eaf2d-125">已关闭的某种原因会严重降低 NuGet 包源。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-125">A package source that is down for some reason can severely slow down NuGet.</span></span>

<span data-ttu-id="eaf2d-126">在 NuGet 1.6 之前无法删除包源，但然后必须要记住的详细信息时，你想要将其添加回。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-126">Prior to NuGet 1.6, you could remove the package source, but then you have to remember the details for when you want to add it back in.</span></span>

<span data-ttu-id="eaf2d-127">NuGet 1.6 允许取消选中的程序包源中禁用它，但保留它。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-127">NuGet 1.6 allows unchecking a package source to disable it, but keep it around.</span></span>

![禁用包](./media/package-source-with-disabled-source.png)

## <a name="bug-fixes"></a><span data-ttu-id="eaf2d-129">Bug 修复</span><span class="sxs-lookup"><span data-stu-id="eaf2d-129">Bug Fixes</span></span>
<span data-ttu-id="eaf2d-130">NuGet 1.6 具有总共 106 工作固定的项。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-130">NuGet 1.6 had a total of 106 work items fixed.</span></span> <span data-ttu-id="eaf2d-131">95 的那些已被归类为 bug 和 10 的那些功能。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-131">95 of those were classified as bugs and 10 of those were features.</span></span>

<span data-ttu-id="eaf2d-132">有关工作的完整列表项固定在 NuGet 1.6，请查看[对于此版本的 NuGet 问题跟踪程序](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%201.6&assignedTo=All&component=All&sortField=Votes&sortDirection=Descending&page=0)。</span><span class="sxs-lookup"><span data-stu-id="eaf2d-132">For a full list of work items fixed in NuGet 1.6, please view the [NuGet Issue Tracker for this release](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%201.6&assignedTo=All&component=All&sortField=Votes&sortDirection=Descending&page=0).</span></span>