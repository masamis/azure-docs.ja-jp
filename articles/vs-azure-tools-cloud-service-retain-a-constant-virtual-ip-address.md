---
title: "クラウド サービスの固定仮想 IP アドレスを保持する方法 | Microsoft Docs"
description: "Azure クラウド サービスの仮想 IP アドレス (VIP) が変化しないようにする方法について説明します。"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: fb9635bfe1111189d72b68b24ac0bfbee8e87203


---
# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>クラウド サービスの固定仮想 IP アドレスを保持する方法
Azure でホストされているクラウド サービスを更新するとき、サービスの仮想 IP アドレス (VIP) が変更されないようにしなければならない場合があります。 ドメイン管理サービスの多くは、ドメイン ネーム システム (DNS) を使用してドメイン名の登録を行います。 DNS が正しく機能するためには、VIP が不変であることが必要です。 Azure ツールの **公開ウィザード** を使用すると、クラウド サービスを更新するときに、その VIP が変更されないようにすることができます。 Cloud Services で DNS ドメイン管理を使用する方法の詳細については、「[Azure クラウド サービスのカスタム ドメイン名の構成](cloud-services/cloud-services-custom-domain-name.md)」を参照してください。

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>VIP を変更せずにクラウド サービスを発行する
運用環境など特定の環境で初めてクラウド サービスを Azure にデプロイするとき、クラウド サービスの VIP が割り当てられます。 その VIP は、デプロイを明示的に削除するか、またはデプロイの更新プロセスによって暗黙的にデプロイが削除されない限り変化しません。 VIP を維持するには、自らデプロイを削除しないようにすると共に、Visual Studio によって自動的にデプロイが削除されないようにする必要があります。 その動作は、**公開ウィザード**に用意されている各種オプションで、デプロイの設定を指定することによって制御できます。 指定できるデプロイとして新規と更新があります。更新デプロイには、増分更新と同時更新とがあり、どちらも VIP が維持されます。 各種デプロイの定義については、[Azure アプリケーションの公開ウィザード](vs-azure-tools-publish-azure-application-wizard.md)に関するページを参照してください。  さらに、エラーが発生した場合に、それまでのクラウド サービスのデプロイを削除するかどうかを制御できます。 そのオプションが正しく設定されていない場合、VIP が予期せず変化する可能性があります。

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>VIP を変更せずにクラウド サービスを更新するには
1. 少なくとも 1 回、クラウド サービスをデプロイした後、Azure プロジェクトのノードのショートカット メニューを開き、 **[公開]**をクリックします。 **Azure アプリケーションの公開** ウィザードが表示されます。
2. サブスクリプションの一覧で、デプロイ対象を選択し、 **[次へ]** をクリックします。 ウィザードの **[設定]** ページが表示されます。
3. **[共通設定]** タブで、デプロイするクラウド サービスの名前、**[環境]**、**[ビルド構成]**、**[サービス構成]** がすべて正しいことを確認します。
4. **[詳細設定]** タブで、ストレージ アカウントとデプロイ ラベルが正しいことと、**[失敗時に配置を削除]** チェック ボックスがオフで、**[配置の更新]** チェック ボックスがオンになっていることを確認します。 **[配置の更新]** チェック ボックスをオンにして、アプリケーションを再発行するときに、デプロイが削除されず、VIP が失われないようにします。 **[失敗時に配置を削除]**チェック ボックスをオフにして、デプロイ中にエラーが発生した場合に、VIP が失われないようにします。
5. ロールの更新方法をさらに詳しく指定するには、**[配置の更新]** ボックスの横にある **[設定]** リンクを選択し、**[配置の更新設定]** ダイアログ ボックスで増分更新または同時更新のオプションを選択します。 増分更新を選択した場合は、インスタンスが 1 つずつ更新されるので、アプリケーションは常時利用できる状態となります。 同時更新を選択した場合は、すべてのインスタンスが同時に更新されます。 同時更新の方が高速ですが、更新処理中はサービスが利用できない可能性があります。
6. 設定が終了したら、 **[次へ]** をクリックします。
7. ウィザードの **[概要]** ページで設定を確認し、**[公開]** をクリックします。
   
   > [!WARNING]
   > デプロイが失敗した場合は、クラウド サービスを異常な状態のまま放置せず、その理由を明らかにして速やかに再デプロイしてください。
   > 
   > 

## <a name="next-steps"></a>次のステップ
Visual Studio から Azure への発行の詳細については、 [Azure アプリケーションの公開ウィザード](vs-azure-tools-publish-azure-application-wizard.md)に関するページをご覧ください。




<!--HONumber=Nov16_HO3-->


