---
title: "Azure CLI のサンプル スクリプト - Linux VM の作成 | Microsoft Docs"
description: "Azure CLI のサンプル スクリプト - Linux VM の作成"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 07d91dfa905d22334bd051f9d5f3d936d38efc88
ms.openlocfilehash: 9532fa1e11d3e85582d77704d1d7cb7f1230eabd
ms.lasthandoff: 02/28/2017

---

# <a name="create-a-fully-configured-virtual-machine"></a>完全に構成された仮想マシンの作成

このスクリプトは、オペレーティング システムが Ubuntu で、関連ネットワーク リソースが設定された Azure 仮想マシンを作成します。 スクリプトが正常に実行されると、この仮想マシンへ SSH 経由でアクセスできるようになります。 

このスクリプトを実行する前に、`az login` コマンドを使用して Azure との接続が作成されていることを確認してください。

このサンプルは、bash シェルに対応しています。 Azure CLI スクリプトを Windows クライアントで実行する方法については、[Windows での Azure CLI の実行](../virtual-machines-windows-cli-options.md)に関する記事を参照してください。

## <a name="sample-script"></a>サンプル スクリプト

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "VM の簡易作成")]

## <a name="clean-up-deployment"></a>デプロイのクリーンアップ 

サンプル スクリプトの実行後、次のコマンドを使用すると、リソース グループ、VM、およびすべての関連リソースを削除できます。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトでは、次のコマンドを使用して、リソース グループ、仮想マシン、およびすべての関連リソースを作成します。 表内の各コマンドは、それぞれのドキュメントにリンクされています。

| コマンド | メモ |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | すべてのリソースを格納するリソース グループを作成します。 |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#create) | Azure 仮想ネットワークとサブネットを作成します。 |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip#create) | 静的 IP アドレスおよび関連付けられる DNS 名を指定してパブリック IP アドレスを作成します。 |
| [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg#create) | インターネットと仮想マシン間のセキュリティ境界となるネットワーク セキュリティ グループ (NSG) を作成します。 |
| [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | 受信トラフィックを許可する NSG 規則を作成します。 このサンプルでは、SSH トラフィック用にポート 22 を開きます。 |
| [az network nic create](https://docs.microsoft.com/cli/azure/network/nic#create) | 仮想ネットワーク カードを作成し、作成したカードを仮想ネットワーク、サブネット、NSG に接続します。 |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | 仮想マシンを作成し、作成したマシンをネットワーク カード、仮想ネットワーク、サブネット、およびNSG に接続します。 このコマンドでは、使用する仮想マシン イメージと管理者の資格情報も指定します。  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | 入れ子になったリソースすべてを含むリソース グループを削除します。 |

## <a name="next-steps"></a>次のステップ

Azure CLI の詳細については、[Azure CLI のドキュメント](https://docs.microsoft.com/cli/azure/overview)のページをご覧ください。

その他の仮想マシン用の CLI サンプル スクリプトは、[Azure Linux VM のドキュメント](../virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)のページにあります。

