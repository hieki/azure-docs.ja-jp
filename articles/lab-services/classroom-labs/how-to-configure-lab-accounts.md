---
title: Azure Lab Services でラボ アカウントを構成する | Microsoft Docs
description: ラボ アカウントを作成後に構成する方法について説明します。
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: ba469c038f04a31a57e798b97b5120bec573feae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "65414035"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Azure Lab Services でラボ アカウントを構成する 
Azure Lab Services では、ラボ アカウントは、クラスルーム ラボなどのマネージド ラボ型のコンテナーです。 管理者は、Azure Lab Services を使用してラボ アカウントを設定し、アカウントにラボを作成できるラボ所有者にアクセスを提供します。 この記事では、ラボ アカウントの作成、すべてのラボ アカウントの表示、またはラボ アカウントの削除を行う方法について説明します。

## <a name="connect-with-a-peer-virtual-network"></a>ピア仮想ネットワークと接続する
ラボの仮想ネットワークにピア ネットワークとして仮想ネットワークを接続するには、次の手順を実行します。

1. **[ラボ アカウント]** ページで、左側のメニューの **[Labs configuration]\(ラボの構成\)** を選択します。

    ![[Labs configuration]\(ラボの構成\) ページ](../media/how-to-manage-lab-accounts/labs-configuration-page.png) 
1. **[仮想ネットワークのピアリング]** で **[有効]** または **[無効]** を選択します。 既定値は **[無効]** です。 仮想ネットワークのピアリングを有効にするには、次の手順を実行します。 
    1. **[Enabled]** を選択します。
    2. ドロップダウン リストから **[VNet]** を選択します。 
3. ツールバーの **[保存]** を選択します。 

このアカウントで作成されたラボは、選択した仮想ネットワークに接続されます。 また、選択した仮想ネットワーク内のリソースにアクセスできます。 詳細については、「[Azure Lab Services でラボのネットワークとピア仮想ネットワークを接続する](how-to-connect-peer-virtual-network.md)」を参照してください。

**[仮想ネットワークのピアリング]** フィールドで仮想ネットワークを選択すると、 **[ラボ作成者にラボの場所の選択を許可する]** オプションが無効になります。 ピア仮想ネットワーク内のリソースと接続するためには、ラボ アカウント内のラボがラボ アカウントと同じリージョンに存在する必要があるためです。 

## <a name="allow-lab-creator-to-pick-location-for-the-lab"></a>ラボの作成者がラボの場所を選択できるようにする
ラボ アカウントとは異なる場所にラボを作成することをラボの作成者に許可するには、次の手順に従います。 

1. **[ラボ アカウント]** ページで、左側のメニューの **[Labs configuration]\(ラボの構成\)** を選択します。
2. **[Allow lab creator to pick lab location]\(ラボ作成者にラボの場所の選択を許可する\)** フィールドで、ラボの作成者がラボの場所を選択できるようにする場合は **[有効]** を選択します。 無効にすると、ラボ アカウントが存在している同じ場所にラボが自動的に作成されます。 
    
    **[仮想ネットワークのピアリング]** フィールドで仮想ネットワークを選択した場合、このフィールドは無効になっています。 ピア仮想ネットワーク内のリソースにアクセスするためには、ラボ アカウント内のラボがラボ アカウントと同じリージョンに存在する必要があるためです。 
1. ツールバーの **[保存]** を選択します。 

    ![ラボの場所の設定を構成する](../media/how-to-manage-lab-accounts/labs-configuration-page-lab-location.png)


## <a name="specify-an-address-range-for-vms-in-the-lab"></a>ラボ内の VM に使用するアドレスの範囲を指定する
次の作業手順では、ラボ内の VM に使用するアドレスの範囲を指定します。 以前に指定した範囲を更新した場合、変更後のアドレスの範囲が適用されるのは、変更後に作成された VM だけです。 

アドレスの範囲を指定する際は、留意すべき制限がいくつかあります。 

- プレフィックスは 23 以下でなければなりません。 
- ラボ アカウントに対して仮想ネットワークがピアリングされている場合、ピアリングされた仮想ネットワークと重複するアドレスの範囲を指定することはできません。

