---
title: コンテナー サポート
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 3886777b283af35e84683480a59097584b537fea
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717150"
---
## <a name="create-an-face-resource"></a>顔リソースを作成する

1. [Azure Portal](https://portal.azure.com) にサインインします。
1. [**顔**リソースの作成](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)をクリックします。
1. すべての必須設定を入力します。

    |Setting|値|
    |--|--|
    |Name|任意の名前 (2 から 64 文字)|
    |サブスクリプション|適切なサブスクリプションを選択します|
    |Location|近くにある任意の使用可能な場所を選択します|
    |価格レベル|`F0` -最小限の価格レベル|
    |リソース グループ|使用可能なリソース グループを選択します|

1. **[作成]** をクリックして、リソースが作成されるまで待ちます。 作成後、リソース ページに移動します。
1. 構成された `endpoint` と API キーを収集します。

    |ポータルでの [リソース] タブ|Setting|値|
    |--|--|--|
    |**概要**|エンドポイント|エンドポイントをコピーします。 それは、`https://face.cognitiveservices.azure.com/face/v1.0` のようなものです|
    |**キー**|API キー|2 つのキーのうちの 1 つをコピーします。 スペースやダッシュを含まない 32 文字の英数字文字列 (`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`) です。|
