---
title: 关于语音转文本 | Microsoft Docs
description: 语音转文本 API 功能概述。
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 2eee1c6f9158f128ed5ffe575f8f498f1d3eb5e9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2018
ms.locfileid: "35366830"
---
# <a name="about-the-speech-to-text-api"></a>关于语音转文本 API

**语音转文本** API 将音频流转录为应用程序可以向用户显示或作为命令输入操作的文本。 API 可以与 SDK 客户端库（用于支持的平台和语言）或 REST API 一起使用。

**语音转文本** API 提供以下功能：

- Microsoft 的高级语音识别技术 - 与 Cortana、Office 和其他 Microsoft 产品使用的一样。

- 实时连续识别。 **语音转文本**允许用户实时将音频转录为文本。 它还支持接收到目前为止已识别的单词的中间结果。 该服务自动识别结束语。 用户还可以选择其他格式选项，包括大写和标点符号、脏话屏蔽和文本规范化。

- 已针对交互式、对话和听写场景优化**语音转文本**结果。 

- 支持多种方言中的多种口语。 有关每种识别模式下支持的语言的完整列表，请参阅[支持的语言](supported-languages.md#speech-to-text)。

- 自定义的语言和声学模型，因此你可以根据用户的说话方式、说话环境和专业词汇自定义应用程序。

- 自然语言理解。 通过与[语言理解](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) 集成，你可以从语音中获取意向和实体。 用户不必了解应用的词汇，但可以用自己的语言描述他们想要的内容。

## <a name="api-capabilities"></a>API 功能

**语音转文本** API 的某些功能无法通过 REST 获得。 下表总结了每种访问 API 的方法的功能。

| 使用案例 | REST | SDK |
|-----|-----|-----|----|
| 转录简短的陈述，如命令（长度 < 15 秒）；没有中期结果 | 是 | 是 |
| 转录较长的陈述（> 15 秒） | 否 | 是 |
| 使用可选的中间结果转录流式处理音频 | 否 | 是 |
| 通过 LUIS 了解说话者意向 | 否\* | 是 |

\* *可以使用单独的 LUIS 订阅获取 LUIS 意向和实体。通过此订阅，SDK 可以为你调用 LUIS 并提供实体和意向结果以及语音转录。使用 REST API，可以自己调用 LUIS 以使用 LUIS 订阅获取意向和实体。*

## <a name="next-steps"></a>后续步骤

* [获取语音试用订阅](https://azure.microsoft.com/try/cognitive-services/)
* [了解如何在 C# 中识别语音](quickstart-csharp-windows.md)
