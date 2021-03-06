---
title: Comparação de exemplos de migração dos serviços de mídia v2 para v3
description: Um conjunto de amostras para ajudá-lo a comparar as diferenças de código entre os serviços de mídia do Azure v2 para v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 03/25/2021
ms.author: inhenkel
ms.openlocfilehash: feb2c83ee7edc3ab22b7b8031e6eb07ef65f9908
ms.sourcegitcommit: f0a3ee8ff77ee89f83b69bc30cb87caa80f1e724
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2021
ms.locfileid: "105558934"
---
# <a name="media-services-migration-code-sample-comparison"></a>Comparação de exemplo de código de migração dos serviços de mídia

![logotipo do guia de migração](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

## <a name="compare-the-sdks"></a>Comparar os SDKs

Você pode usar alguns dos nossos exemplos de código para comparar a maneira como as coisas são feitas entre SDKs.

## <a name="samples-for-comparison"></a>Exemplos de comparação

A tabela a seguir é uma lista de exemplos de comparação entre V2 e V3 para cenários comuns.

|Cenário|API v2|API v3|
|---|---|---|
|Criar um ativo e enviar um arquivo |[exemplo de .NET v2](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[exemplo de .NET v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|Enviar um trabalho|[exemplo de .NET v2](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[exemplo de .NET v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>Mostra como criar primeiro uma transformação e, em seguida, enviar uma tarefa.|
|Publicar um ativo com criptografia AES |1. criar `ContentKeyAuthorizationPolicyOption`<br/>2. criar `ContentKeyAuthorizationPolicy`<br/>3. criar `AssetDeliveryPolicy`<br/>4. criar `Asset` e carregar conteúdo ou enviar `Job` e usar `OutputAsset`<br/>5. associar `AssetDeliveryPolicy` a `Asset`<br/>6. criar `ContentKey`<br/>7. anexar `ContentKey` a `Asset`<br/>8. criar `AccessPolicy`<br/>9. criar `Locator`<br/><br/>[exemplo de .NET v2](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. criar `ContentKeyPolicy`<br/>2. criar `Asset`<br/>3. carregar conteúdo ou usar `Asset` como `JobOutput`<br/>4. criar `StreamingLocator`<br/><br/>[exemplo de .NET v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|
|Obter detalhes do trabalho e gerenciar trabalhos |[Gerenciar trabalhos com v2](../previous/media-services-dotnet-manage-entities.md#get-a-job-reference) |[Gerenciar trabalhos com v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L546)|
