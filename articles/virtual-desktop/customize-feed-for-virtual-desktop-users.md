---
title: Windows Virtual Desktop ユーザー用のフィードをカスタマイズする - Azure
description: PowerShell コマンドレットで Windows Virtual Desktop ユーザー用のフィードをカスタマイズする方法。
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: helohr
ms.openlocfilehash: 40fd863d9ad92b91b8e691fbb7be233e16b82c17
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950752"
---
# <a name="customize-feed-for-windows-virtual-desktop-users"></a>Windows Virtual Desktop ユーザー用のフィードをカスタマイズする

フィードは、RemoteApp とリモート デスクトップのリソースがユーザーの認識可能な方法で表示されるようにカスタマイズすることができます。

まず、まだ行っていない場合は、PowerShell セッション内で使用する [Windows Virtual Desktop PowerShell モジュールをダウンロードしてインポート](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview)します。

## <a name="customize-the-display-name-for-a-remoteapp"></a>RemoteApp の表示名をカスタマイズする

公開された RemoteApp の表示名は、フレンドリ名を設定することで変更できます。 既定では、フレンドリ名は RemoteApp プログラムの名前と同じです。

アプリ グループの公開された Remoteapp の一覧を取得するには、次の PowerShell コマンドレットを実行します。

```powershell
Get-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![Name と FriendlyName が強調表示された PowerShell コマンドレット Get-RDSRemoteApp のスクリーンショット。](media/get-rdsremoteapp.png)

RemoteApp にフレンドリ名を割り当てるには、次の PowerShell コマンドレットを実行します。

```powershell
Set-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -Name <existingappname> -FriendlyName <newfriendlyname>
```
![Name と New FriendlyName が強調表示された PowerShell コマンドレット Set-RDSRemoteApp のスクリーンショット。](media/set-rdsremoteapp.png)

## <a name="customize-the-display-name-for-a-remote-desktop"></a>リモート デスクトップの表示名をカスタマイズする

公開されたリモート デスクトップの表示名は、フレンドリ名を設定することで変更できます。 ホスト プールとデスクトップ アプリ グループを、PowerShell を使用して手動で作成した場合、既定のフレンドリ名は「セッション デスクトップ」です。 ホスト プールとデスクトップ アプリ グループを、GitHub の Azure Resource Manager テンプレートまたは Azure Marketplace のサービスを使用して作成した場合、既定のフレンドリ名はホスト プール名と同じです。

リモート デスクトップのリソースを取得するには、次の PowerShell コマンドレットを実行します。

```powershell
Get-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![Name と FriendlyName が強調表示された PowerShell コマンドレット Get-RDSRemoteApp のスクリーンショット。](media/get-rdsremotedesktop.png)

リモート デスクトップのリソースにフレンドリ名を割り当てるには、次の PowerShell コマンドレットを実行します。

```powershell
Set-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -FriendlyName <newfriendlyname>
```
![Name と New FriendlyName が強調表示された PowerShell コマンドレット Set-RDSRemoteApp のスクリーンショット。](media/set-rdsremotedesktop.png)

## <a name="next-steps"></a>次の手順

これでフィードをユーザーに合わせてカスタマイズしたので、Windows Virtual Desktop クライアントにサインインして、それを試すことができます。これを行うには、Windows Virtual Desktop への接続方法に関するページに進みます。
    
 * [Windows 10 または Windows 7 から接続する](connect-windows-7-and-10.md)
 * [Web ブラウザーから接続する](connect-web.md) 
