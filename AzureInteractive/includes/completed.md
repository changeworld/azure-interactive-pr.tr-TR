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
<span data-ttu-id="ce24f-103">Azure hizmetlerini kullanarak başarıyla tam özellikli ve sunucusuz bir uygulama oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="ce24f-103">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="ce24f-104">Kaynakları temizleme</span><span class="sxs-lookup"><span data-stu-id="ce24f-104">Clean up resources</span></span>

<span data-ttu-id="ce24f-105">Bu uygulamayla işiniz bittiğinde aşağıdaki komutu kullanarak öğretici boyunca oluşturulan tüm kaynakları silebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ce24f-105">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="ce24f-106">İstendiğinde `y` yazın.</span><span class="sxs-lookup"><span data-stu-id="ce24f-106">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="ce24f-107">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="ce24f-107">Next steps</span></span>

<span data-ttu-id="ce24f-108">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="ce24f-108">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="ce24f-109">Azure Blob depolamasını statik bir web sitesini ve yüklenen görüntüleri barındıracak şekilde yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="ce24f-109">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="ce24f-110">Azure İşlevleri'ni kullanarak Azure Blob depolamasına görüntü yükleme.</span><span class="sxs-lookup"><span data-stu-id="ce24f-110">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="ce24f-111">Azure İşlevleri'ni kullanarak görüntüleri yeniden boyutlandırma.</span><span class="sxs-lookup"><span data-stu-id="ce24f-111">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="ce24f-112">Görüntü meta verilerini Azure Cosmos DB'de depolama.</span><span class="sxs-lookup"><span data-stu-id="ce24f-112">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="ce24f-113">Bilişsel Hizmetler Görüntü İşleme API'sini kullanarak görüntü resim yazılarını otomatik olarak oluşturma.</span><span class="sxs-lookup"><span data-stu-id="ce24f-113">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="ce24f-114">Azure Active Directory ile kullanıcıların kimliğini doğrulayarak web uygulamasının güvenliğini sağlama.</span><span class="sxs-lookup"><span data-stu-id="ce24f-114">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="ce24f-115">Daha fazla hizmeti İşlevler ile bağlamayı öğrenmek için Logic Apps öğreticisine geçin.</span><span class="sxs-lookup"><span data-stu-id="ce24f-115">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="ce24f-116">Logic Apps ile İşlevler</span><span class="sxs-lookup"><span data-stu-id="ce24f-116">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)