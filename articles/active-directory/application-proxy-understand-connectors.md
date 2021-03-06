---
title: "Azure AD アプリケーション プロキシ コネクタを理解する | Microsoft Docs"
description: "Azure AD アプリケーション プロキシ コネクタの基本について説明します。"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: 41d6f678dba769cf7f949751da8cacf3df7f88c1
ms.lasthandoff: 03/03/2017


---

# <a name="understand-azure-ad-application-proxy-connectors"></a>Azure AD アプリケーション プロキシ コネクタを理解する

この記事では、Azure AD アプリケーション プロキシの "隠し味" であるコネクタについて説明します。 コネクタはシンプルで、デプロイと管理が簡単なうえに、非常に強力です。

> [!NOTE]
> アプリケーション プロキシは、Azure Active Directory の Premium または Basic エディションにアップグレードしている場合にのみ利用できる機能です。 詳細については、「 [Azure Active Directory のエディション](active-directory-editions.md)」をご覧ください。

## <a name="what-are-azure-ad-application-proxy-connectors"></a>Azure AD アプリケーション プロキシ コネクタとは
アプリケーション プロキシは、社内ネットワークに "コネクタ" と呼ばれる Windows Server サービスをインストールすることによって機能します。 コネクタは、高可用性と拡張性のニーズに基づいてインストールできます。 コネクタ&1; つから始めて、必要に応じてさらに追加することができます。 コネクタをインストールするたびに、テナントで機能するコネクタのプールに追加されます。

特に小規模なデプロイでは可能ですが、アプリケーション サーバーにはコネクタをインストールしないことをお勧めします。

使用しなくなったコネクタを削除する必要はありません。 コネクタが動作してサービスに接続すると、アクティブな状態が保たれます。 使用していないコネクタは_非アクティブ_としてタグ付けされ、非アクティブな状態が 10 日間続くと削除されます。 

Azure AD の接続の問題を解決する方法については、「[How to troubleshoot Azure AD Application Proxy connectivity problems (Azure AD アプリケーション プロキシの接続に関する問題のトラブルシューティング方法)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/03/24/how-to-troubleshoot-azure-ad-application-proxy-connectivity-problems)」を参照してください。 

## <a name="what-are-the-cloud-rules-for-connectors"></a>コネクタのクラウド規則とは
コネクタとサービスは、高可用性のすべてのタスクに対処します。 コネクタは動的に追加したり削除できます。 新しい要求を受信するたびに、現在使用できるコネクタのいずれかにルーティングされます。 コネクタが一時的に使用できない場合は、トラフィックに応答しません。

コネクタはステートレスであり、サービス設定への接続とコネクタを認証する証明書以外には、コンピューターに構成データが格納されません。 サービスに接続すると、すべての必要な構成データをプルして、数分ごとに更新します。
また、サーバーをポーリングして、新しいバージョンのコネクタがあるかどうかを調べます。 新しいバージョンが見つかると、コネクタは自身を更新します。

イベント ログとパフォーマンス カウンターを使用するか、下に示すようにクラウドからコネクタの状態ページを使用して、コネクタを実行しているコンピューターからコネクタを監視することができます。

 ![Azure AD アプリケーション プロキシ コネクタ](./media/application-proxy-understand-connectors/app-proxy-connectors.png)

## <a name="all-networking-is-outbound"></a>すべてのネットワークがアウトバウンド
コネクタは送信要求のみを送信するため、接続は常にコネクタによって開始されます。 セッションが確立するとトラフィックは両方向に流れるため、受信ポートを開く必要はありません。

送信トラフィックは、アプリケーション プロキシ サービスと公開済みアプリケーションに送信されます。 サービスへのトラフィックは、いくつかの異なるポート番号の Azure データセンターに送信されます。 詳しくは、「[Azure Portal でアプリケーション プロキシを有効にする](active-directory-application-proxy-enable.md)」をご覧ください。

