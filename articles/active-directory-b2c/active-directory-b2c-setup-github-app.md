---
title: GitHub アカウントでのサインアップおよびサインインを設定する - Azure Active Directory B2C
description: Azure Active Directory B2C を使用するアプリケーションで GitHub アカウントを持つ顧客にサインアップとサインインを提供します。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 056570db89fbe1a3db55c138b46e5b73acc282f8
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/19/2019
ms.locfileid: "69622435"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C を使用して GitHub アカウントでのサインアップおよびサインインを設定する

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-github-oauth-application"></a>GitHub OAuth アプリケーションを作成する

Azure Active Directory (Azure AD) B2C で [ID プロバイダー](active-directory-b2c-reference-oauth-code.md)として GitHub アカウントを使用するには、それを表すアプリケーションをテナント内に作成する必要があります。 まだ GitHub アカウントを持っていない場合は、[https://www.github.com/](https://www.github.com/) でサインアップできます。

1. GitHub 資格情報を使用して [GitHub Developer](https://github.com/settings/developers) Web サイトにサインインします。
1. **OAuth アプリ**を選択し、**新規 OAuth アプリ**を選択します。
1. **アプリケーション名**と**ホームページ URL** を入力します。
1. **[Authorization callback URL]** (承認コールバック URL) に `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` を入力します。 `your-tenant-name`を Azure AD B2C テナントの名前に置き換えます。 テナントが Azure AD B2C に大文字で定義されている場合でも、テナント名を入力するときに、すべての小文字を使用します。
1. **[Register application (アプリケーションを登録する)]** をクリックします。
1. **[クライアント ID]** と **[クライアント シークレット]** の値をコピーします。 ID プロバイダーをテナントに追加するには、両方が必要です。

## <a name="configure-a-github-account-as-an-identity-provider"></a>ID プロバイダーとして GitHub アカウントを構成する

1. Azure AD B2C テナントの全体管理者として [Azure Portal](https://portal.azure.com/) にサインインします。
1. ご利用の Azure AD B2C テナントを含むディレクトリを使用していることを確認してください。そのためには、トップ メニューにある **[ディレクトリ + サブスクリプション]** フィルターを選択して、ご利用のテナントを含むディレクトリを選択します。
1. Azure Portal の左上隅の **[すべてのサービス]** を選択し、 **[Azure AD B2C]** を検索して選択します。
1. **[ID プロバイダー]** を選択してから、 **[GitHub (プレビュー)]** を選択します。
1. **[名前]** を入力します。 たとえば、「*GitHub*」とします。
1. **[クライアント ID]** には、前に作成した GitHub アプリケーションのクライアント ID を入力します。
1. **[クライアント シークレット]** には、記録したクライアント シークレットを入力します。
1. **[保存]** を選択します。
