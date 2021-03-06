---
title: Azure portal で Azure Firewall DNAT を使用して受信トラフィックをフィルター処理する
description: このチュートリアルでは、Azure portal を使用して Azure Firewall DNAT をデプロイして構成する方法を学習します。
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 08/29/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: f0a58382b9825a7b32aee69c00b9801d1c77251a
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114629"
---
# <a name="tutorial-filter-inbound-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>チュートリアル:Azure portal で Azure Firewall DNAT を使用して受信トラフィックをフィルター処理する

受信トラフィックの変換とサブネットに対するフィルター処理を行うように Azure Firewall 宛先ネットワーク アドレス変換 (DNAT) を構成できます。 DNAT を構成すると、NAT ルール コレクションの動作は、**Dnat** に設定されます。 その後、NAT ルール コレクション内の各ルールを使用して、ファイアウォールのパブリック IP およびポートをプライベート IP およびポートに変換できます。 DNAT ルールは、変換されたトラフィックを許可するための対応するネットワーク ルールを暗黙的に追加します。 この動作は、変換されたトラフィックに一致する拒否ルールを使用してネットワーク ルール コレクションを明示的に追加することで、オーバーライドすることができます。 Azure Firewall ルール処理ロジックの詳細については、「[Azure Firewall ルール処理ロジック](rule-processing.md)」を参照してください。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
> * テスト ネットワーク環境を設定する
> * ファイアウォールをデプロイする
> * 既定のルートを作成する
> * DNAT ルールを構成する
> * ファイアウォールをテストする

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

このチュートリアルでは、2 つのピアリングされた VNet を作成します。

- **VN-Hub** - ファイアウォールはこの VNet 内にあります。
- **VN-Spoke** - ワークロード サーバーはこの VNet 内にあります。

## <a name="create-a-resource-group"></a>リソース グループの作成

