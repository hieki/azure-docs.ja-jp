---
title: クイック スタート:.NET 用 Form Recognizer クライアント ライブラリ | Microsoft Docs
description: .NET 用 Form Recognizer クライアント ライブラリの使用を開始します。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 07/12/2019
ms.author: pafarley
ms.openlocfilehash: f6cb364c231c2415bf49a24e6e9406a14640b892
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/29/2019
ms.locfileid: "68600118"
---
# <a name="quickstart-form-recognizer-client-library-for-net"></a>クイック スタート:.NET 用 Form Recognizer クライアント ライブラリ

.NET 用 Form Recognizer クライアント ライブラリの使用を開始します。 Form Recognizer は、機械学習テクノロジを使用して、フォーム ドキュメントからキーと値のペアおよびテーブル データを識別して抽出するコグニティブ サービスです。 Form Recognizer は、元のファイル内の関係を含む構造化データを出力します。 以下の手順に従って、SDK パッケージをインストールし、基本タスクのコード例を試してみましょう。

.NET 用 Form Recognizer クライアント ライブラリは、次の目的で使用することができます。

* カスタム Form Recognizer モデルをトレーニングする
* カスタム モデルを使用してフォームを分析する
* カスタム モデルのリストを取得する

[リファレンス ドキュメント](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/formrecognizer?view=azure-dotnet-preview) | [ライブラリのソース コード](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.FormRecognizer) | [パッケージ (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.FormRecognizer/)

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション - [無料アカウントを作成します](https://azure.microsoft.com/free/)。
* 最新バージョンの [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)。
* トレーニング データのセットを含む Azure Storage Blob。 トレーニング データをまとめるためのヒントとオプションについては、[カスタム モデル用のトレーニング データ セットの作成](../build-training-data-set.md)に関する記事を参照してください。 

## <a name="setting-up"></a>設定

### <a name="create-a-form-recognizer-azure-resource"></a>Form Recognizer Azure リソースを作成する

[!INCLUDE [create resource](../includes/create-resource.md)]

試用版のサブスクリプションまたはリソースからキーを取得した後、`FORM_RECOGNIZER_KEY` という名前のキーの[環境変数を作成](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)します。

### <a name="create-a-new-c-application"></a>新しい C# アプリケーションを作成する

コンソール ウィンドウ (cmd、PowerShell、Bash など) で、`dotnet new` コマンドを使用し、`formrecognizer-quickstart` という名前で新しいコンソール アプリを作成します。 このコマンドにより、1 つのソース ファイルを使用する単純な "Hello World" C# プロジェクトが作成されます。_Program.cs_。 

```console
dotnet new console -n formrecognizer-quickstart
```

新しく作成されたアプリ フォルダーにディレクトリを変更します。 次を使用してアプリケーションをビルドします。

```console
dotnet build
```

ビルドの出力に警告やエラーが含まれないようにする必要があります。 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

プロジェクト ディレクトリから、好みのエディターまたは IDE で _Program.cs_ ファイルを開きます。 次の `using` ステートメントを追加します。

```csharp
using Microsoft.Azure.CognitiveServices.FormRecognizer;
using Microsoft.Azure.CognitiveServices.FormRecognizer.Models;

using System;
using System.IO;
using System.Threading.Tasks;
```

次に、アプリケーションの **Main** メソッドに次のコードを追加します。 この非同期タスクは後で定義します。

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_main)]

### <a name="install-the-client-library"></a>クライアント ライブラリをインストールする

次のコマンドを使用して、アプリケーション ディレクトリ内に .NET 用 Form Recognizer クライアント ライブラリをインストールします。

```console
dotnet add package Microsoft.Azure.CognitiveServices.FormRecognizer --version 0.8.0-preview
```

Visual Studio IDE を使用している場合、クライアント ライブラリは、ダウンロード可能な NuGet パッケージとして入手できます。

## <a name="object-model"></a>オブジェクト モデル

Form Recognizer SDK の主な機能は、次のクラスによって処理されます。

|名前|説明|
|---|---|
|[FormRecognizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.formrecognizerclient?view=azure-dotnet-preview)|このクラスは、すべての Form Recognizer 機能に必要です。 このクラスは、サブスクリプション情報を使用してインスタンス化します。これを使用して、他のクラスのインスタンスを生成します。|
|[TrainRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.trainrequest?view=azure-dotnet-preview)| このクラスは、独自のトレーニング入力データを使用してカスタム Form Recognizer モデルをトレーニングするために使用します。 |
|[TrainResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.trainresult?view=azure-dotnet-preview)| このクラスは、モデル ID を含む、カスタム モデルのトレーニング操作の結果を提供します。これを使用してフォームを分析できます。 |
|[AnalyzeResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.analyzeresult?view=azure-dotnet-preview)| このクラスは、カスタム モデルの分析操作の結果を提供します。 これには、**ExtractedPage** インスタンスの一覧が含まれます。 |
|[ExtractedPage](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.extractedpage?view=azure-dotnet-preview)| このクラスは、単一のフォーム ドキュメントから抽出されたすべてのデータを表します。|

