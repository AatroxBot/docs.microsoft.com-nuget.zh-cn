---
title: "NuGet CLI config 命令 |Microsoft 文档"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 01/18/2018
ms.topic: reference
ms.prod: nuget
ms.technology: 
description: "Nuget.exe config 命令参考"
keywords: "nuget 配置引用，config 命令"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 34d156a29207122bac3c21c3307cbe7373b5f031
ms.sourcegitcommit: 7969f6cd94eccfee5b62031bb404422139ccc383
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2018
---
# <a name="config-command-nuget-cli"></a>config 命令 (NuGet CLI)

**适用于：**所有&bullet;**受支持的版本**： 所有

获取或设置 NuGet 配置值。 对于其他用法，请参阅[配置 NuGet 行为](../consume-packages/configuring-nuget-behavior.md)。 有关允许的密钥名称的详细信息，请参阅[NuGet 配置文件引用](../reference/nuget-config-file.md)。

## <a name="usage"></a>用法

```cli
nuget config -Set <name>=[<value>] [<name>=<value> ...] [options]
nuget config -AsPath <name> [options]
```

其中`<name>`和`<value>`指定在配置中设置的键 / 值对。 根据需要，可以指定任意数量的对。 若要删除一个值，指定的名称和`=`登录，但没有值。

有关允许的密钥名称，请参阅[NuGet 配置文件引用](../reference/nuget-config-file.md)。

在 NuGet 3.4 +`<value>`可以使用[环境变量](cli-ref-environment-variables.md)。

## <a name="options"></a>选项

| 选项 | 描述 |
| --- | --- |
| AsPath | 返回配置值为路径，忽略时`-Set`使用。 |
| ConfigFile | 要修改的 NuGet 配置文件。 如果未指定， *%AppData%\NuGet\NuGet.Config*使用。 |
| ForceEnglishOutput | *（3.5 +)*强制 nuget.exe 运行使用固定的、 基于英语的区域性。 |
| 帮助 | 显示的帮助命令的信息。 |
| NonInteractive | 取消显示提示用户输入或确认。 |
| 详细级别 | 指定的输出中显示的详细信息量：*正常*， *quiet*，*详细*。 |

另请参阅[环境变量](cli-ref-environment-variables.md)

### <a name="examples"></a>示例

```cli
nuget config -Set repositoryPath=c:\packages -configfile c:\my.config

nuget config -Set repositoryPath=

nuget config -Set repositoryPath=%PACKAGE_REPO% -configfile %ProgramData%\NuGet\NuGetDefaults.Config

nuget config -Set HTTP_PROXY=http://127.0.0.1 -set HTTP_PROXY.USER=domain\user
```