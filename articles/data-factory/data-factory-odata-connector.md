---
title: "OData ソースからデータを移動する | Microsoft Docs"
description: "Azure Data Factory を使用して OData ソースからデータを移動する方法を説明します。"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 4521a236bfc13e6aca7e13e7400c11d353bc3a66
ms.openlocfilehash: 9c385adfa3da73bef2d05352049d1f71aa5c5847
ms.lasthandoff: 12/09/2016


---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Azure Data Factory を使用して OData ソースからデータを移動する
この記事では、Azure Data Factory のコピー アクティビティを使用して、OData ソースと他のデータ ストアとの間でデータを移動する方法について説明します。 この記事は、「 [データ移動アクティビティ](data-factory-data-movement-activities.md) 」という記事に基づき、コピー アクティビティによるデータ移動の一般概要とサポートされるデータ ストアの組み合わせについて紹介しています。

## <a name="supported-versions-and-authentication-types"></a>サポートされているバージョンと認証の種類
この OData コネクタは OData バージョン 3.0 および 4.0 をサポートしているため、クラウド OData とオンプレミス OData のどちらのソースのデータもコピーできます。 後者の場合は、Data Management Gateway のインストールが必要になります。 Data Management Gateway の詳細については、 [オンプレミスとクラウド間でのデータ移動](data-factory-move-data-between-onprem-and-cloud.md) に関する記事を参照してください。

次の認証の種類がサポートされています。

* **クラウド** OData フィードにアクセスするには、匿名認証、基本認証 (ユーザー名とパスワード)、または Azure Active Directory ベースの OAuth 認証を使用できます。
* **オンプレミス** OData フィードにアクセスするには、匿名認証、基本認証 (ユーザー名とパスワード)、または Windows 認証を使用できます。

## <a name="copy-data-wizard"></a>データのコピー ウィザード
OData ソースとの間でデータをコピーするパイプラインを作成する最も簡単な方法は、データのコピー ウィザードを使用することです。 データのコピー ウィザードを使用してパイプラインを作成する簡単な手順については、「 [チュートリアル: コピー ウィザードを使用してパイプラインを作成する](data-factory-copy-data-wizard-tutorial.md) 」をご覧ください。

