---
title: include dosyası
description: include dosyası
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/21/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 7e51d3cd0533b4fb64d7dfa783af55266d536f54
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079712"
---
<span data-ttu-id="23530-103">Bu noktada uygulama, görüntü yüklemenizi ve görüntülemenizi sağlayan etkin bir galeridir.</span><span class="sxs-lookup"><span data-stu-id="23530-103">At this point, the application is a functional gallery that allows you to upload and view images.</span></span> <span data-ttu-id="23530-104">Bu modülde, Microsoft Bilişsel Hizmetler'in Görüntü İşleme API'sini kullanarak yüklenen görüntüler için resim yazısı oluşturmayı ve bu resim yazılarını Cosmos DB'ye görüntü meta verileriyle birlikte kaydetmeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="23530-104">In this module, you learn how to use the Computer Vision API from Microsoft Cognitive Services to generate captions for uploaded images and save the captions with the image metadata in Cosmos DB.</span></span>

## <a name="create-a-computer-vision-account"></a><span data-ttu-id="23530-105">Görüntü İşleme hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="23530-105">Create a Computer Vision account</span></span>

<span data-ttu-id="23530-106">Microsoft Bilişsel Hizmetler, geliştiricilerin uygulamalarını daha akıllı hale getirmesini sağlamak için kullanımına sunulan bir hizmet koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="23530-106">Microsoft Cognitive Services is a collection of services available to developers to make their applications more intelligent.</span></span> <span data-ttu-id="23530-107">Görüntü İşleme API'si, görüntüleri gelişmiş algoritmalar kullanarak işleyen ve her görüntüyle ilgili bilgileri döndüren sunucusuz bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="23530-107">The Computer Vision API is a serverless service that processes images using advanced algorithms and returns information about each image.</span></span> <span data-ttu-id="23530-108">Ayda en fazla 5000 API çağrısı yapabileceğiniz ücretsiz katmana sahiptir.</span><span class="sxs-lookup"><span data-stu-id="23530-108">It has a free tier that provides up to 5000 API calls per month.</span></span>

1. <span data-ttu-id="23530-109">Cloud Shell oturumunuzun açık olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="23530-109">Ensure you are still signed into the Cloud Shell.</span></span> <span data-ttu-id="23530-110">Aksi takdirde **Enter focus mode** (Odak moduna gir) öğesini seçerek bir Cloud Shell penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="23530-110">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="23530-111">Kaynak grubunuzda **ComputerVision** türünde benzersiz ada sahip yeni bir Bilişsel Hizmetler hesabı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="23530-111">Create a new Cognitive Services account of kind **ComputerVision** with a unique name in your resource group.</span></span> <span data-ttu-id="23530-112">Ücretsiz katman için SKU olarak **F0** kullanın.</span><span class="sxs-lookup"><span data-stu-id="23530-112">For the free tier, use **F0** as the SKU.</span></span> <span data-ttu-id="23530-113">Mevcut bir Görüntü İşleme hesabınız varsa Standart hesap (S1) oluşturmanız gerekebilir ancak bunun için ücret alınabileceğini lütfen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="23530-113">If you already have an existing Computer Vision account, you may need to create a Standard account (S1); however, this may incur some costs.</span></span>

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a><span data-ttu-id="23530-114">Görüntü İşleme URL'si ve anahtarı için İşlev Uygulaması ayarlarını oluşturma</span><span class="sxs-lookup"><span data-stu-id="23530-114">Create Function App settings for Computer Vision URL and key</span></span>

<span data-ttu-id="23530-115">Görüntü İşleme API'sini çağırmak için URL ve anahtar gerekir.</span><span class="sxs-lookup"><span data-stu-id="23530-115">To call the Computer Vision API, a URL and key are required.</span></span>

1. <span data-ttu-id="23530-116">Görüntü İşleme API'si anahtarını ve URL'sini alın ve Bash değişkenlerine kaydedin.</span><span class="sxs-lookup"><span data-stu-id="23530-116">Obtain the Computer Vision API key and URL and save them in bash variables.</span></span>

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. <span data-ttu-id="23530-117">İşlev uygulamasında **COMP_VISION_KEY** ve **COMP_VISION_URL** adlı uygulama ayarlarını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="23530-117">Create app settings named **COMP_VISION_KEY** and **COMP_VISION_URL**, respectively, in the function app.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-computer-vision-api-from-resizeimage-function"></a><span data-ttu-id="23530-118">ResizeImage işlevinden Görüntü İşleme API'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="23530-118">Call Computer Vision API from ResizeImage function</span></span>

<span data-ttu-id="23530-119">Aşağıdaki adımlarda **ResizeImage** işlevini yüklenen görüntüleri tanımlamak ve açıklamayı Cosmos DB'ye kaydetmek için Görüntü İşleme API'sini çağıracak şekilde değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="23530-119">In the following steps, you modify the **ResizeImage** function to call the Computer Vision API to describe each uploaded image and save the description in Cosmos DB.</span></span>

