---
title: LUIS 预构建实体电话号码参考 - Azure | Microsoft Docs
titleSuffix: Azure
description: 本文包含语言理解 (LUIS) 中的电话号码预构建实体信息。
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 0f72b807b9b0ec110a80d67babb1c45902b8c810
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321559"
---
# <a name="phonenumber-entity"></a>Phonenumber 实体
`phonenumber` 实体提取各种包括国家/地区代码在内的电话号码。 此实体已定型，因此不需要将陈述示例添加到应用程序。 仅在 `en-us` 区域性中支持 `phonenumber` 实体。 

## <a name="types-of-phonenumber"></a>phonenumber 类型
Phonenumber 托管在 [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) Github 存储库

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>预构建的 phonenumber 实体解析
以下示例演示了 builtin.phonenumber 实体解析。

```JSON
{
  "query": "my mobile is 00 44 161 1234567",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
      "entity": "00 44 161 1234567",
      "type": "builtin.phonenumber",
      "startIndex": 13,
      "endIndex": 29,
      "resolution": {
        "value": "00 44 161 1234567"
      }
    }
  ]
}
```


## <a name="next-steps"></a>后续步骤

了解有关[百分比](luis-reference-prebuilt-percentage.md)、[数字](luis-reference-prebuilt-number.md)和[温度](luis-reference-prebuilt-temperature.md)实体。 