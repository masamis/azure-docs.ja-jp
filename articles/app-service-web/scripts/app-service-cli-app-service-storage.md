---
title: "Azure CLI のサンプル スクリプト - ストレージ アカウントへの Web アプリの接続 | Microsoft Docs"
description: "Azure CLI のサンプル スクリプト - ストレージ アカウントへの Web アプリの接続"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 02/21/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: b75744aa48b14524e0963da4461cf575bce25a00
ms.lasthandoff: 03/11/2017

---

# <a name="connect-a-web-app-to-a-storage-account"></a>Web アプリをストレージ アカウントに接続する

このシナリオでは、Azure ストレージ アカウントと Azure Web アプリの作成方法について説明します。 作成後、アプリの設定でストレージ アカウントを Web アプリにリンクします。

このスクリプトを実行する前に、`az login` コマンドを使用して Azure との接続が作成されていることを確認してください。

このサンプルは、bash シェルに対応しています。 Azure CLI スクリプトを Windows クライアントで実行する方法については、[Windows での Azure CLI の実行](../../virtual-machines/virtual-machines-windows-cli-options.md)に関する記事を参照してください。

## <a name="sample-script"></a>サンプル スクリプト

[!code-azurecli[main](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure ストレージ")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトでは、次のコマンドを使用して、リソース グループ、Web アプリ、ストレージ アカウント、およびすべての関連リソースを作成します。 表内の各コマンドは、それぞれのドキュメントにリンクされています。

| コマンド | メモ |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | すべてのリソースを格納するリソース グループを作成します。 |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | App Service プランを作成します。 App Service プランとは、Azure Web アプリ用のサーバー ファームのようなものです。 |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | App Service プラン内に Azure Web アプリを作成します。 |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#create) | ストレージ アカウントを作成します。 これは、静的なアセットを格納する場所になります。 |
| [az storage account show-connection-string](https://docs.microsoft.com/cli/azure/storage/account#show-connection-string) | |
| [az appservice web config appsetings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) | Azure Web アプリのアプリケーション設定を作成または更新します。 アプリケーション設定は、アプリの環境変数として公開されます。 |

## <a name="next-steps"></a>次のステップ

Azure CLI の詳細については、[Azure CLI のドキュメント](https://docs.microsoft.com/cli/azure/overview)のページをご覧ください。

その他の App Service の CLI サンプル スクリプトは、[Azure App Service のドキュメント](../app-service-cli-samples.md)のページにあります。
