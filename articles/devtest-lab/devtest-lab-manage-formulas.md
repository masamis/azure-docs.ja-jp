---
title: "VM を作成するための Azure DevTest Labs 数式の管理 | Microsoft Docs"
description: "Azure DevTest Labs 数式を作成、更新、削除し、新しい VM の作成に使用する方法について説明します。"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 52590f0c47954d115f39cbe988e04a0ed3feb94d
ms.openlocfilehash: cd3352ae30734cce116fda5a838f95e5e0b86a8a
ms.lasthandoff: 02/27/2017


---
# <a name="manage-devtest-labs-formulas-to-create-vms"></a>VM を作成するための DevTest ラボの数式の管理
Azure DevTest Labs における数式とは、仮想マシン (VM) の作成に使用できる既定のプロパティ値の一覧です。 数式から VM を作成する際に、この既定値をそのまま使用することも変更することもできます。 数式は、[カスタム イメージ](devtest-lab-create-template.md)や [Marketplace イメージ](devtest-lab-configure-marketplace-images.md)と同様に、VM を短時間でプロビジョニングするための手法です。  

この記事では、次のタスクを実行する方法について説明します。

* [数式の作成](#create-a-formula)
* [数式を使用した VM のプロビジョニング](#use-a-formula-to-provision-a-vm)
* [数式の変更](#modify-a-formula)
* [数式の削除](#delete-a-formula)

> [!NOTE]
> 数式 - [カスタム イメージ](devtest-lab-create-template.md)のような数式を使用すると、VHD ファイルから基本イメージを作成できます。 基本イメージは、新しい VM のプロビジョニングに使用されます。 どちらの方法がご使用の環境に適しているかを判断するには、「[ のカスタム イメージと数式の比較](devtest-lab-comparing-vm-base-image-types.md)」の記事を参照してください。
> 
> 

## <a name="create-a-formula"></a>数式の作成
DevTest Labs の *ユーザー* アクセス許可が付与されていれば、だれでも、数式をベースとして使用して VM を作成することができます。 数式を作成する方法は&2; つあります。 

* ベースから - 数式のすべての特性を定義するときに使用します。
* 既存のラボ VM から - 既存の VM の設定に基づいて数式を作成するときに使用します。

### <a name="create-a-formula-from-a-base"></a>ベースから数式を作成する
数式を、カスタムイメージ、Marketplace イメージ、またはその他の数式から作成する手順は次のとおりです。

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040)にサインインします。
2. **[その他のサービス]** を選択し、一覧の **[DevTest Labs]** を選択します。
3. ラボの一覧で目的のラボを選択します。  
4. ラボのブレードで、 **[数式 (再利用可能なベース)]**を選択します。
   
    ![Formula menu](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. **[Lab formulas (ラボ数式)]** ブレードで、**[+ Add (+ 追加)]** を選択します。
   
    ![数式の追加](./media/devtest-lab-manage-formulas/add-formula.png)
6. **[ベースの選択]** ブレードで、数式を作成する際のベース (カスタム イメージ、Marketplace イメージ、または数式) を選択します。
   
    ![Base list](./media/devtest-lab-manage-formulas/base-list.png)
7. **[Create formula]** (数式の作成) ブレードで、次の値を指定します。
   
   * **Formula name** (数式の名前) - 数式の名前を入力します。 この値は、VM を作成する際に基本イメージの一覧に表示されます。 入力した名前は検証され、有効でない場合は有効な名前の要件を示すメッセージが表示されます。
   * **説明** - 数式のわかりやすい説明を入力します。 この値は、VM を作成する際に数式のコンテキスト メニューに表示されます。
   * **ユーザー名** - 管理者特権を付与するユーザー名を入力します。
   * **パスワード** - 指定されたユーザーに使用するシークレット (パスワード) に関連付けられている値を入力、またはドロップダウン リストから選択します。  
   * **イメージ** - このフィールドには、前のブレードで選択した基本イメージの名前が表示されます。 
   * **仮想マシン サイズ** - 定義済みの項目の中から、作成する VM のプロセッサ コア、RAM サイズ、ハード ドライブ サイズを指定するものを選びます。
   * **仮想ネットワーク** - 目的の仮想ネットワークを選んでタップします。
   * **サブネット** - 目的のサブネットを指定します。
   * **パブリック IP アドレス** - 選択したサブネットに対してパブリック IP アドレスを許可するようにラボのポリシーが設定されている場合は、**[はい]** または **[いいえ]** を選択して IP アドレスをパブリックにするかどうかを指定します。 パブリック IP アドレスを許可するように設定されていない場合、このオプションは無効になっており、 **[いいえ]**が選択されています。
   * **アーティファクト** - 基本イメージに追加するアーティファクトを選択して構成します。 セキュリティで保護された文字列の値は、数式には保存されません。 そのため、セキュリティで保護された文字列であるアーティファクト パラメーターは表示されません。 
     
       ![[Create formula]](./media/devtest-lab-manage-formulas/create-formula.png)
8. **[作成]** を選択すると、数式が作成されます。

### <a name="create-a-formula-from-a-vm"></a>VM から数式を作成する
既存の VM に基づいて数式を作成する手順は次のとおりです。 

> [!NOTE]
> VM から数式を作成するには、2016 年 3 月 30 日以降に作成された VM である必要があります。 
> 
> 

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040)にサインインします。
2. **[その他のサービス]** を選択し、一覧の **[DevTest Labs]** を選択します。
3. ラボの一覧で目的のラボを選択します。  
4. ラボの **[概要]** ブレードで、数式の作成元にする VM を選択します。
   
    ![Labs VMs](./media/devtest-lab-manage-formulas/my-vms.png)
5. VM のブレードで、 **[数式の作成 (再利用可能なベース)]**を選択します。
   
    ![[Create formula]](./media/devtest-lab-manage-formulas/create-formula-menu.png)
6. **[Create formula (数式の作成)]** ブレードで、新しい数式の**名前**と**説明**を入力します。
   
    ![[Create Formula] (数式の作成) ブレード](./media/devtest-lab-manage-formulas/create-formula-blade.png)
7. **[OK]** を選択して数式を作成します。

## <a name="use-a-formula-to-provision-a-vm"></a>数式を使用した VM のプロビジョニング
数式を作成したら、その数式に基づいて VM を作成できます。 「[アーティファクトを含む VM の追加](devtest-lab-add-vm-with-artifacts.md)」の記事では、そのプロセスについて説明します。

## <a name="modify-a-formula"></a>数式の変更
数式を変更するには、次の手順に従います。

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040)にサインインします。
2. **[その他のサービス]** を選択し、一覧の **[DevTest Labs]** を選択します。
3. ラボの一覧で目的のラボを選択します。  
4. ラボのブレードで、 **[数式 (再利用可能なベース)]**を選択します。
   
    ![Formula menu](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. **[Lab formulas]** (ラボ数式) ブレードで、変更する数式を選択します。
6. **[Update formula (数式の更新)]** ブレードで、必要な編集を行い、**[更新]** を選択します。

## <a name="delete-a-formula"></a>数式の削除
数式を削除するには、次の手順に従います。

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040)にサインインします。
2. **[その他のサービス]** を選択し、一覧の **[DevTest Labs]** を選択します。
3. ラボの一覧で目的のラボを選択します。  
4. ラボの **[設定]** ブレードで、**[Formulas (数式)]** を選択します。
   
    ![Formula menu](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. **[Lab formulas]** (ラボ数式) ブレードで、削除する数式の右側にある省略記号を選択します。
   
    ![Formula menu](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. 数式のコンテキスト メニューで、 **[削除]**を選択します。
   
    ![Formula context menu](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. 削除の確認のダイアログ ボックスで **[はい]** を選択します。

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>関連するブログ記事
* [Custom images or formulas? (カスタム イメージか数式か?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>次のステップ
VM の作成時に使用する数式を作成したら、次は [VM をラボに追加](devtest-lab-add-vm-with-artifacts.md)します。


