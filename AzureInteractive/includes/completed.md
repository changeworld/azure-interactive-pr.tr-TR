---
title: include dosyası
description: include dosyası
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 1c4aaf7afd9fbc54dd34c2cca13a8b74e947c16a
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079701"
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