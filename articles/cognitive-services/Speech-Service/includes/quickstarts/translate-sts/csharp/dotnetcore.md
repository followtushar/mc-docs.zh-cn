---
title: 快速入门：将语音翻译为语音，C# (.NET Core Windows) - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 12/09/2019
ms.date: 03/23/2020
ms.author: v-tawe
ms.openlocfilehash: 2a7d49a3ae037436c5295e900af778226e17014f
ms.sourcegitcommit: e94ed1c9eff4e88be2ca389909e60b14cc0d92f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084600"
---
## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境](../../../../quickstarts/setup-platform.md?tabs=dotnetcore)
> * [创建空示例项目](../../../../quickstarts/create-project.md?tabs=dotnetcore)

## <a name="add-sample-code"></a>添加示例代码

1. 打开 `Program.cs` 并将其中的所有代码替换为以下内容。

   ```Csharp
   using System;
   using System.Threading.Tasks;
   using Microsoft.CognitiveServices.Speech;
   using Microsoft.CognitiveServices.Speech.Translation;

   namespace helloworld
   {
       class Program
       {
           public static async Task TranslateSpeechToSpeech()
           {
               // Creates an instance of a speech translation config with specified host and subscription key.
               // Replace with your own subscription key and region identifier from here: https://docs.azure.cn/cognitive-services/speech-service/regions
               var config = SpeechTranslationConfig.FromHost(new Uri("wss://YourServiceRegion.stt.speech.azure.cn/"), "YourSubscriptionKey");

               // Sets source and target languages.
               // Replace with the languages of your choice, from list found here: https://docs.azure.cn/cognitive-services/speech-service/language-support#speech-translation
               string fromLanguage = "en-US";
               string toLanguage = "de";
               config.SpeechRecognitionLanguage = fromLanguage;
               config.AddTargetLanguage(toLanguage);

               // Sets the synthesis output voice name.
               // Replace with the languages of your choice, from list found here: https://docs.azure.cn/cognitive-services/speech-service/language-support#text-to-speech
               config.VoiceName = "de-DE-Hedda";

               // Creates a translation recognizer using the default microphone audio input device.
               using (var recognizer = new TranslationRecognizer(config))
               {
                   // Prepare to handle the synthesized audio data.
                   recognizer.Synthesizing += (s, e) =>
                   {
                       var audio = e.Result.GetAudio();
                       Console.WriteLine(audio.Length != 0
                           ? $"AUDIO SYNTHESIZED: {audio.Length} byte(s)"
                           : $"AUDIO SYNTHESIZED: {audio.Length} byte(s) (COMPLETE)");
                   };

                   // Starts translation, and returns after a single utterance is recognized. The end of a
                   // single utterance is determined by listening for silence at the end or until a maximum of 15
                   // seconds of audio is processed. The task returns the recognized text as well as the translation.
                   // Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
                   // shot recognition like command or query.
                   // For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
                   Console.WriteLine("Say something...");
                   var result = await recognizer.RecognizeOnceAsync();

                   // Checks result.
                   if (result.Reason == ResultReason.TranslatedSpeech)
                   {
                       Console.WriteLine($"RECOGNIZED '{fromLanguage}': {result.Text}");
                       Console.WriteLine($"TRANSLATED into '{toLanguage}': {result.Translations[toLanguage]}");
                   }
                   else if (result.Reason == ResultReason.RecognizedSpeech)
                   {
                       Console.WriteLine($"RECOGNIZED '{fromLanguage}': {result.Text} (text could not be translated)");
                   }
                   else if (result.Reason == ResultReason.NoMatch)
                   {
                       Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                   }
                   else if (result.Reason == ResultReason.Canceled)
                   {
                       var cancellation = CancellationDetails.FromResult(result);
                       Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                       if (cancellation.Reason == CancellationReason.Error)
                       {
                           Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                           Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
                           Console.WriteLine($"CANCELED: Did you update the subscription info?");
                       }
                   }
               }
           }

           static void Main(string[] args)
           {
               TranslateSpeechToSpeech().Wait();
           }
       }
   }
   ```

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

1. 在菜单栏中，选择“文件”   > “全部保存”  。

## <a name="build-and-run-the-application"></a>生成并运行应用程序

1. 从菜单栏中，选择“构建”   > “构建解决方案”  以构建应用程序。 现在，编译代码时应不会提示错误。

1. 选择“调试”   > “开始调试”  （或按 F5  ）以启动 helloworld  应用程序。

1. 说一个英语短语或句子。 应用程序将语音传输到语音服务，该服务会将语音翻译并转录为文本（在本例中为德语）。 然后，语音服务会将合成的语音和文本发送回应用程序以供显示。

````
Say something...
AUDIO SYNTHESIZED: 76784 byte(s)
AUDIO SYNTHESIZED: 0 byte(s)(COMPLETE)
RECOGNIZED 'en-US': What's the weather in Seattle?
TRANSLATED into 'de': Wie ist das Wetter in Seattle?
````

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]