以下の例は、[Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)、[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)、または [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) を使用してパイプラインを作成する際に使用できるサンプルの JSON 定義です。 これらの例は、OData ソースから Azure BLOB ストレージにデータをコピーする方法を示しています。 ただし、Azure Data Factory のコピー アクティビティを使用して、 [こちら](data-factory-data-movement-activities.md#supported-data-stores-and-formats) に記載されているシンクのいずれかにデータをコピーすることができます。

## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>サンプル: OData ソースから Azure Blob にデータをコピーする
このサンプルは、OData ソースから Azure Blob Storage にデータをコピーする方法を示します。 Azure Data Factory のコピー アクティビティを使用して、 **こちら** に記載されているシンクのいずれかにデータを [直接](data-factory-data-movement-activities.md#supported-data-stores-and-formats) コピーすることもできます。  

このサンプルでは、次の Data Factory のエンティティがあります。

1. [OData](#odata-linked-service-properties)型のリンクされたサービス。
2. [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)型のリンクされたサービス。
3. [ODataResource](#odata-dataset-type-properties) 型の入力[データセット](data-factory-create-datasets.md)。
4. [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) 型の出力[データセット](data-factory-create-datasets.md)。
5. [RelationalSource](#odata-copy-activity-type-properties) と [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) を使用するコピー アクティビティを含む[パイプライン](data-factory-create-pipelines.md)

サンプルでは、1 時間ごとに OData ソースに対するクエリの結果のデータを Azure Blob にコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。

**OData のリンクされたサービス** 次のサンプルは匿名認証を使用しています。 使用可能なさまざまな種類の認証については、 [ODBC のリンクされたサービス](#odata-linked-service-properties) に関するセクションをご覧ください。

```json
    {
        "name": "ODataLinkedService",
           "properties":
        {
            "type": "OData",
               "typeProperties":
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
               }
           }
    }
```

**Azure Storage のリンクされたサービス**

```json
    {
          "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
          }
    }
```

**OData の入力データセット**

”external” を ”true” に設定すると、データセットが Data Factory の外部にあり、Data Factory のアクティビティによって生成されたものではないことが Data Factory サービスに通知されます。

```json
    {
        "name": "ODataDataset",
        "properties":
        {
            "type": "ODataResource",
            "typeProperties":
            {
                 "path": "Products"
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3                
            }
        }
    }
```

データセット定義での **パス** の指定は省略可能です。

**Azure BLOB の出力データセット**

データは新しい BLOB に 1 時間おきに書き込まれます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスは、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダー パスは開始時間の年、月、日、時刻の部分を使用します。

```json
    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
```


**コピー アクティビティのあるパイプライン**

パイプラインには、入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティが含まれています。 パイプライン JSON 定義で、**source** 型が **RelationalSource** に設定され、**sink** 型が **BlobSink** に設定されています。 **query** プロパティに指定された SQL クエリは、OData ソースの最新のデータを選択します。

```json
    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }
```

パイプライン定義での **クエリ** の指定は省略可能です。 Data Factory サービスでデータの取得に使用する **URL** は次の形式になります: リンクされたサービスに指定した URL (必須) + データセットに指定したパス (オプション) + パイプラインのクエリ (オプション)。

## <a name="odata-linked-service-properties"></a>OData のリンクされたサービスのプロパティ
次の表は、OData のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| type |type プロパティを **OData** |はい |
| URL |OData サービスの URL です。 |はい |
| authenticationType |OData ソースへの接続に使用される認証の種類です。 <br/><br/> クラウド OData の場合、有効な値は、匿名、基本、または OAuth です (Azure Data Factory で現在サポートされているのは Azure Active Directory ベースの OAuth のみです)。 <br/><br/> オンプレミスの OData では、Anonymous、Basic、Windows のいずれかの値になります。 |はい |
| username |基本認証を使用している場合は、ユーザー名を指定します。 |はい (基本認証を使用している場合のみ) |
| パスワード |ユーザー名に指定したユーザー アカウントのパスワードを指定します。 |はい (基本認証を使用している場合のみ) |
| authorizedCredential |OAuth を使用している場合は、Data Factory コピー ウィザードまたはエディターの **[承認]** ボタンをクリックして資格情報を入力すると、このプロパティの値が自動生成されます。 |はい (OAuth 認証を使用している場合のみ) |
| gatewayName |Data Factory サービスが、オンプレミスの OData サービスへの接続に使用するゲートウェイの名前。 オンプレミスの OData ソースからデータをコピーする場合にのみ指定します。 |いいえ |

### <a name="using-basic-authentication"></a>基本認証を使用する
```json
    {
        "name": "inputLinkedService",
        "properties":
        {
            "type": "OData",
               "typeProperties":
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
               "username": "username",
               "password": "password"
           }
       }
    }
```

### <a name="using-anonymous-authentication"></a>匿名認証を使用する
```json
    {
        "name": "ODataLinkedService",
           "properties":
        {
            "type": "OData",
            "typeProperties":
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }
```

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>オンプレミス OData ソースにアクセスする際に Windows 認証を使用する
```json
    {
        "name": "inputLinkedService",
        "properties":
        {
            "type": "OData",
               "typeProperties":
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
               "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }
```

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a>クラウド OData ソースにアクセスする際に OAuth 認証を使用する
```json
    {
        "name": "inputLinkedService",
        "properties":
        {
            "type": "OData",
               "typeProperties":
            {
               "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
               "authenticationType": "OAuth",
               "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
           }
       }
    }
```

## <a name="odata-dataset-type-properties"></a>OData データセットの type プロパティ
データセットの定義に利用できるセクションとプロパティの完全な一覧については、「[データセットの作成](data-factory-create-datasets.md)」という記事を参照してください。 データセット JSON の構造、可用性、ポリシーなどのセクションは、データセットのすべての型 (Azure SQL、Azure BLOB、Azure テーブルなど) でほぼ同じです。

**typeProperties** セクションはデータセット型ごとに異なり、データ ストアのデータの場所などに関する情報を提供します。 **ODataResource** 型のデータセット (OData データセットを含む) の typeProperties セクションには次のプロパティがあります。

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| パス |OData リソースへのパス |いいえ |

## <a name="odata-copy-activity-type-properties"></a>OData のコピー アクティビティの type プロパティ
アクティビティの定義に利用できるセクションとプロパティの完全な一覧については、[パイプラインの作成](data-factory-create-pipelines.md)に関する記事を参照してください。 名前、説明、入力テーブル、出力テーブル、ポリシーなどのプロパティは、あらゆる種類のアクティビティで使用できます。

一方、アクティビティの typeProperties セクションで使用できるプロパティは、各アクティビティの種類によって異なります。 コピー アクティビティの場合、ソースとシンクの種類によって異なります。

source の種類が **RelationalSource** (OData を含む) である場合は、typeProperties セクションで次のプロパティを使用できます。

| プロパティ | 説明 | 例 | 必須 |
| --- | --- | --- | --- |
| query |カスタム クエリを使用してデータを読み取ります。 |"?$select=Name, Description&$top=5" |いいえ |

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>OData の型マッピング
[データ移動アクティビティ](data-factory-data-movement-activities.md) に関する記事のとおり、コピー アクティビティは次の 2 段階のアプローチで型を source から sink に自動的に変換します。

1. ネイティブの source 型から .NET 型に変換する
2. .NET 型からネイティブの sink 型に変換する

OData データ ストアからデータを移動するとき、OData データ型は .NET 型にマップされます。

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>パフォーマンスとチューニング
Azure Data Factory でのデータ移動 (コピー アクティビティ) のパフォーマンスに影響する主な要因と、パフォーマンスを最適化するための各種方法については、「[コピー アクティビティのパフォーマンスとチューニングに関するガイド](data-factory-copy-activity-performance.md)」を参照してください。

