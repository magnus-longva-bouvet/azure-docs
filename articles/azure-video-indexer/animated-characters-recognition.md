---
title: Animated character detection with Azure Video Indexer
description: Azure Video Indexer supports detection, grouping, and recognition of characters in animated content via integration with Cognitive Services custom vision. This functionality is available both through the portal and through the API.
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: juliako
---
# Animated character detection 

[!INCLUDE [announcement](./includes/deprecation-announcement.md)]

Azure Video Indexer supports detection, grouping, and recognition of characters in animated content via integration with [Cognitive Services custom vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/). This functionality is available both through the portal and through the API.

After uploading an animated video with a specific animation model, Azure Video Indexer extracts keyframes, detects animated characters in these frames, groups similar character, and chooses the best sample. Then, it sends the grouped characters to Custom Vision that identifies characters based on the models it was trained on. 

Before you start training your model, the characters are detected namelessly. As you add names and train the model the Azure Video Indexer will recognize the characters and name them accordingly.

## Flow diagram

The following diagram demonstrates the flow of the animated character detection process.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/animated-characters-recognition/flow.png" alt-text="Image of a flow diagram ." lightbox="./media/animated-characters-recognition/flow.png":::

## Accounts

Depending on a type of your Azure Video Indexer account, different feature sets are available. For information on how to connect your account to Azure, see [Create an Azure Video Indexer account connected to Azure](connect-to-azure.md).

* The trial account: Azure Video Indexer uses an internal Custom Vision account to create model and connect it to your Azure Video Indexer account. 
* The paid account: you connect your Custom Vision account to your Azure Video Indexer account (if you don’t already have one, you need to create an account first).

### Trial vs. paid

|Functionality|Trial|Paid|
|---|---|---|
|Custom Vision account|Managed behind the scenes by Azure Video Indexer. |Your Custom Vision account is connected to Azure Video Indexer.|
|Number of animation models|One|Up to 100 models per account (Custom Vision limitation).|
|Training the model|Azure Video Indexer trains the model for new characters additional examples of existing characters.|The account owner trains the model when they're ready to make changes.|
|Advanced options in Custom Vision|No access to the Custom Vision portal.|You can adjust the models yourself in the Custom Vision portal.|

## Use the animated character detection with portal  and API

For details, see [Use the animated character detection with portal and API](animated-characters-recognition-how-to.md).

## Limitations

* Currently, the "animation identification" capability isn't supported in East-Asia region.
* Characters that appear to be small or far in the video may not be identified properly if the video's quality is poor.
* The recommendation is to use a model per set of animated characters (for example per an animated series).

## Next steps

[Azure Video Indexer overview](video-indexer-overview.md)
