---
title: NuGet 包名称争议的解决
description: 此过程用于解决 NuGet 包发布服务器之间有关品牌、商标和其他冲突情况的争议。
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 01/18/2018
ms.topic: conceptual
ms.openlocfilehash: 39fe993d73f11ca4b83ae07b1e8b93ae0682d519
ms.sourcegitcommit: a6ca160b1e7e5c58b135af4eba0e9463127a59e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="resolving-disputes-over-nuget-package-names"></a><span data-ttu-id="8aacf-103">解决对 NuGet 包名称的争议</span><span class="sxs-lookup"><span data-stu-id="8aacf-103">Resolving disputes over NuGet package names</span></span>

<span data-ttu-id="8aacf-104">本文推荐的解决过程，适用于社区成员解决与其他 NuGet 发布服务器之间的争议。</span><span class="sxs-lookup"><span data-stu-id="8aacf-104">This article provides a recommended resolution process for community members to resolve disputes with other NuGet publishers.</span></span>

<span data-ttu-id="8aacf-105">例如，假设 Northwind Traders 生成 CRM 系统，且他们将客户端驱动程序作为其网站中可下载的 MSI 提供给该系统。</span><span class="sxs-lookup"><span data-stu-id="8aacf-105">For example, suppose that Northwind Traders makes a CRM system for which they provide client drivers as a downloadable MSI from their website.</span></span> <span data-ttu-id="8aacf-106">独立开发人员 Nancy 想要将 Northwind 客户端库的使用方法变得更简便，因此将其转换成名为 `NorthwindTraders.Client` 的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="8aacf-106">Nancy, an independent developer, wants to make it easier to use Northwind's client library, and turns it into a NuGet package called `NorthwindTraders.Client`.</span></span> <span data-ttu-id="8aacf-107">随后，Northwind 想要为其客户端库生成自己的官方 NuGet 包，因此想要对 Nancy 对该包名称的所有权提出异议。</span><span class="sxs-lookup"><span data-stu-id="8aacf-107">Later, Northwind wants to produce an official NuGet package of their own for their client library, and would thus like to dispute Nancy's ownership of the package name.</span></span>

<span data-ttu-id="8aacf-108">在此情况中，Nancy 的行为似乎没有恶意，只是通过贡献自己的时间和代码支持 Northwind 的工具和客户。</span><span class="sxs-lookup"><span data-stu-id="8aacf-108">In this scenario, Nancy does not appear to be acting with bad intentions, but is rather supporting Northwind's tools and customers by contributing her own time and code.</span></span> <span data-ttu-id="8aacf-109">同时，Northwind 是 Northwind 名称的合法所有者。</span><span class="sxs-lookup"><span data-stu-id="8aacf-109">At the same time, Northwind is the legitimate owner of the Northwind name.</span></span>

<span data-ttu-id="8aacf-110">通过遵照以下流程，Northwind 和 Nancy 可以共同协作，商讨出合适的解决方案，因为双方都希望为开发人员社区提供服务。</span><span class="sxs-lookup"><span data-stu-id="8aacf-110">By following the process below, Northwind and Nancy can work together to a suitable resolution, because both are interested in serving the developer community.</span></span> <span data-ttu-id="8aacf-111">通常不需要 NuGet 团队进行干涉；协作通常是最佳解决方案。</span><span class="sxs-lookup"><span data-stu-id="8aacf-111">It's typically not necessary for the NuGet team to become involved; collaboration usually works out best.</span></span> <span data-ttu-id="8aacf-112">事实上，引起 NuGet 团队注意的每个争议均得到了解决，无需团队做出裁决。</span><span class="sxs-lookup"><span data-stu-id="8aacf-112">In fact, every dispute brought to the NuGet team's attention to date has been worked out without the team needing to pass judgment.</span></span>

## <a name="process"></a><span data-ttu-id="8aacf-113">进程</span><span class="sxs-lookup"><span data-stu-id="8aacf-113">Process</span></span>

1. <span data-ttu-id="8aacf-114">使用包详细信息页上的“联系所有者”链接，联系与其有争议的包的所有者。</span><span class="sxs-lookup"><span data-stu-id="8aacf-114">Contact the owners of the package you have the dispute with using the **Contact Owners** link on the package details page.</span></span> <span data-ttu-id="8aacf-115">以友好和直接的方式说明问题。</span><span class="sxs-lookup"><span data-stu-id="8aacf-115">Explain your issue in a kind and direct manner.</span></span>
2. <span data-ttu-id="8aacf-116">将邮件副本发送到 [support@nuget.org](mailto:support@nuget.org)，以便 NuGet 和 .NET Foundation 了解此争议。</span><span class="sxs-lookup"><span data-stu-id="8aacf-116">Send a copy of your message to [support@nuget.org](mailto:support@nuget.org) so that NuGet and the .NET Foundation are aware of your dispute.</span></span>
3. <span data-ttu-id="8aacf-117">最长需等待 30 天才能解决，之后请再次通知 [support@nuget.org](mailto:support@nuget.org)。</span><span class="sxs-lookup"><span data-stu-id="8aacf-117">Wait a maximum of 30 days for a resolution, then notify [support@nuget.org](mailto:support@nuget.org) again.</span></span> <span data-ttu-id="8aacf-118">nuget.org 支持团队将参与商讨，与双方共同解决争议。</span><span class="sxs-lookup"><span data-stu-id="8aacf-118">The nuget.org support team will get involved and try to work through the dispute with both parties.</span></span>