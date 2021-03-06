---
title: Azure Notification Hubs で Windows プッシュ通知サービスを構成する | Microsoft Docs
description: Azure 通知ハブ用に Windows プッシュ通知サービスの設定を構成する方法について説明します。
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: c3e3f1e7df5c90c690756375ff1e1b0350c72714
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "60240281"
---
# <a name="configure-windows-push-notification-service-wns-settings-for-a-notification-hub-in-the-azure-portal"></a>Azure portal で通知ハブ用に Windows プッシュ通知サービス (WNS) の設定を構成する
この記事では、Azure portal を使用して Azure 通知ハブ用に Windows 通知サービス (WNS) の設定を構成する方法について説明します。  

## <a name="prerequisites"></a>前提条件
通知ハブを作成していない場合は、ここで作成します。 詳細については、「[Azure portal 内で Azure 通知ハブを作成する](create-notification-hub-portal.md)」を参照してください。 

## <a name="configure-windows-push-notification-service-wns"></a>Windows プッシュ通知サービス (WNS) の構成

次の手順は、通知ハブ用に Windows プッシュ通知サービス (WNS) の設定を構成する手順を示しています。 

1. Azure portal の **[通知ハブ]** ページで、左側のメニューの **[Windows (WNS)]** を選択します。
2. **[パッケージ SID]** と **[セキュリティ キー]** に値を入力します。
3. **[保存]** を選択します。

   ![[パッケージ SID] ボックスと [セキュリティ キー] ボックスを示すスクリーンショット](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

## <a name="next-steps"></a>次の手順
Azure Notification Hubs と Windows プッシュ通知サービス (WNS) を使用して、ユニバーサル Windows プラットフォーム アプリケーションにプッシュ通知を送信する詳細な手順については、[Azure Notification Hubs を使用した UWP アプリへの通知の送信](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)に関するページをご覧ください。


