---
title: Azure portal を使用してサービス カタログ アプリをデプロイする | Microsoft Docs
description: マネージド アプリケーションの利用者に、Azure portal でサービス カタログ アプリをデプロイする方法を示します。
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 4d2e8b442f70ee791fe65a32402e5272eda3f209
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "60589044"
---
# <a name="deploy-service-catalog-app-through-azure-portal"></a>Azure portal を使用してサービス カタログ アプリをデプロイする

[前のクイック スタート](publish-managed-app-definition-quickstart.md)ではマネージド アプリケーションの定義を発行しました。 このクイック スタートでは、その定義からサービス カタログ アプリを作成します。

## <a name="create-service-catalog-app"></a>サービス カタログ アプリを作成する

Azure portal で次の手順のようにします。

1. **[リソースの作成]** を選択します。

   ![リソースの作成](./media/deploy-service-catalog-quickstart/create-new.png)

1. 「**Service Catalog Managed Application**」を検索し、使用可能なオプションからそれを選択します。

   ![サービス カタログ アプリケーションを検索する](./media/deploy-service-catalog-quickstart/select-service-catalog.png)

1. マネージ アプリケーション サービスの説明が表示されます。 **作成** を選択します。

   ![作成の選択](./media/deploy-service-catalog-quickstart/create-service-catalog.png)

1. アクセスできるマネージ アプリケーションの定義が表示されます。 使用可能な定義から、デプロイするものを選択します。 このクイック スタートでは、前のクイック スタートで作成した **Managed Storage Account** の定義を使用します。 **作成** を選択します。

   ![デプロイする定義を選択する](./media/deploy-service-catalog-quickstart/select-definition.png)

1. **[基本]** タブの値を指定します。サービス カタログ アプリをデプロイする Azure サブスクリプションを選択します。 **applicationGroup** という名前の新しいリソース グループを作成します。 アプリの場所を選択します。 終わったら、 **[OK]** を選択します。

   ![基本の値を指定する](./media/deploy-service-catalog-quickstart/provide-basics.png)

1. ストレージ アカウント名のプレフィックスを指定します。 作成するストレージ アカウントの種類を選択します。 終わったら、 **[OK]** を選択します。

   ![ストレージの値を指定する](./media/deploy-service-catalog-quickstart/provide-storage.png)

1. 概要を確認します。 検証が成功したら、 **[OK]** を選択してデプロイを開始します。

   ![概要を表示する](./media/deploy-service-catalog-quickstart/view-summary.png)

## <a name="view-results"></a>結果の表示

サービス カタログ アプリのデプロイが完了すると、2 つの新しいリソース グループが作成されています。 1 つのリソース グループでは、サービス カタログ アプリが保持されています。 もう 1 つのリソース グループでは、サービス カタログ アプリ用のリソースが保持されています。

1. **applicationGroup** という名前のリソース グループを表示して、サービス カタログ アプリを確認します。

   ![アプリケーションを表示する](./media/deploy-service-catalog-quickstart/view-managed-application.png)

1. **applicationGroup{ハッシュ文字}** という名前のリソース グループを表示して、サービス カタログ アプリ用のリソースを確認します。

   ![リソースの表示](./media/deploy-service-catalog-quickstart/view-resources.png)

## <a name="next-steps"></a>次の手順

* マネージド アプリケーションの定義ファイルの作成方法を学習するには、「[マネージド アプリケーション定義を作成して発行する](publish-service-catalog-app.md)」をご覧ください。
* Azure CLI については、[Azure CLI を使用したサービス カタログ アプリのデプロイ](./scripts/managed-application-cli-sample-create-application.md)に関する記事をご覧ください。
* PowerShell については、[PowerShell を使用したサービス カタログ アプリのデプロイ](./scripts/managed-application-poweshell-sample-create-application.md)に関する記事をご覧ください。