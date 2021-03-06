---
title: パーティション再分割を使用して Azure Stream Analytics での処理を最適化する
description: この記事では、パーティション再分割を使用して、並列化できない Azure Stream Analytics ジョブを最適化する方法について説明します。
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 07/26/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 9c802e6d23daf502da351549c66a7dae1247c068
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68517352"
---
# <a name="use-repartitioning-to-optimize-processing-with-azure-stream-analytics"></a>パーティション再分割を使用して Azure Stream Analytics での処理を最適化する

この記事では、完全に[並列化](stream-analytics-scale-jobs.md)できないシナリオのために、パーティション再分割を使用して Azure Stream Analytics クエリをスケーリングする方法について説明します。

次の場合は[並列処理](stream-analytics-parallelization.md)を使用できない可能性があります。

* 入力ストリームのパーティション キーを制御できない。
* ソースが入力を複数のパーティションに分散させ、それらを後でマージする必要がある。 

## <a name="how-to-repartition"></a>パーティションを再分割する方法

Event Hubs の **PartitionId** など、自然な入力スキームに従ってシャード化されていないストリームのデータを処理する場合は、パーティション再分割または再シャッフルが必要です。 パーティションを再分割すると、各シャードを個別に処理できるため、ストリーミング パイプラインを直線的にスケールアウトできます。

パーティションを再分割するには、クエリの **PARTITION BY** ステートメントの後にキーワード **INTO** を使用します。 次の例では、**DeviceID** によってデータをパーティション数 10 に分割します。 **DeviceID** のハッシュを使用して、どのパーティションがどのサブストリームを受け入れるかを決定します。 出力がパーティション分割された書き込みをサポートし、10 個のパーティションがあると仮定して、データはパーティション分割されたストリームごとに個別にフラッシュされます。

```sql
SELECT * 
INTO output
FROM input
PARTITION BY DeviceID 
INTO 10
```

次のクエリ例では、パーティション再分割された 2 つのデータ ストリームを結合します。 パーティション再分割された 2 つのデータ ストリームを結合する場合、これらのストリームのパーティション キーとカウントが同じである必要があります。 結果は、同じパーティション スキームを持つストリームです。

```sql
WITH step1 AS (SELECT * FROM input1 PARTITION BY DeviceID INTO 10),
step2 AS (SELECT * FROM input2 PARTITION BY DeviceID INTO 10)

SELECT * INTO output FROM step1 PARTITION BY DeviceID UNION step2 PARTITION BY DeviceID
```

出力スキームでは、各サブストリームを個別にフラッシュできるように、ストリーム スキーム キーとカウントが一致している必要があります。 また、ストリームは、フラッシュする前に別のスキームによって再びマージおよびパーティション再分割することもできますが、この方法は処理の全体的な待機時間が増え、リソースの使用率が増加するため、避けてください。

## <a name="streaming-units-for-repartitions"></a>パーティション再分割のためのストリーミング ユニット

ジョブのリソース使用量について実験して観察し、必要なパーティションの正確な数を判断します。 [ストリーミング ユニット (SU)](stream-analytics-streaming-unit-consumption.md) の数は、各パーティションに必要な物理リソースに応じて調整する必要があります。 一般に、各パーティションには 6 つの SU が必要です。 ジョブに割り当てられているリソースが不足しているときは、ジョブが恩恵を受ける場合にシステムでパーティション再分割が適用されます。

## <a name="repartitions-for-sql-output"></a>SQL 出力のためのパーティション再分割

ジョブで出力に SQL データベースを使用する場合、スループットを最大化するために、最適なパーティション数に一致するように明示的なパーティション再分割を使用します。 SQL は 8 つのライターで最適に動作するため、フラッシュする前またはさらに上流の段階でフローを 8 つに再分割すると、ジョブのパフォーマンスを向上させることができます。 詳細については、「[Azure SQL Database への Azure Stream Analytics の出力](stream-analytics-sql-output-perf.md)」を参照してください。


## <a name="next-steps"></a>次の手順

* [Azure Stream Analytics の使用](stream-analytics-introduction.md)
* [Azure Stream Analytics でのクエリの並列処理の活用](stream-analytics-parallelization.md)