1. **[ラボ アカウント]** ページで、左側のメニューの **[Labs configuration]\(ラボの構成\)** を選択します。
2. ラボに作成される VM のアドレスの範囲を **[アドレスの範囲]** フィールドに指定します。 アドレスの範囲は、Classless Inter-Domain Routing (CIDR) 表記で指定する必要があります (例: 10.20.0.0/23)。 ラボ内の仮想マシンはこのアドレスの範囲に作成されます。
3. ツールバーの **[保存]** を選択します。 

    ![アドレスの範囲を構成する](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>ユーザーをラボの作成者ロールに追加する
ラボ アカウントでクラスルーム ラボを設定するには、ユーザーがラボ アカウントにおける**ラボの作成者**ロールのメンバーであることが必要です。 ラボ アカウントを作成するために使用したアカウントは、このロールに自動的に追加されます。 クラスルーム ラボの作成に、同じユーザー アカウントを使用する場合は、この手順をスキップすることができます。 クラスルーム ラボの作成に、別のユーザー アカウントを使用する場合は、次の手順に従います。 

クラスのラボを作成するアクセス許可を教師に与えるには、教師を**ラボの作成者**ロールに追加します。

1. **[ラボ アカウント]** ページで、 **[アクセス制御 (IAM)]** を選択し、ツールバーの **[+ ロール割り当ての追加]** をクリックします。 

    ![[アクセス制御] -> [ロールの割り当ての追加] ボタン](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. **[ロールの割り当ての追加]** ページで、 **[ロール]** から **[ラボの作成者]** を選択し、ラボの作成者ロールに追加するユーザーを選択した後、 **[保存]** を選択します。 

    ![ラボ作成者の追加](../media/tutorial-setup-lab-account/add-lab-creator.png)

## <a name="specify-marketplace-images-available-to-lab-creators"></a>ラボ作成者が利用できる Marketplace イメージを指定する
ラボ アカウント所有者は、ラボ作成者がラボ アカウントにラボを作成するために使用できる Marketplace イメージを指定できます。 

1. 左側のメニューで、 **[Marketplace イメージ]** を選択します。 既定では、イメージの完全な一覧 (有効なものと無効なものの両方) が表示されます。 上部のドロップダウン リストから **[有効のみ]** / **[無効のみ]** オプションを選択することにより、一覧をフィルター処理して、有効/無効のイメージのみを表示することができます。 
    
    ![Marketplace イメージのページ](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    一覧に表示される Marketplace イメージは、次の条件を満たすものだけです。
        
    - 単一の VM を作成する。
    - Azure Resource Manager を使用して VM をプロビジョニングする
    - 追加のライセンス プランの購入を必要としない
2. 有効になっている Marketplace イメージを**無効にする**には、次のいずれかの操作を行います。 
    1. 最後の列で **[...] (省略記号)** を選択し、 **[Disable image]\(イメージの無効化\)** を選択します。 

        ![1 つのイメージを無効にする](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. 一覧内のイメージ名の前のチェック ボックスをオンにして、一覧から 1 つ以上のイメージを選択し、 **[Disable selected images]\(選択したイメージの無効化\)** を選択します。 

        ![複数のイメージを無効にする](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. 同様に、Marketplace イメージを**有効にする**には、次のいずれかの操作を行います。 
    1. 最後の列で **[...] (省略記号)** を選択し、 **[Enable image]\(イメージの有効化\)** を選択します。 
    2. 一覧内のイメージ名の前のチェック ボックスをオンにして、一覧から 1 つ以上のイメージを選択し、 **[Enable selected images]\(選択したイメージの有効化\)** を選択します。 




## <a name="next-steps"></a>次の手順
次の記事を参照してください。

- [ラボ所有者としてラボを作成および管理する](how-to-manage-classroom-labs.md)
- [ラボ所有者としてテンプレートを設定および発行する](how-to-create-manage-template.md)
- [ラボ所有者としてラボの使用を構成および制御する](how-to-configure-student-usage.md)
- [ラボ ユーザーとしてクラスルーム ラボにアクセスする](how-to-use-classroom-lab.md)