送信トラフィックしかないため、コネクタ間で負荷分散をセットアップしたり、ファイアウォール経由で受信アクセスを構成する必要はありません。

送信トラフィックのファイアウォール規則を構成する方法の詳細については、「[既存のオンプレミス プロキシ サーバーと連携する](application-proxy-working-with-proxy-servers.md)」を参照してください。

[Azure AD アプリケーション プロキシ コネクタ ポート テスト ツール](https://aadap-portcheck.connectorporttest.msappproxy.net/)を使った、コネクタがアプリケーション プロキシ サービスにアクセスできることを確認します。 少なくとも、米国中部リージョンと自分に最も近いリージョンにすべて緑色のチェックマークが表示されていることを確認します。 さらに、緑色のチェックマークが多いほど、リカバリ性が高いことを意味します。 

## <a name="network-security"></a>ネットワークのセキュリティ

コネクタは、サービスとバックエンド アプリケーションの両方に要求を送信することを許可するネットワーク上の任意の場所にインストールできます。 インストールされたコネクタは、社内ネットワークや DMZ (非武装地帯) 内、またはアプリへのアクセスを持つクラウドで実行される仮想マシン上でも正常に動作します。

通常 DMZ へのデプロイは、より複雑です。 ただし、コネクタを DMZ にデプロイする理由の&1; つは、DMZ で実行されるコンポーネント (バックエンド アプリケーションのロード バランサーや侵入検知システムのようなセキュリティ制御など) に使用できる他のインフラストラクチャを利用することです。

## <a name="domain-joining"></a>ドメイン参加

コネクタは、ドメインに参加していないコンピューターで実行できます。 ただし、統合 Windows 認証 (IWA) を使用するアプリケーションにシングル サインオン (SSO) の使用を選択した場合は、ドメイン参加済みのコンピューターが必要です。 

この場合、コネクタを実行するコンピューターは、公開済みアプリケーションの関連ユーザーの代わりに [Kerberos](https://web.mit.edu/kerberos) の制約付き委任を実行できるドメインに参加する必要があります。

コネクタは、部分的な信頼関係のあるドメインやフォレスト、または読み取り専用ドメイン コントローラー (RODC) に参加することもできます。

## <a name="connectors-on-hardened-environments"></a>制限の厳しい環境でのコネクタ

ほとんどの場合、コネクタのデプロイはとても簡単で、特別な構成は必要ありません。 ただし、考慮すべき特有の条件がいくつかあります。

* 送信トラフィックを制限する組織は、こちらの手順に従って必要なポートを開く必要があります。
* FIPS 準拠のコンピューターは、コネクタ サービス、コネクタ アップデーター サービス、およびそのインストーラーが、証明書を生成してコンピューターに格納することを許可するよう、構成を変更することが必要な場合があります。
* ネットワーク要求を発行するプロセスに基づいて環境をロック ダウンしている組織では、コネクタ サービスとコネクタ アップデーター サービスの両方が、すべての必要なポートと IP アドレスにアクセスできることを確認する必要があります。
* 場合によっては、送信/転送プロキシによって証明書の双方向認証が中断されて、通信に失敗することがあります。

## <a name="all-connectors-are-created-almost-equal"></a>すべてのコネクタはほぼ等しく作成されている

すべてのコネクタは互いに同一であると見なされ、それぞれのコネクタにすべての受信要求が届くことがあります。 つまり、すべてのコネクタに同じネットワーク接続があり、[Kerberos](https://web.mit.edu/kerberos) 設定が同じであることが必要です。

コネクタとサービス間のすべての通信は、コネクタを実行するコンピューターに発行されてインストールされたクライアント証明書によって保護されます。 コネクタの証明書の更新方法については、「[Azure Portal でアプリケーション プロキシを有効にする](active-directory-application-proxy-enable.md)」を参照してください。

## <a name="connector-authentication"></a>コネクタの認証

セキュリティで保護されたサービスを提供するために、コネクタはサービスに対して認証を行い、サービスはコネクタに対して認証を行う必要があります。 これを実行するには、コネクタが接続を開始するときにクライアント証明書とサーバー証明書を使用します。 こうすることで、管理者のユーザー名とパスワードがコネクタを実行するコンピューターに保存されることはありません。

使用する証明書は、アプリケーション プロキシ サービス固有のものです。 これらの証明書は新規登録時に作成され、コネクタによって数か月ごとに自動更新されます。 

コネクタが数か月間サービスに接続していないと、証明書の有効期限が切れることがあります。 このような場合は登録が必要なため、コネクタをアンインストールしてから再インストールして、登録をトリガーします。 次の PowerShell コマンドを実行できます。

```
* Import-module AppProxyPSModule
* Register-AppProxyConnector
```

## <a name="performance-and-scalability"></a>パフォーマンスと拡張性

オンライン サービスのスケールは透過的ですが、コネクタに関してはスケールが重要な要因になります。 ピーク時のトラフィックを処理するには十分なコネクタが必要です。 コネクタはステートレスであるため、ユーザー数やセッション数に依存しません。 その代わりに、要求数やペイロード サイズに依存します。 標準的な Web トラフィックでは、平均的なコンピューターによって&1; 秒間に数千件の要求が処理されます。 この数字は、そのコンピューターの特性によって異なります。

コネクタのパフォーマンスは、CPU とネットワークの制約を受けます。 SSLによる暗号化と暗号化解除にはCPU のパフォーマンスが必要であり、Azure でアプリケーションとオンライン サービスへの高速接続を実現するにはネットワークが重要です。 これに対し、コネクタにとってメモリはあまり重要ではありません。

コネクタ サービスでは、可能な限りコネクタの負荷を軽減するよう努めています。 処理の大半はオンライン サービスによって行われ、すべての認証されていないトラフィックが処理されます。 クラウドでできることはすべてクラウドで実行しています。

パフォーマンスに関するその他の要因としては、コネクタと次の項目間のネットワークの品質が挙げられます。

* _オンライン サービス:_ 接続が遅い場合、または待機時間が長い場合は、 サービス レベルに影響を及ぼします。 Express Route 経由で Azure に接続することをお勧めします。 それ以外の場合は、ネットワーク チームにより、Azure への接続が効率的な方法で確実に処理されるようにします。

* _バックエンド アプリケーション:_ コネクタとバックエンド アプリケーション間にプロキシが追加されていることがあります。 このような場合は、コネクタを実行するコンピューターからブラウザーを開き、これらのアプリケーションにアクセスすることで、問題を簡単に解決できます。 コネクタは Azure で実行し、アプリケーションはオンプレミスで実行している場合は、ユーザーが期待するほどのエクスペリエンスが得られないことがあります。
* _ドメイン コントローラー:_ Kerberos の制約付き委任 (KCD) を使用して SSO を実行する場合、コネクタは、バックエンドに要求を送信する前にドメイン コントローラーに接続します。 コネクタには Kerberos チケットのキャッシュがありますが、ビジー状態では、ドメイン コントローラーの応答によりエクスペリエンスの低下が生じることがあります。 これは、コネクタは Azure で実行し、ドメイン コントローラーはオンプレミスで実行する場合によく見られます。

## <a name="automatic-updates-to-the-connector"></a>コネクタの自動更新

コネクタ アップデーター サービスは、自動的に最新の状態を保持する方法を提供します。 このサービスによって、すべての新機能、セキュリティ、およびパフォーマンス向上のメリットを継続的に利用できます。

Azure AD では、デプロイしたすべてのコネクタの自動更新をサポートしています。 アプリケーション プロキシ コネクタ アップデーター サービスを実行している限り、コネクタは自動更新されます。その際ダウンタイムは発生せず、手動による手順は不要です。 サーバーでコネクタ アップデーター サービスが見つからない場合は、更新プログラムを取得するためにコネクタを再インストールする必要があります。 コネクタのインストール方法の詳細については、[セットアップ ドキュメント](https://azure.microsoft.com/en-us/documentation/articles/active-directory-application-proxy-enable.md)を参照してください。

### <a name="updater-impact"></a>アップデーターの影響

_コネクタが&1; つのテナント:_ コネクタが&1; つだけの場合、そのコネクタは最新グループの一部として更新されます。 トラフィックを再ルーティングするその他のコネクタがないため、更新時にはサービスを使用できなくなります。 このようなダウンタイムを回避してより簡単に高可用性を確保するには、2 つめのコネクタをインストールしてコネクタ グループを作成することをお勧めします。 これを実行する方法の詳細については、[コネクタ グループに関するドキュメント](https://azure.microsoft.com/en-us/documentation/articlesactive-directory-application-proxy-connectors.md)を参照してください。

_他のテナント:_ コネクタの更新時に、トラフィックは他のコネクタに再ルーティングされますが、中断は最小限に抑制されます。 ただし、更新が開始されると、進行中のトランザクションに中断が生じることがあります。 お使いのブラウザーによって自動的に操作が再試行され、中断の可能性が明らかにされます。 それ以外の場合は、ページを更新してこれに対処する必要があります。

ダウンタイムが&1; 分間でも生じることは耐えがたいことですが、これらの更新によってさまざまな改善が加えられ、より優れたコネクタを提供することができるため、これは価値あることと考えられます。

コネクタの最新の更新における変更点について、詳しくは[最新の更新](https://azure.microsoft.com/en-us/updates/app-proxy-connector-12sept2016)を参照してください。 このページは更新ごとに改訂されます。

## <a name="under-the-hood"></a>コネクタの性能

Azure では、便利なツールを多数提供しています。 特にコネクタには、多くの役立つ機能があります。 コネクタは Windows Server の Web アプリケーション プロキシに基づいているため、Windows イベント ログと Windows パフォーマンス カウンターの充実したセットを含む、同じ管理ツールのほとんどがコネクタに備わっています。下のイベント ビューアーの表示をご覧ください。

 ![AzureAD イベント ビューアー](./media/application-proxy-understand-connectors/event-view-window.png)

パフォーマンス モニター

 ![AzureAD パフォーマンス モニター](./media/application-proxy-understand-connectors/performance-monitor.png)

コネクタには管理者ログとセッション ログがあります。 管理者ログには主要なイベントとそのエラーが含まれます。 セッション ログには、すべてのトランザクションとその処理の詳細が含まれます。 

ログを参照するには、イベント ビューアーの [表示] メニューで、[分析およびデバッグ ログの表示] を有効にする必要があります。 次に、ログを有効にしてイベントの収集を開始します。 コネクタはより新しいバージョンに基づいているため、Windows Server 2012 R2 の Web アプリケーション プロキシでは管理者ログとセッション ログは表示されません。

 ![AzureAD イベント ビューアー セッション](./media/application-proxy-understand-connectors/event-view-window-session.png)

[サービス] ウィンドウでサービスの状態を確認することができます。 コネクタは、コネクタそのものと更新を処理するサービスの、2 つの Windows サービスで構成されています。 この両方を常に実行する必要があります。

 ![AzureAD サービスのローカル](./media/application-proxy-understand-connectors/aad-connector-services.png)

アプリケーション プロキシ コネクタのエラーを解決する方法については、「[アプリケーション プロキシのトラブルシューティング](https://azure.microsoft.com/en-us/documentation/articles/active-directory-application-proxy-troubleshoot)」を参照してください。

## <a name="next-steps"></a>次のステップ
[既存のオンプレミス プロキシ サーバーと連携する](application-proxy-working-with-proxy-servers.md)

[Azure AD アプリケーション プロキシ コネクタをサイレント インストールする方法](active-directory-application-proxy-silent-installation.md)


