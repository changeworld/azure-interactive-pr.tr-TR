---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a66fcc3a406c79fcf9881ddaaaf8330f5b373043
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460015"
---
Azure hizmetlerini kullanarak başarıyla tam özellikli ve sunucusuz bir uygulama oluşturdunuz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayla işiniz bittiğinde aşağıdaki komutu kullanarak öğretici boyunca oluşturulan tüm kaynakları silebilirsiniz:

```azurecli
az group delete --name first-serverless-app
```

İstendiğinde `y` yazın.  

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Azure Blob depolamasını statik bir web sitesini ve yüklenen görüntüleri barındıracak şekilde yapılandırma.
> * Azure İşlevleri'ni kullanarak Azure Blob depolamasına görüntü yükleme.
> * Azure İşlevleri'ni kullanarak görüntüleri yeniden boyutlandırma.
> * Görüntü meta verilerini Azure Cosmos DB'de depolama.
> * Bilişsel Hizmetler Görüntü İşleme API'sini kullanarak görüntü resim yazılarını otomatik olarak oluşturma.
> * Azure Active Directory ile kullanıcıların kimliğini doğrulayarak web uygulamasının güvenliğini sağlama.

Daha fazla hizmeti İşlevler ile bağlamayı öğrenmek için Logic Apps öğreticisine geçin. 

> [!div class="nextstepaction"]
> [Logic Apps ile İşlevler](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)
