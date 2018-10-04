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
<span data-ttu-id="304e6-103">Azure hizmetlerini kullanarak başarıyla tam özellikli ve sunucusuz bir uygulama oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="304e6-103">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="304e6-104">Kaynakları temizleme</span><span class="sxs-lookup"><span data-stu-id="304e6-104">Clean up resources</span></span>

<span data-ttu-id="304e6-105">Bu uygulamayla işiniz bittiğinde aşağıdaki komutu kullanarak öğretici boyunca oluşturulan tüm kaynakları silebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="304e6-105">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="304e6-106">İstendiğinde `y` yazın.</span><span class="sxs-lookup"><span data-stu-id="304e6-106">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="304e6-107">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="304e6-107">Next steps</span></span>

<span data-ttu-id="304e6-108">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="304e6-108">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="304e6-109">Azure Blob depolamasını statik bir web sitesini ve yüklenen görüntüleri barındıracak şekilde yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="304e6-109">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="304e6-110">Azure İşlevleri'ni kullanarak Azure Blob depolamasına görüntü yükleme.</span><span class="sxs-lookup"><span data-stu-id="304e6-110">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="304e6-111">Azure İşlevleri'ni kullanarak görüntüleri yeniden boyutlandırma.</span><span class="sxs-lookup"><span data-stu-id="304e6-111">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="304e6-112">Görüntü meta verilerini Azure Cosmos DB'de depolama.</span><span class="sxs-lookup"><span data-stu-id="304e6-112">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="304e6-113">Bilişsel Hizmetler Görüntü İşleme API'sini kullanarak görüntü resim yazılarını otomatik olarak oluşturma.</span><span class="sxs-lookup"><span data-stu-id="304e6-113">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="304e6-114">Azure Active Directory ile kullanıcıların kimliğini doğrulayarak web uygulamasının güvenliğini sağlama.</span><span class="sxs-lookup"><span data-stu-id="304e6-114">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="304e6-115">Daha fazla hizmeti İşlevler ile bağlamayı öğrenmek için Logic Apps öğreticisine geçin.</span><span class="sxs-lookup"><span data-stu-id="304e6-115">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="304e6-116">Logic Apps ile İşlevler</span><span class="sxs-lookup"><span data-stu-id="304e6-116">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)