## <a name="code-examples"></a>コード例

<!--
    Include code snippets and short descriptions for each task you list in the the bulleted list. Briefly explain each operation, but include enough clarity to explain complex or otherwise tricky operations.

    Include links to the service's reference content when introducing a class for the first time
-->

これらのコード スニペットでは、.NET 用 Form Recognizer クライアント ライブラリを使用して次のタスクを実行する方法を示します。

* [クライアントを認証する](#authenticate-the-client)
* [カスタム Form Recognizer モデルをトレーニングする](#train-a-custom-model)
* [カスタム モデルを使用してフォームを分析する](#analyze-forms-with-a-custom-model)
* [カスタム モデルのリストを取得する](#get-a-list-of-custom-models)

### <a name="define-variables"></a>変数の定義

メソッドを定義する前に、次の変数定義を **Program** クラスの先頭に追加します。 一部の変数は自分で設定する必要があります。 

* サービスのエンドポイント値は、Azure portal の **[概要]** セクションで確認できます。 
* トレーニング データの SAS URL を取得するには、Microsoft Azure Storage Explorer を開き、ご利用のコンテナーを右クリックし、 **[Shared Access Signature の取得]** を選択します。 アクセス許可の **[読み取り]** と **[表示]** がオンになっていることを確認し、 **[作成]** をクリックします。 次に、その値を **URL** セクションにコピーします。 それは次の書式になります。`https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_variables)]

### <a name="authenticate-the-client"></a>クライアントを認証する

`Main` メソッドの下に、`Main` で参照されているタスクを定義します。 ここでは、上で定義したサブスクリプション変数を使用してクライアント オブジェクトを認証します。 他のメソッドは後で定義します。

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_maintask)]

### <a name="train-a-custom-model"></a>カスタム モデルをトレーニングする

次のメソッドは、Form Recognizer クライアント オブジェクトを使用して、Azure BLOB コンテナーに格納されているドキュメントの新しい認識モデルをトレーニングします。 このメソッドは、ヘルパー メソッドを使用して、([ModelResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.modelresult?view=azure-dotnet-preview) オブジェクトによって表される) 新しくトレーニングされたモデルに関する情報を表示し、そのモデル ID を返します。

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_train)]

次のヘルパー メソッドは、Form Recognizer モデルに関する情報を表示します。

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_displaymodel)]

### <a name="analyze-forms-with-a-custom-model"></a>カスタム モデルを使用してフォームを分析する

このメソッドは、Form Recognizer クライアントとモデル ID を使用して PDF フォーム ドキュメントを分析し、キーと値のデータを抽出します。 このメソッドは、ヘルパー メソッドを使用して、([AnalyzeResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.analyzeresult?view=azure-dotnet-preview) オブジェクトによって表される) 結果を表示します。

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_analyzepdf)]

次のヘルパー メソッドは、分析操作に関する情報を表示します。

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_displayanalyze)]

### <a name="get-a-list-of-custom-models"></a>カスタム モデルのリストを取得する

自分のアカウントに属するすべてのトレーニング済みモデルのリストを返すことができ、それらがいつ作成されたかに関する情報を取得できます。 モデルのリストは、[ModelsResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.modelsresult?view=azure-dotnet-preview) オブジェクトによって表されます。

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_getmodellist)]

## <a name="run-the-application"></a>アプリケーションの実行

アプリケーション ディレクトリから、`dotnet run` コマンドを呼び出してアプリケーションを実行します。

```console
dotnet run
```

## <a name="clean-up-resources"></a>リソースのクリーンアップ

Cognitive Services サブスクリプションをクリーンアップして削除したい場合は、リソースまたはリソース グループを削除することができます。 リソース グループを削除すると、それに関連付けられている他のリソースも削除されます。

* [ポータル](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>次の手順

このクイックスタートでは、Form Recognizer .NET クライアント ライブラリを使用してカスタム モデルをトレーニングし、フォームを分析しました。 次に、より適切なトレーニング データ セットを作成し、より正確なモデルを生成するためのヒントについて学習します。

> [!div class="nextstepaction"]
>[トレーニング データ セットの作成](../build-training-data-set.md)

* [Form Recognizer とは](../overview.md)
* このサンプルのソース コードは、[GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/FormRecognizer) にあります。
