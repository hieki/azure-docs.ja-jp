---
title: Microsoft Azure Backup Recovery Services コンテナーを削除する
description: この記事では、Microsoft Azure Backup Recovery Services コンテナーを削除する方法について説明します。
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: dacurwin
ms.openlocfilehash: 3d6d374b6e516180ec488fe4de1317a3c99a7f7c
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050118"
---
# <a name="delete-an-azure-backup-recovery-services-vault"></a>Azure Backup Recovery Services コンテナーを削除する

この記事では、Microsoft [Azure Backup](backup-overview.md) Recovery Services (MARS) コンテナーを削除する方法について説明します。 依存関係を削除してからコンテナーの削除を行うための手順が含まれています。


## <a name="before-you-start"></a>開始する前に

保護されたサーバーやバックアップ管理サーバーなどの依存関係が関連付けられている場合には、Recovery Services コンテナーを削除できません。

- バックアップ データを含むコンテナーは、(保護を停止したとしてもバックアップデータを保持していると) 削除できません。

- 依存関係が含まれるコンテナーを削除すると、次のエラーが表示されます。

  ![コンテナーの削除エラー。](./media/backup-azure-delete-vault/error.png)

- 依存関係を含むオンプレミスの保護されたアイテムをポータルから削除すると、警告メッセージが表示されます。

  ![保護されたサーバーの削除エラー。](./media/backup-azure-delete-vault/error-message.jpg)

  
コンテナーを削除するには、ご使用のセットアップに対応するシナリオを選択し、推奨される手順に従います。