1. Azure Portal ([https://portal.azure.com](https://portal.azure.com)) にサインインします。
2. Azure portal のホーム ページで **[リソース グループ]** をクリックし、 **[追加]** をクリックします。
3. **[リソース グループ名]** に「**RG-DNAT-Test**」と入力します。
4. **[サブスクリプション]** で、ご使用のサブスクリプションを選択します。
5. **[リソース グループの場所]** で、場所を選択します。 後で作成するすべてのリソースは同じ場所にある必要があります。
6. **Create** をクリックしてください。

## <a name="set-up-the-network-environment"></a>ネットワーク環境を設定する

最初に VNet を作成し、次にそれらをピアリングします。

### <a name="create-the-hub-vnet"></a>ハブ VNet を作成する

1. Azure portal のホーム ページで、 **[すべてのサービス]** をクリックします。
2. **[ネットワーク]** で、 **[仮想ネットワーク]** をクリックします。
3. **[追加]** をクリックします。
4. **[名前]** に「**VN-Hub**」と入力します。
5. **[アドレス空間]** に「**10.0.0.0/16**」と入力します。
6. **[サブスクリプション]** で、ご使用のサブスクリプションを選択します。
7. **[リソース グループ]** で **[既存のものを使用]** を選択し、 **[RG-DNAT-Test]** を選択します。
8. **[場所]** で、以前使用したのと同じ場所を選択します。
9. **[サブネット]** で、 **[名前]** に「**AzureFirewallSubnet**」と入力します。

     ファイアウォールはこのサブネットに配置されます。サブネット名は AzureFirewallSubnet **でなければなりません**。
     > [!NOTE]
     > AzureFirewallSubnet サブネットのサイズは /26 です。 サブネットのサイズの詳細については、「[Azure Firewall に関する FAQ](firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size)」を参照してください。

10. **[アドレス範囲]** に「**10.0.1.0/26**」と入力します。
11. 他の設定については既定値を使用し、 **[作成]** をクリックします。

### <a name="create-a-spoke-vnet"></a>スポーク VNet を作成する

1. Azure portal のホーム ページで、 **[すべてのサービス]** をクリックします。
2. **[ネットワーク]** で、 **[仮想ネットワーク]** をクリックします。
3. **[追加]** をクリックします。
4. **[名前]** に「**VN-Spoke**」と入力します。
5. **[アドレス空間]** に「**192.168.0.0/16**」と入力します。
6. **[サブスクリプション]** で、ご使用のサブスクリプションを選択します。
7. **[リソース グループ]** で **[既存のものを使用]** を選択し、 **[RG-DNAT-Test]** を選択します。
8. **[場所]** で、以前使用したのと同じ場所を選択します。
9. **[サブネット]** の下の **[名前]** に「**SN-Workload**」と入力します。

    サーバーはこのサブネットに含まれます。
10. **[アドレス範囲]** に「**192.168.1.0/24**」と入力します。
11. 他の設定については既定値を使用し、 **[作成]** をクリックします。

### <a name="peer-the-vnets"></a>VNet をピアリングする

次に、2 つの VNet をピアリングします。

#### <a name="hub-to-spoke"></a>ハブからスポークへ

1. **VN-Hub** 仮想ネットワークをクリックします。
2. **[設定]** で **[ピアリング]** をクリックします。
3. **[追加]** をクリックします。
4. 名前として「**Peer-HubSpoke**」と入力します。
5. 仮想ネットワークとして **[VN-Spoke]** を選択します。
6. Click **OK**.

#### <a name="spoke-to-hub"></a>スポークからハブへ

1. **VN-Spoke** 仮想ネットワークをクリックします。
2. **[設定]** で **[ピアリング]** をクリックします。
3. **[追加]** をクリックします。
4. 名前として「**Peer-SpokeHub**」と入力します。
5. 仮想ネットワークとして **[VN-Hub]** を選択します。
6. **[転送されたトラフィックを許可する]** をクリックします。
7. Click **OK**.

## <a name="create-a-virtual-machine"></a>仮想マシンの作成

ワークロード仮想マシンを作成して、**SN-Workload** サブネットに配置します。

1. Azure portal のホーム ページで、 **[すべてのサービス]** をクリックします。
2. **[Compute]** で、 **[仮想マシン]** をクリックします。
3. **[追加]** 、 **[Windows Server]** 、 **[Windows Server 2016 Datacenter]** 、 **[作成]** を順にクリックします。

**基本**

1. **[名前]** に「**Srv-Workload**」と入力します。
5. ユーザー名とパスワードを入力します。
6. **[サブスクリプション]** で、ご使用のサブスクリプションを選択します。
7. **[リソース グループ]** で **[既存のものを使用]** を選択し、 **[RG-DNAT-Test]** をクリックします。
8. **[場所]** で、以前使用したのと同じ場所を選択します。
9. Click **OK**.

**サイズ**

1. Windows Server を実行するテスト仮想マシンの適切なサイズを選択します。 例: **[B2ms]** (8 GB RAM、16 GB のストレージ)。
2. **[選択]** をクリックします。

**設定**

1. **[ネットワーク]** の **[仮想ネットワーク]** で、 **[VN-Spoke]** を選択します。
2. **[サブネット]** で、 **[SN-Workload]** を選択します。
3. **[パブリック IP アドレス]** をクリックし、 **[なし]** をクリックします。
4. **[パブリック受信ポートを選択]** で、 **[パブリック受信ポートなし]** を選択します。 
2. 他の設定については既定値のままにし、 **[OK]** をクリックします。

**まとめ**

概要を確認し、 **[作成]** をクリックします。 これが完了するまでに数分かかります。

デプロイが完了したら、仮想マシンのプライベート IP アドレスをメモしてください。 これは、後でファイアウォールを構成するときに使用します。 仮想マシンの名前をクリックし、 **[設定]** で **[ネットワーク]** をクリックして、プライベート IP アドレスを見つけます。

## <a name="deploy-the-firewall"></a>ファイアウォールをデプロイする

1. ポータルのホーム ページで、 **[リソースの作成]** をクリックします。
2. **[ネットワーク]** をクリックし、 **[おすすめ]** の後で **[すべて表示]** をクリックします。
3. **[ファイアウォール]** をクリックし、 **[作成]** をクリックします。 
4. **[ファイアウォールの作成]** ページで、次の表を使用してファイアウォールを構成します。

   |Setting  |値  |
   |---------|---------|
   |名前     |FW-DNAT-test|
   |Subscription     |\<該当するサブスクリプション\>|
   |Resource group     |**[Use Existing]\(既存の使用\)** : RG-DNAT-Test |
   |Location     |以前使用したのと同じ場所を選択します|
   |仮想ネットワークの選択     |**[Use Existing]\(既存の使用\)** : VN-Hub|
   |パブリック IP アドレス     |**新規作成**。 パブリック IP アドレスは、Standard SKU タイプであることが必要です。|

5. **[Review + create]\(レビュー + 作成\)** をクリックします。
6. 概要を確認し、 **[作成]** をクリックしてファイアウォールを作成します。

   デプロイが完了するまでに数分かかります。
7. デプロイが完了したら、**RG-DNAT-Test** リソース グループに移動し、**FW-DNAT-test** ファイアウォールをクリックします。
8. プライベート IP アドレスをメモします。 後で既定のルートを作成するときにこれを使用します。

## <a name="create-a-default-route"></a>既定のルートを作成する

**SN-Workload** サブネットで、送信の既定ルートがファイアウォールを通過するように構成します。

1. Azure portal のホーム ページで、 **[すべてのサービス]** をクリックします。
2. **[ネットワーク]** で、 **[ルート テーブル]** をクリックします。
3. **[追加]** をクリックします。
4. **[名前]** に「**RT-FWroute**」と入力します。
5. **[サブスクリプション]** で、ご使用のサブスクリプションを選択します。
6. **[リソース グループ]** で **[既存のものを使用]** を選択し、 **[RG-DNAT-Test]** を選択します。
7. **[場所]** で、以前使用したのと同じ場所を選択します。
8. **Create** をクリックしてください。
9. **[更新]** をクリックし、 **[RT-FWroute]** ルート テーブルをクリックします。
10. **[サブネット]** 、 **[関連付け]** の順にクリックします。
11. **[仮想ネットワーク]** をクリックし、 **[VN-Spoke]** を選択します。
12. **[サブネット]** で、 **[SN-Workload]** をクリックします。
13. Click **OK**.
14. **[ルート]** をクリックし、 **[追加]** をクリックします。
15. **[ルート名]** に「**FW-DG**」と入力します。
16. **[アドレス プレフィックス]** に「**0.0.0.0/0**」と入力します。
17. **[次ホップの種類]** で、 **[仮想アプライアンス]** を選択します。

    Azure Firewall は実際はマネージド サービスですが、この状況では仮想アプライアンスが動作します。
18. **[次ホップ アドレス]** に、前にメモしておいたファイアウォールのプライベート IP アドレスを入力します。
19. Click **OK**.

## <a name="configure-a-nat-rule"></a>NAT ルールを構成する

1. **RG-DNAT-Test** を開き、**FW-DNAT-test** ファイアウォールをクリックします。 
2. **FW-DNAT-test** ページの **[設定]** で、 **[ルール]** をクリックします。 
3. **[NAT ルール コレクションの追加]** をクリックします。 
4. **[名前]** に「**RC-DNAT-01**」と入力します。 
5. **[優先度]** に「**200**」と入力します。 
6. **[ルール]** の **[名前]** に「**RL-01**」と入力します。
7. **[プロトコル]** で **[TCP]** を選択します。
8. **[ソース アドレス]** に「*」と入力します。 
9. **[宛先アドレス]** に、ファイアウォールのパブリック IP アドレスを入力します。 
10. **[宛先ポート]** に「**3389**」と入力します。 
11. **[Translated Address] (変換されたアドレス)** に、Srv-Workload 仮想マシンのプライベート IP アドレスを入力します。 
12. **[Translated port] (変換されたポート)** に「**3389**」と入力します。 
13. **[追加]** をクリックします。 

## <a name="test-the-firewall"></a>ファイアウォールをテストする

1. リモート デスクトップをファイアウォールのパブリック IP アドレスに接続します。 **Srv-Workload** 仮想マシンに接続する必要があります。
2. リモート デスクトップを閉じます。

## <a name="clean-up-resources"></a>リソースのクリーンアップ

次のチュートリアルのためにファイアウォール リソースを残しておくことができます。不要であれば、**RG-DNAT-Test** リソース グループを削除して、ファイアウォール関連のすべてのリソースを削除します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、以下の内容を学習しました。

> [!div class="checklist"]
> * テスト ネットワーク環境を設定する
> * ファイアウォールをデプロイする
> * 既定のルートを作成する
> * DNAT ルールを構成する
> * ファイアウォールをテストする

次に、Azure Firewall のログを監視することができます。

> [!div class="nextstepaction"]
> [チュートリアル:Azure Firewall のログを監視する](./tutorial-diagnostics.md)
