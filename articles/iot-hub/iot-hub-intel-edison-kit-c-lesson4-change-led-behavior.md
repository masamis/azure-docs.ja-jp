---
title: "Azure IoT への Intel Edison (C) の接続 - レッスン 4: LED の点滅 | Microsoft Docs"
description: "LED のオンとオフの動作を変更するメッセージをカスタマイズします。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Arduino による LED の制御"
ms.assetid: 9826c55a-0e24-4296-ae54-29b7fe66436a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 475b25f02715a60493e79ecd2170854019dfc4ac
ms.openlocfilehash: 278bdf74e2fa8f7074bb8f5ed8eae2d47402b299
ms.lasthandoff: 01/25/2017


---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>LED のオンとオフの動作の変更
## <a name="what-you-will-do"></a>学習内容
LED のオンとオフの動作を変更するメッセージをカスタマイズします。 問題が発生した場合は、[トラブルシューティングのページ][troubleshooting]で解決策を探してください。

## <a name="what-you-will-learn"></a>学習内容
追加の関数を使用して、LED のオンとオフの動作を変更します。

## <a name="what-you-need"></a>必要なもの
[クラウドからデバイスへのメッセージを受信するサンプル アプリケーションを Intel Edison で実行する][receive-cloud-to-device-messages]作業を正常に完了している必要があります。

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>main.c と gulpfile.js への関数の追加
1. 以下のコマンドを実行して、Visual Studio Code でサンプル アプリケーションを開きます。

   ```bash
   cd Lesson4
   code .
   ```
2. `main.c` ファイルを開き、blinkLED() 関数の後に次の関数を追加します。

   ```c
   static void turnOnLED()
   {
     mraa_gpio_write(context, 1);
   }

   static void turnOffLED()
   {
     mraa_gpio_write(context, 0);
   }
   ```

   ![関数が追加された main.c ファイル](media/iot-hub-intel-edison-lessons/lesson4/updated_app_c.png)

3. `receiveMessageCallback` 関数の `else if` ブロックの前に、以下の条件を追加します。

   ```c
   else if (0 == strcmp((const char*)value, "\"on\""))
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\""))
   {
       turnOffLED();
   }
   ```

   これで、サンプル アプリケーションが応答できるメッセージ経由の命令を増やす構成が完了しました。 "on" 命令では LED がオンになり、"off" 命令ではオフになります。
4. gulpfile.js ファイルを開いて、関数 `sendMessage` の前に新しい関数を追加します。

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   }
   ```

   ![関数を追加した Gulpfile.js ファイル][gulpfile]
5. `sendMessage` 関数の `var message = buildMessage(sentMessageCount);` の行を、以下のスニペットに示した新しい行で置き換えます。

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. 変更をすべて保存します。

### <a name="deploy-and-run-the-sample-application"></a>サンプル アプリケーションのデプロイと実行
次のコマンドを実行して、サンプル アプリケーションを Edison にデプロイして実行します。

```bash
gulp deploy && gulp run
```

LED が&2; 秒間点灯した後、2 秒間消灯します。 最後の "stop" メッセージは、サンプル アプリケーションの実行を停止するためのものです。

![オンとオフ][on-and-off]

ご利用ありがとうございます。 IoT ハブから Edison に送信されるメッセージを正常にカスタマイズしました。

### <a name="summary"></a>概要
このオプションのセクションでは、サンプル アプリケーションが LED のオンとオフの動作を通常とは異なる方法で制御できるようにメッセージをカスタマイズする方法を紹介しました。

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_c.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_c.png

