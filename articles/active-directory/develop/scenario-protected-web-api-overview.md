---
title: 保護された Web API - 概要 | Azure
description: 保護された Web API を構築する方法 (概要) について説明します。
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02bd4b84cc7542714f6db45c12c4b5b13a7fb449
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852589"
---
# <a name="scenario-protected-web-api"></a>シナリオ: 保護された Web API

このシナリオでは、Web API を公開する方法と、認証されたユーザーのみが API にアクセスできるように Web API を保護する方法をご紹介します。 職場と学校の両方のアカウントまたは個人的な Microsoft 個人アカウントで認証されたユーザーがあなたの Web API を使用できるようにする必要が生じる場合があります。

## <a name="prerequisites"></a>前提条件

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>詳細

Web API を保護するために知っておく必要があるいくつかの詳細を以下に示します。

- お使いのアプリの登録では、少なくとも 1 つのスコープを公開する必要があります。 お使いの Web API で受け入れられるトークンのバージョンは、サインイン対象ユーザーによって異なります。
- Web API のコードの構成では、Web API を呼び出すときに使用するトークンを検証する必要があります。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [アプリの登録](scenario-protected-web-api-app-registration.md)