1. <span data-ttu-id="23530-120">İşlev uygulamanızı Azure portalda açın.</span><span class="sxs-lookup"><span data-stu-id="23530-120">Open your function app in the Azure Portal.</span></span>

1. <span data-ttu-id="23530-121">**C#**</span><span class="sxs-lookup"><span data-stu-id="23530-121">**C#**</span></span>

    1. <span data-ttu-id="23530-122">(C#) Sol gezinti bölmesinden **ResizeImage** işlevini bulun ve kod penceresini açın.</span><span class="sxs-lookup"><span data-stu-id="23530-122">(C#) Using the left navigation, locate the **ResizeImage** function and open its code window.</span></span>

    1. <span data-ttu-id="23530-123">(C#) Kodu [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) dosyasının içeriğiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="23530-123">(C#) Replace the code with the contents of [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span></span> <span data-ttu-id="23530-124">Bu kod, Görüntü İşleme API'sini çağırmak için `HttpClient` kullanır ve sonucu Cosmos DB'ye kaydeder.</span><span class="sxs-lookup"><span data-stu-id="23530-124">This code uses `HttpClient` to call Computer Vision API and save its result in Cosmos DB.</span></span>

1. <span data-ttu-id="23530-125">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="23530-125">**JavaScript**</span></span>

    1. <span data-ttu-id="23530-126">(JavaScript) Bu işlevin Görüntü İşleme API'sine HTTP çağrısı yapması için `axios` npm paketi gerekir.</span><span class="sxs-lookup"><span data-stu-id="23530-126">(JavaScript) This function requires the `axios` package from npm to make an HTTP call to the Computer Vision API.</span></span> <span data-ttu-id="23530-127">npm paketini yüklemek için sol gezinti bölmesinde İşlev Uygulamasının adına ve ardından **Platform özellikleri**'ne tıklayın.</span><span class="sxs-lookup"><span data-stu-id="23530-127">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="23530-128">(JavaScript) Konsol penceresini açmak için **Konsol**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="23530-128">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="23530-129">(JavaScript) Konsolda `npm install axios` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="23530-129">(JavaScript) Run the command `npm install axios` in the console.</span></span> <span data-ttu-id="23530-130">İşlemin tamamlanması bir veya iki dakika kadar sürebilir.</span><span class="sxs-lookup"><span data-stu-id="23530-130">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="23530-131">(JavaScript) Sol gezinti bölmesinde işlev adına (**ResizeImage**) tıklayarak işlevi açın, ardından **index.js** dosyasının içeriğinin tamamını [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) dosyasının içeriğiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="23530-131">(JavaScript) Click on the function name (**ResizeImage**) in the left navigation to reveal the function, and then replace all of **index.js** with the contents of [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span></span>

1. <span data-ttu-id="23530-132">Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="23530-132">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="23530-133">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="23530-133">Click **Save**.</span></span> <span data-ttu-id="23530-134">İşlevin başarıyla kaydedildiğinden ve hata olmadığından emin olmak için günlükler panelini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="23530-134">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="23530-135">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="23530-135">Test the application</span></span>

1. <span data-ttu-id="23530-136">Uygulamayı bir tarayıcıda açın.</span><span class="sxs-lookup"><span data-stu-id="23530-136">Open the application in a browser.</span></span> <span data-ttu-id="23530-137">Bir görüntü dosyası seçin ve yükleyin.</span><span class="sxs-lookup"><span data-stu-id="23530-137">Select an image file, and upload it.</span></span>

1. <span data-ttu-id="23530-138">Birkaç saniye sonra sayfada yeni görüntünün küçük resmi görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="23530-138">After a few seconds, the thumbnail of the new image should appear on the page.</span></span> <span data-ttu-id="23530-139">Görüntünün üzerine giderek Görüntü İşleme tarafından oluşturulan açıklamayı görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="23530-139">Hover over the image to see the description generated by Computer Vision.</span></span>


## <a name="summary"></a><span data-ttu-id="23530-140">Özet</span><span class="sxs-lookup"><span data-stu-id="23530-140">Summary</span></span>

<span data-ttu-id="23530-141">Bu modülde Microsoft Bilişsel Hizmetler Görüntü İşleme API'sini kullanarak yüklenen görüntüler için otomatik resim yazısı oluşturmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="23530-141">In this module, you learned how to automatically generate captions for each uploaded image using Microsoft Cognitive Services Computer Vision API.</span></span> <span data-ttu-id="23530-142">Bir sonraki modülde Azure App Service Kimlik Doğrulama hizmetini kullanarak uygulamaya kimlik doğrulama özelliği eklemeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="23530-142">Next, you learn how to add authentication to the application using Azure App Service Authentication.</span></span>