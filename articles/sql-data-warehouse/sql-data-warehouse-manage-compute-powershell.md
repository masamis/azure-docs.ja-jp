---
title: "Azure SQL Data Warehouse のコンピューティング能力の管理 (PowerShell) | Microsoft Docs"
description: "コンピューティング能力を管理するための PowerShell のタスク。 DWU を調整することで、コンピューティング リソースをスケーリングします。 また、コストを節約するために、コンピューティング リソースを一時停止および再開します。"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: f9814fc0011153a25489d60d696bb567edfcebc4
ms.openlocfilehash: a4cde433850ab3627f870b51f5a897c6f846c2f0


---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Azure SQL Data Warehouse のコンピューティング能力の管理 (PowerShell)
> [!div class="op_single_selector"]
> * [概要](sql-data-warehouse-manage-compute-overview.md)
> * [ポータル](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST ()](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
> 
> 

## <a name="before-you-begin"></a>開始する前に
### <a name="install-the-latest-version-of-azure-powershell"></a>Azure PowerShell の最新バージョンのインストール
> [!NOTE]
> SQL Data Warehouse で Azure PowerShell を使用するには、Azure PowerShell バージョン 1.0.3 以降が必要です。  現在のバージョンを確認するには、 **Get-Module -ListAvailable -Name Azure**コマンドを実行します。 最新バージョンは、[Microsoft Web Platform Installer][Microsoft Web Platform Installer] からインストールできます。  詳細については、[Azure PowerShell をインストールして構成する方法][How to install and configure Azure PowerShell]に関する記事をご覧ください。
> 
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a>Azure PowerShell コマンドレットの使用開始
作業を開始するには:

1. Azure PowerShell を開きます。
2. PowerShell プロンプトで、次のコマンドを実行して Azure Resource Manager にサインインし、サブスクリプションを選択します。
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>コンピューティング能力のスケーリング
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

DWU を変更するには、[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase] PowerShell コマンドレットを使用します。 次の例では、MyServer サーバーにホストされているデータベース MySQLDW のサービスレベル目標を DW1000 に設定します。

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>コンピューティングの一時停止
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

データベースを一時停止するには、[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase] コマンドレットを使用します。 次の例では、"Server01" という名前のサーバーにホストされている "Database02" という名前のデータベースが一時停止されます。 サーバーは "ResourceGroup1" という名前の Asure リソース グループ内にあります。

> [!NOTE]
> サーバーが foo.database.windows.net である場合は、PowerShell コマンドレットでサーバー名として "foo" を使用してください。
> 
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
バリエーションの&1; つとして、次の例では $database オブジェクトにデータベースを取り込みます。 オブジェクトは [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase] にパイプ処理されます。 結果は、オブジェクト resultDatabase に格納されます。 最後のコマンドは結果を表示します。

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>コンピューティングの再開
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

データベースを開始するには、[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] コマンドレットを使用します。 次の例では、"Server01" という名前のサーバーにホストされている "Database02" という名前のデータベースが開始されます。 サーバーは "ResourceGroup1" という名前の Asure リソース グループ内にあります。

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

バリエーションの&1; つとして、次の例では $database オブジェクトにデータベースを取り込みます。 オブジェクトは [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] にパイプ処理され、結果が $resultDatabase に格納されます。 最後のコマンドは結果を表示します。

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>次のステップ
他の管理タスクについては、[管理の概要][Management overview]に関する記事をご覧ください。

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/



<!--HONumber=Jan17_HO3-->