シナリオ | コンテナーを削除するために依存関係を削除する手順 |
-- | --
Azure をバックアップ先とする Azure Backup エージェントを使用して保護されている、オンプレミスのファイルとフォルダーがある | 「[MARS 管理コンソールからバックアップ アイテムを削除する](#delete-backup-items-from-the-mars-management-console)」の手順を実行します
Azure への MABS (Microsoft Azure Backup Server) または DPM (System Center Data Protection Manager) を使用して保護されたオンプレミスのマシンがある | 「[MABS 管理コンソールからバックアップ アイテムを削除する](#delete-backup-items-from-the-mabs-management-console)」の手順を実行します
保護されたアイテムがクラウドにある (例: laaS 仮想マシンまたは Azure Files の共有など)  | 「[クラウド内の保護されたアイテムを削除する](#delete-protected-items-in-the-cloud)」の手順を実行します
保護されたアイテムがオンプレミスとクラウドの両方にある | 次のすべてのセクションの手順を次の順番で実行します。 <br> 1.[クラウド内の保護されたアイテムを削除する](#delete-protected-items-in-the-cloud)<br> 2.[MARS 管理コンソールからバックアップ アイテムを削除する](#delete-backup-items-from-the-mars-management-console) <br> 手順 3.[MABS 管理コンソールからバックアップ アイテムを削除する](#delete-backup-items-from-the-mabs-management-console)
保護されたアイテムはオンプレミスまたはクラウドのどちらにもないが、コンテナーの削除エラーが引き続き発生している | 「[Azure Resource Manager を使用して Recovery Services コンテナーを削除する](#delete-the-recovery-services-vault-by-using-azure-resource-manager)」の手順を実行します


## <a name="delete-protected-items-in-the-cloud"></a>クラウド内の保護されたアイテムを削除する

最初に「 **[開始する前に](#before-you-start)** 」セクションを読み、依存関係とコンテナーの削除プロセスを理解してください。

保護を停止してバックアップ データを削除するには、次の手順を実行します。

1. ポータルから **[Recovery Services コンテナー]** に移動し、次に **[バックアップ アイテム]** に移動します。 次に、クラウド内の保護されたアイテムを選択します (Azure Virtual Machines、Azure Storage の Azure Files サービス、Azure Virtual Machines 上の SQL Server など)。

    ![バックアップの種類を選択する。](./media/backup-azure-delete-vault/azure-storage-selected.png)

2. 右クリックしてバックアップ アイテムを選択します。 バックアップ アイテムが保護されているかどうかに応じて、メニューに **[バックアップの停止]** ウィンドウまたは **[バックアップ データの削除]** ウィンドウのいずれかが表示されます。

    - **[バックアップの停止]** ウィンドウが表示されたら、ボックスの一覧から **[バックアップ データの削除]** を選択します。 バックアップ アイテムの名前を入力し (このフィールドでは大文字と小文字が区別されます)、ボックスの一覧から理由を選択します。 コメントがあれば、入力します。 次に、 **[バックアップの停止]** を選択します。

        ![[バックアップの停止] ウィンドウ。](./media/backup-azure-delete-vault/stop-backup-item.png)

    - **[バックアップ データの削除]** ウィンドウが表示されたら、バックアップ アイテムの名前を入力し (このフィールドでは大文字と小文字が区別されます)、ボックスの一覧から理由を選択します。 コメントがあれば、入力します。 次に、 **[削除]** を選択します。 

         ![[バックアップ データの削除] ウィンドウ。](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. **通知**アイコン ![新しい通知アイコン。](./media/backup-azure-delete-vault/messages.png) を確認します。 プロセスが完了すると、サービスによって次のメッセージが表示されます。*バックアップを停止し、"* バックアップ アイテム *" のバックアップ データを削除しています*。 *操作は正常に完了しました*。
6. **[バックアップ アイテム]** メニューで **[更新]** を選択して、バックアップ アイテムが削除されたことを確認します。

      ![バックアップ アイテムの削除に関するページ。](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="delete-protected-items-on-premises"></a>オンプレミスの保護されたアイテムを削除する

最初に「 **[開始する前に](#before-you-start)** 」セクションを読み、依存関係とコンテナーの削除プロセスを理解してください。

1. コンテナーのダッシュボード メニューから **[バックアップ インフラストラクチャ]** を選択します。
2. オンプレミスのシナリオに応じて、次のオプションのいずれかを選択します。

      - MARS の場合は、 **[保護されたサーバー]** 、 **[Azure Backup エージェント]** の順に選択します。 次に、削除するサーバーを選択します。 

        ![MARS の場合、目的のコンテナーを選択してそのダッシュボードを開く。](./media/backup-azure-delete-vault/identify-protected-servers.png)

      - MABS または DPM の場合、 **[バックアップ管理サーバー]** を選択します。 次に、削除するサーバーを選択します。 


          ![MABS の場合、目的のコンテナーを選択してそのダッシュボードを開く。](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. 警告メッセージのある **[削除]** ウィンドウが表示されます。

     ![[削除] ウィンドウ。](./media/backup-azure-delete-vault/delete-protected-server.png)

     警告メッセージと、同意のチェック ボックスの指示を確認します。
    > [!NOTE]
    >- 保護されたサーバーが Azure サービスと同期していて、バックアップ アイテムが存在する場合は、同意のチェック ボックスに、依存するバックアップ アイテムの数と、バックアップ アイテムを表示するためのリンクが表示されます。
    >- 保護されたサーバーが Azure サービスと同期しておらず、バックアップ アイテムが存在する場合は、同意のチェック ボックスに、バックアップ アイテムの数だけが表示されます。
    >- バックアップ アイテムが存在しない場合は、同意のチェック ボックスで削除が求められます。

4. 同意のチェック ボックスをオンにして、 **[削除]** を選択します。




5. **通知**アイコン ![バックアップデータの削除](./media/backup-azure-delete-vault/messages.png) を確認します。 操作が完了すると、次のメッセージが表示されます。"*バックアップを停止し、"バックアップ アイテム" のバックアップ データを削除しています。* " *操作は正常に完了しました*。
6. **[バックアップ アイテム]** メニューで **[更新]** を選択して、バックアップ アイテムが削除されていることを確認します。

このプロセスが完了したら、管理コンソールからバックアップ アイテムを削除できます。
    
   - [MARS 管理コンソールからバックアップ アイテムを削除する](#delete-backup-items-from-the-mars-management-console)
    - [MABS 管理コンソールからバックアップ アイテムを削除する](#delete-backup-items-from-the-mabs-management-console)


### <a name="delete-backup-items-from-the-mars-management-console"></a>MARS 管理コンソールからバックアップ アイテムを削除する

1. MARS 管理コンソールを開き、 **[アクション]** ウィンドウに移動して、 **[バックアップのスケジュール]** を選択します。
2. **[スケジュールされたバックアップの変更または停止]** ページで、 **[このバックアップ スケジュールの使用を中止して、保存されているバックアップをすべて削除する]** を選択します。 次に、 **[次へ]** を選択します。

    ![スケジュールされたバックアップを変更または停止する。](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. **[スケジュールされたバックアップの停止]** ページで、 **[完了]** を選択します。

    ![スケジュールされたバックアップを停止する。](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. セキュリティ PIN (暗証番号) の入力を求められます。これは手動で生成する必要があります。 これを行うには、最初に Azure portal にサインインします。
5. **[Recovery Services コンテナー]**  >  **[設定]**  >  **[プロパティ]** の順に移動します。
6. **[セキュリティ PIN]** の下にある **[生成]** を選択します。 この PIN をコピーします。 この PIN は 5 分間だけ有効です。
7. 管理コンソールに PIN を貼り付け、 **[OK]** を選択します。

    ![セキュリティ PIN を生成する。](./media/backup-azure-delete-vault/security-pin.png)

8. **[バックアップの進行状況の変更]** ページに、次のメッセージが表示されます。"*削除されたバックアップ データは 14 日間保持されます。14 日が経過すると、バックアップ データが完全に削除されます。* " と表示されます。  

    ![バックアップ インフラストラクチャを削除する。](./media/backup-azure-delete-vault/deleted-backup-data.png)

オンプレミスのバックアップ アイテムを削除したら、ポータルから次の手順を実行します。

### <a name="delete-backup-items-from-the-mabs-management-console"></a>MABS 管理コンソールからバックアップ アイテムを削除する

MABS 管理コンソールからバックアップ アイテムを削除するには、次の 2 つの方法が使用できます。

#### <a name="method-1"></a>方法 1
保護を停止してバックアップ データを削除するには、次の手順を実行します。

1.  DPM 管理者コンソールを開いてから、ナビゲーション バーの **[保護]** を選択します。
2.  表示ウィンドウで、削除する保護グループのメンバーを選択します。 右クリックして **[Stop Protection of Group Members]\(グループ メンバーの保護を停止する\)** オプションを選択します。
3.  **[保護の停止]** ダイアログ ボックスで、 **[保護されるデータを削除する]** を選択してから、 **[Delete storage online]\(オンラインでストレージを削除する\)** チェック ボックスをオンにします。 次に、 **[保護の停止]** を選択します。

    ![[保護の停止] ウィンドウで [保護されるデータを削除する] を選択する。](./media/backup-azure-delete-vault/delete-storage-online.png)

    保護されたメンバーの状態が、"*非アクティブなレプリカを利用可能*" に変わります。

4. 非アクティブな保護グループを右クリックして、 **[非アクティブな保護の削除]** を選択します。

    ![非アクティブな保護を削除する。](./media/backup-azure-delete-vault/remove-inactive-protection.png)

5. **[非アクティブな保護の削除]** ウィンドウで **[Delete storage online]\(オンラインでストレージを削除する\)** チェック ボックスをオンにしてから、 **[OK]** を選択します。

    ![オンライン ストレージを削除する。](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

#### <a name="method-2"></a>方法 2
**MABS 管理**コンソールを開きます。 **[データの保護方法の選択]** で、 **[オンライン保護を利用する]** チェック ボックスをオフにします。

  ![データ保護方法を選択します。](./media/backup-azure-delete-vault/data-protection-method.png)

オンプレミスのバックアップ アイテムを削除したら、ポータルから次の手順を実行します。


## <a name="delete-the-recovery-services-vault"></a>Recovery Services コンテナーを削除する

1. すべての依存関係が削除されたら、コンテナー メニュー内の **[Essentials]** ウィンドウにスクロールします。
2. バックアップ アイテム、バックアップ管理サーバー、レプリケートされたアイテムがどれも一覧に表示されていないことを確認します。 アイテムがコンテナーにまだ表示されている場合は、「[開始する前に](#before-you-start)」セクションを参照してください。

3. コンテナーにアイテムがなくなったら、コンテナー ダッシュボードで **[削除]** を選択します。

    ![コンテナー ダッシュボードで [削除] を選択する。](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. コンテナーを削除することを確認するために、 **[はい]** を選択します。 コンテナーが削除されます。 ポータルが **[新規作成]** サービス メニューに戻ります。

## <a name="delete-the-recovery-services-vault-by-using-azure-resource-manager"></a>Azure Resource Manager を使用して Recovery Services コンテナーを削除する

Recovery Services コンテナーを削除するこのオプションは、すべての依存関係が削除されても "*コンテナーの削除エラー*" が引き続き発生する場合にのみ、お勧めします。 次のヒントのいずれかまたはすべてをお試しください。

- コンテナー メニューの **[基本]** ウィンドウから、バックアップ アイテム、バックアップ管理サーバー、レプリケートされたアイテムがどれも一覧に表示されていないことを確認します。 バックアップ アイテムがある場合は、「[開始する前に](#before-you-start)」セクションを参照してください。
- [ポータルからのコンテナーの削除](#delete-the-recovery-services-vault)を再試行します。
- 依存関係をすべて削除してもなお、"*コンテナーの削除エラー*" が発生する場合は、ARMClient ツールを使用して次の手順を実行します (注記の後に記載)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. [chocolatey.org](https://chocolatey.org/) にアクセスし、Chocolatey をダウンロードしてインストールします。 次に、次のコマンドを実行して ARMClient エージェントをインストールします。

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. 自分の Azure アカウントにサインインして、次のコマンドを実行します。

    `ARMClient.exe login [environment name]`

3. Azure portal で、削除するコンテナーのサブスクリプション ID とリソース グループ名を収集します。

ARMClient コマンドの詳細については、[ARMClient の README](https://github.com/projectkudu/ARMClient/blob/master/README.md) を参照してください。

### <a name="use-the-azure-resource-manager-client-to-delete-a-recovery-services-vault"></a>Azure Resource Manager クライアントを使用して Recovery Services コンテナーを削除する

1. ご利用のサブスクリプション ID、リソース グループ名、コンテナー名を使用して、次のコマンドを実行します。 依存関係がなければ、そのコンテナーは次のコマンドを実行するときに削除されます。

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. コンテナーが空でない場合は、次のエラー メッセージが表示されます: "*内部にリソースが存在するため、資格情報コンテナーを削除できません*"。 コンテナー内にある保護されたアイテムまたはコンテナーを削除するには、次のコマンドを実行します。

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Azure portal で、値が削除されていることを確認します。


## <a name="next-steps"></a>次の手順

[Recovery Services コンテナーの詳細情報](backup-azure-recovery-services-vault-overview.md)<br/>
[Recovery Services コンテナーの監視と管理の詳細情報](backup-azure-manage-windows-server.md)
