---
title: Azure Application Insights for ASP.NET Core | Microsoft Docs
description: "Web アプリケーションの可用性、パフォーマンス、使用状況を監視します。"
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 08ce387dd37ef2fec8f4dded23c20217a36e9966
ms.openlocfilehash: 4c97ec97d9d12d86796e995c71d8c9bd6c6a3df2


---
# <a name="application-insights-for-aspnet-core"></a>Application Insights for ASP.NET Core
[Application Insights](app-insights-overview.md) を使うと、Web アプリケーションの可用性、パフォーマンス、利用状況を監視できます。 アプリのパフォーマンスと効果に関するフィードバックが得られたら、各開発ライフサイクルにおける設計の方向性について、情報に基づいて選択できます。

![例](./media/app-insights-asp-net-core/sample.png)

[Microsoft Azure](http://azure.com)のサブスクリプションが必要になります。 Windows、XBox Live、またはその他の Microsoft クラウド サービスに与えられる Microsoft アカウントでサインインします。 所属するチームが組織の Azure サブスクリプションを持っている場合は、自分の Microsoft アカウントを使用してサブスクリプションに追加してもらうよう所有者に依頼してください。

## <a name="getting-started"></a>使用の開始
[ファースト ステップ ガイド](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started)に従います。

## <a name="using-application-insights"></a>Application Insights の使用
[Microsoft Azure ポータル](https://portal.azure.com) にサインインし、アプリを監視するために作成したリソースを参照します。

別のブラウザー ウィンドウで、しばらくの間、アプリを使用します。 Application Insights グラフにデータが表示されます  ([更新] のクリックが必要な場合があります)。開発中は少量のデータのみが表示されますが、これらのグラフは、アプリを発行して多数のユーザーが使用したときに真に有用になります。 

[概要] ページには、最も関心が高いと考えられる、サーバーの応答時間、ページの読み込み時間、失敗した要求の数を示すパフォーマンス グラフが表示されます。 グラフの詳細とデータを表示するには、グラフをクリックします。

ポータルのビューは、次の&2; つの主なカテゴリに分類されます。

* [メトリックス エクスプローラー](app-insights-metrics-explorer.md): 応答時間、失敗率、[API](app-insights-api-custom-events-metrics.md) を使用して作成したメトリックスなど、メトリックスと数値のグラフとテーブルを表示します。 アプリとそのユーザーの理解を深めるには、プロパティ値によってデータをフィルター処理してセグメント化します。
* [検索エクスプローラー](app-insights-diagnostic-search.md): 特定の要求、例外、ログ トレース、[API](app-insights-api-custom-events-metrics.md) を使用して作成したイベントなど、個々のイベントを一覧表示します。 イベントのフィルター処理と検索、問題を調査するための関連イベント間の移動を行います。
* [分析](app-insights-analytics.md) は、テレメトリに対して SQL に似たクエリを実行することができる、強力な分析および診断ツールです。

## <a name="alerts"></a>アラート
* 失敗率や他のメトリックスの異常な変化について通知する、[プロアクティブ診断アラート](app-insights-proactive-diagnostics.md)を自動的に取得します。
* 世界中の場所で Web サイトを継続的にテストして、テストが失敗したときにすぐに電子メールを送信するように [可用性テスト](app-insights-monitor-web-app-availability.md) を設定します。
* 応答時間や例外発生率などのメトリックスが許容される限界を超えた場合に、それがわかるように [メトリックス アラート](app-insights-monitor-web-app-availability.md) を設定します。

## <a name="next-steps"></a>次のステップ
* [Web ページにテレメトリを追加](app-insights-javascript.md) して、ページ閲覧状況やパフォーマンスを監視します。
* [依存関係の監視](app-insights-asp-net-dependencies.md) : REST、SQL、その他の外部リソースによる低下が発生しているかどうかを確認します。
* [API の使用](app-insights-api-custom-events-metrics.md) : アプリのパフォーマンスと使用の詳細を表示するための独自のイベントとメトリックスを送信します。
* [可用性テスト](app-insights-monitor-web-app-availability.md) : 世界中からアプリを常にチェックします。 

## <a name="open-source"></a>オープン ソース
[コードを読んで協力してください。](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)




<!--HONumber=Jan17_HO4-->


