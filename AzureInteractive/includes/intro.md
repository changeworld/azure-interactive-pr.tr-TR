---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 0f86f2698a3a0c1e20272c335b63faf03b4b92d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460033"
---
<span data-ttu-id="7fd57-103">Bu öğreticide HTML tabanlı kullanıcı arabirimine sahip basit bir web uygulaması dağıtacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7fd57-103">In this tutorial, you deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="7fd57-104">Sunucusuz arka uç, uygulamanın görüntüleri yüklemesini ve otomatik olarak bunları açıklayan resim yazıları almasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7fd57-104">A serverless backend enables the application to upload images and automatically get captions describing them.</span></span>

![Web uygulamasını çalıştırma](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

<span data-ttu-id="7fd57-106">Aşağıdaki diyagramda, uygulamada kullanılan Azure hizmetleri gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="7fd57-106">The following diagram shows the Azure services used by the application:</span></span>

1. <span data-ttu-id="7fd57-107">Blob Depolama, statik web içeriğini (HTML, CSS, JS) sunar ve görüntüleri depolar.</span><span class="sxs-lookup"><span data-stu-id="7fd57-107">Blob Storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="7fd57-108">Azure İşlevleri görüntü yükleme, yeniden boyutlandırma ve meta veri depolama işlemlerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="7fd57-108">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="7fd57-109">Cosmos DB, görüntü meta verilerini depolar.</span><span class="sxs-lookup"><span data-stu-id="7fd57-109">Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="7fd57-110">Logic Apps, Görüntü İşleme API'sinden resim yazılarını alır.</span><span class="sxs-lookup"><span data-stu-id="7fd57-110">Logic Apps gets image captions from Computer Vision API.</span></span>
5. <span data-ttu-id="7fd57-111">Azure Active Directory, kullanıcı kimlik doğrulamasını yönetir.</span><span class="sxs-lookup"><span data-stu-id="7fd57-111">Azure Active Directory manages user authentication.</span></span>

![Çözüm mimarisi diyagramı](media/functions-first-serverless-web-app/0-architecture.jpg)

<span data-ttu-id="7fd57-113">Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="7fd57-113">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="7fd57-114">Azure Blob depolamasını statik bir web sitesini ve yüklenen görüntüleri barındıracak şekilde yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="7fd57-114">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="7fd57-115">Azure İşlevleri'ni kullanarak Azure Blob depolamasına görüntü yükleme.</span><span class="sxs-lookup"><span data-stu-id="7fd57-115">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="7fd57-116">Azure İşlevleri'ni kullanarak görüntüleri yeniden boyutlandırma.</span><span class="sxs-lookup"><span data-stu-id="7fd57-116">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="7fd57-117">Görüntü meta verilerini Azure Cosmos DB'de depolama.</span><span class="sxs-lookup"><span data-stu-id="7fd57-117">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="7fd57-118">Bilişsel Hizmetler Görüntü İşleme API'sini kullanarak görüntü resim yazılarını otomatik olarak oluşturma.</span><span class="sxs-lookup"><span data-stu-id="7fd57-118">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="7fd57-119">Azure Active Directory ile kullanıcıların kimliğini doğrulayarak web uygulamasının güvenliğini sağlama.</span><span class="sxs-lookup"><span data-stu-id="7fd57-119">Use Azure Active Directory to secure the web app by authenticating users.</span></span>
