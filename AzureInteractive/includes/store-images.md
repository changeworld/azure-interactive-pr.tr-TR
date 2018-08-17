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
ms.openlocfilehash: 2202cdebe77668972372983a0e802d00edabf6dd
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079691"
---
<span data-ttu-id="1ff3d-103">Azure Cosmos DB, Microsoft’un sunucusuz, genel olarak dağıtılan çok modelli veritabanıdır.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-103">Azure Cosmos DB is Microsoft's serverless, globally distributed, multi-model database.</span></span> <span data-ttu-id="1ff3d-104">Bu modülde Cosmos DB'de görüntü meta verilerini JSON belgesi olarak depolamayı ve almayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-104">In this module, you learn how to use Azure Functions to store and retrieve image metadata as JSON documents in Cosmos DB.</span></span>

## <a name="create-a-cosmos-db-account-database-and-collection"></a><span data-ttu-id="1ff3d-105">Cosmos DB hesabı, veritabanı ve koleksiyonu oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ff3d-105">Create a Cosmos DB account, database, and collection</span></span>

<span data-ttu-id="1ff3d-106">Cosmos DB hesabı, Cosmos DB veritabanlarını içeren bir Azure kaynağıdır.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-106">A Cosmos DB account is an Azure resource that contains Cosmos DB databases.</span></span>

1. <span data-ttu-id="1ff3d-107">Cloud Shell oturumunuzun açık olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-107">Ensure you are still signed into the Cloud Shell.</span></span>  <span data-ttu-id="1ff3d-108">Aksi takdirde **Enter focus mode** (Odak moduna gir) öğesini seçerek bir Cloud Shell penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-108">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="1ff3d-109">Bu öğreticideki diğer kaynaklarla aynı kaynak grubunda benzersiz ada sahip bir Cosmos DB hesabı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-109">Create a Cosmos DB account with a unique name in the same resource group as the other resources in this tutorial.</span></span>

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. <span data-ttu-id="1ff3d-110">Cosmos DB hesabı oluşturulduktan sonra hesapta **imagesdb** adlı yeni bir veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-110">After the Cosmos DB account is created, create a new database named **imagesdb** in the account.</span></span>

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. <span data-ttu-id="1ff3d-111">Veritabanı oluşturulduktan sonra veritabanında 400 istek birimi (RU) performansa sahip **images** adlı yeni bir koleksiyon oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-111">After the database is created, create a new collection named **images** in the database with a throughput of 400 request units (RUs).</span></span>

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-cosmos-db-when-a-thumbnail-is-created"></a><span data-ttu-id="1ff3d-112">Küçük resim oluşturulduğunda Cosmos DB'ye belge kaydetme</span><span class="sxs-lookup"><span data-stu-id="1ff3d-112">Save a document to Cosmos DB when a thumbnail is created</span></span>

<span data-ttu-id="1ff3d-113">Cosmos DB çıkışı bağlaması, Azure İşlevlerini kullanarak Cosmos DB koleksiyonunda belge oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-113">The Cosmos DB output binding lets you create documents in a Cosmos DB collection from Azure Functions.</span></span> <span data-ttu-id="1ff3d-114">Aşağıdaki adımlarda **ResizeImage** işlevindeki Cosmos DB çıkışını yapılandıracak ve işlevi kaydedilecek bir belge (nesne) oluşturacak şekilde değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-114">In the following steps, you configure a Cosmos DB output binding in the **ResizeImage** function and modify the function to return a document (object) to be saved.</span></span>

1. <span data-ttu-id="1ff3d-115">İşlev uygulamasını Azure portalda açın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-115">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="1ff3d-116">Sol gezinti bölmesinde **ResizeImage** işlevini genişletin ve **Tümleştir**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-116">In the left hand navigation, expand the **ResizeImage** function, and then select **Integrate**.</span></span>

1. <span data-ttu-id="1ff3d-117">**Çıkışlar** bölümünde **Yeni Çıkış**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-117">Under **Outputs**, click **New Output**.</span></span>

1. <span data-ttu-id="1ff3d-118">**Azure Cosmos DB** öğesini bulun ve seçin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-118">Find the **Azure Cosmos DB** item and select it.</span></span> <span data-ttu-id="1ff3d-119">Ardından **Seç**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-119">Then click **Select**.</span></span>

    ![Yeni Çıkış'ı seçin](media/functions-first-serverless-web-app/4-new-output.jpg)

1. <span data-ttu-id="1ff3d-121">**Azure Cosmos DB çıkışı** bölümündeki alanları aşağıdaki değerlerle doldurun.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-121">Fill out the fields under **Azure Cosmos DB output** with the following values.</span></span>

    | <span data-ttu-id="1ff3d-122">Ayar</span><span class="sxs-lookup"><span data-stu-id="1ff3d-122">Setting</span></span>      |  <span data-ttu-id="1ff3d-123">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="1ff3d-123">Suggested value</span></span>   | <span data-ttu-id="1ff3d-124">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1ff3d-124">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="1ff3d-125">**Belge parametre adı**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-125">**Document parameter name**</span></span> | <span data-ttu-id="1ff3d-126">**İşlev dönüş değeri kullan**'ı seçin</span><span class="sxs-lookup"><span data-stu-id="1ff3d-126">Select **Use function return value**</span></span> | <span data-ttu-id="1ff3d-127">Metin kutusunun değeri otomatik olarak **$return** olur.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-127">The value of the textbox is automatically set to **$return**.</span></span> |
    | <span data-ttu-id="1ff3d-128">**Veritabanı adı**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-128">**Database name**</span></span> | <span data-ttu-id="1ff3d-129">imagesdb</span><span class="sxs-lookup"><span data-stu-id="1ff3d-129">imagesdb</span></span> | <span data-ttu-id="1ff3d-130">Oluşturduğunuz veritabanının adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-130">Use the name of the database that you created.</span></span> |
    | <span data-ttu-id="1ff3d-131">**Koleksiyon adı**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-131">**Collection name**</span></span> | <span data-ttu-id="1ff3d-132">images</span><span class="sxs-lookup"><span data-stu-id="1ff3d-132">images</span></span> | <span data-ttu-id="1ff3d-133">Oluşturduğunuz koleksiyonun adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-133">Use the name of the collection that you created.</span></span> |

1. <span data-ttu-id="1ff3d-134">**Azure Cosmos DB hesabı bağlantısı**'nın yanında **Yeni**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-134">Next to **Azure Cosmos DB account connection**, click **new**.</span></span> <span data-ttu-id="1ff3d-135">Daha önce oluşturduğunuz Cosmos DB hesabını seçin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-135">Select the Cosmos DB account you previously created.</span></span>

    ![Azure Cosmos DB çıkış bağlaması için ayarları girin](media/functions-first-serverless-web-app/4-cosmos-db-output.png)

1. <span data-ttu-id="1ff3d-137">Cosmos DB çıkış bağlamasını oluşturmak için **Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-137">Click **Save** to create the Cosmos DB output binding.</span></span>

1. <span data-ttu-id="1ff3d-138">Sol taraftaki **ResizeImage** işlevinin adına tıklayarak işlevi açın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-138">Click on the **ResizeImage** function name on the left to open the function.</span></span>

1. <span data-ttu-id="1ff3d-139">**C#**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-139">**C#**</span></span>

    1. <span data-ttu-id="1ff3d-140">(C#) İşlevin dönüş türünü **void** yerine **object** olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-140">(C#) Change the return type of the function from **void** to **object**.</span></span>

    1. <span data-ttu-id="1ff3d-141">(C#) İşlevin sonuna aşağıdaki kod bloğunu ekleyerek kaydedilecek belgenin döndürülmesini sağlayın:</span><span class="sxs-lookup"><span data-stu-id="1ff3d-141">(C#) At the end of the function, add the following code block to return the document to be saved:</span></span>
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![Değişikliklerden sonra run.csx dosyasının ResizeImages işlevi bölümü](media/functions-first-serverless-web-app/4-update-function.png)

1. <span data-ttu-id="1ff3d-143">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-143">**JavaScript**</span></span>

    1. <span data-ttu-id="1ff3d-144">(JavaScript) Cosmos DB'ye kaydedilecek belgeyi döndürmek için `else` yan tümcesindeki `context.done()` deyimini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-144">(JavaScript) Change the `context.done()` statement in the `else` clause to return the document to be saved to Cosmos DB.</span></span>

    ```javascript
    if (error) {
        context.done(error);
    } else {
        context.bindings.thumbnail = stream;
        context.done(null, {
            id: context.bindingData.name,
            imgPath: "/images/" + context.bindingData.name,
            thumbnailPath: "/thumbnails/" + context.bindingData.name
        });
    }
    ```

1. <span data-ttu-id="1ff3d-145">Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-145">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="1ff3d-146">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-146">Click **Save**.</span></span> <span data-ttu-id="1ff3d-147">İşlevin başarıyla kaydedildiğinden ve hata olmadığından emin olmak için günlükler panelini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-147">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="create-a-function-to-list-images-from-cosmos-db"></a><span data-ttu-id="1ff3d-148">Cosmos DB'deki görüntüleri listelemek için işlev oluşturma</span><span class="sxs-lookup"><span data-stu-id="1ff3d-148">Create a function to list images from Cosmos DB</span></span>

<span data-ttu-id="1ff3d-149">Web uygulamasının Cosmos DB'den görüntü meta verilerini alması için bir API gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-149">The web application requires an API to retrieve image metadata from Cosmos DB.</span></span> <span data-ttu-id="1ff3d-150">Aşağıdaki adımlarda:</span><span class="sxs-lookup"><span data-stu-id="1ff3d-150">In the following steps.</span></span> <span data-ttu-id="1ff3d-151">Veritabanı koleksiyonunu sorgulamak için Cosmos DB giriş bağlaması kullanan bir HTTP ile tetiklenen işlev oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-151">uou create an HTTP triggered function that uses a Cosmos DB input binding to query the database collection.</span></span>

1. <span data-ttu-id="1ff3d-152">İşlev uygulamanızda sol taraftaki **İşlevler**'in üzerine gelin ve **+** simgesine tıklayarak yeni bir işlev oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-152">In your function app, hover over **Functions** on the left and click **+** to create a new function.</span></span>

1. <span data-ttu-id="1ff3d-153">**HttpTrigger** şablonunu bulun ve seçin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-153">Find the **HttpTrigger** template and select it.</span></span>

1. <span data-ttu-id="1ff3d-154">Görüntü alma URL'si üreten bir işlev oluşturmak için bu değerleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-154">Use these values to create a function that generates a get images URL.</span></span>

    | <span data-ttu-id="1ff3d-155">Ayar</span><span class="sxs-lookup"><span data-stu-id="1ff3d-155">Setting</span></span>      |  <span data-ttu-id="1ff3d-156">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="1ff3d-156">Suggested value</span></span>   | <span data-ttu-id="1ff3d-157">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1ff3d-157">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="1ff3d-158">**İşlevinizi adlandırın**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-158">**Name your function**</span></span> | <span data-ttu-id="1ff3d-159">GetImages</span><span class="sxs-lookup"><span data-stu-id="1ff3d-159">GetImages</span></span> | <span data-ttu-id="1ff3d-160">Uygulamanın işlevi keşfedebilmesi için bu adı aynı şekilde yazın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-160">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="1ff3d-161">**Yetkilendirme düzeyi**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-161">**Authorization level**</span></span> | <span data-ttu-id="1ff3d-162">Anonim</span><span class="sxs-lookup"><span data-stu-id="1ff3d-162">Anonymous</span></span> | <span data-ttu-id="1ff3d-163">İşleve herkesin erişmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-163">Allow the function to be accessed publicly.</span></span> |

1. <span data-ttu-id="1ff3d-164">**Oluştur**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-164">Click **Create**.</span></span>

1. <span data-ttu-id="1ff3d-165">Yeni işlev oluşturulduktan sonra sol gezinti bölmesinde işlev adının altında **Tümleştir**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-165">When the new function is created, click **Integrate** under the function's name on the left navigation.</span></span>

1. <span data-ttu-id="1ff3d-166">**Yeni Giriş**'e tıklayın ve **Azure Cosmos DB**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-166">Click **New Input** and select **Azure Cosmos DB**.</span></span> 

    ![Yeni Giriş'i seçin](media/functions-first-serverless-web-app/4-new-input.jpg)

1. <span data-ttu-id="1ff3d-168">**Seç**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-168">Click **Select**.</span></span>

1. <span data-ttu-id="1ff3d-169">Aşağıdaki değerleri girin:</span><span class="sxs-lookup"><span data-stu-id="1ff3d-169">Fill out the following values:</span></span>

    | <span data-ttu-id="1ff3d-170">Ayar</span><span class="sxs-lookup"><span data-stu-id="1ff3d-170">Setting</span></span>      |  <span data-ttu-id="1ff3d-171">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="1ff3d-171">Suggested value</span></span>   | <span data-ttu-id="1ff3d-172">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1ff3d-172">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="1ff3d-173">**Belge parametre adı**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-173">**Document parameter name**</span></span> | <span data-ttu-id="1ff3d-174">documents</span><span class="sxs-lookup"><span data-stu-id="1ff3d-174">documents</span></span> | <span data-ttu-id="1ff3d-175">İşlevdeki parametre adı ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-175">Matches parameter name in the function.</span></span> |
    | <span data-ttu-id="1ff3d-176">**Veritabanı adı**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-176">**Database name**</span></span> | <span data-ttu-id="1ff3d-177">imagesdb</span><span class="sxs-lookup"><span data-stu-id="1ff3d-177">imagesdb</span></span> |  |
    | <span data-ttu-id="1ff3d-178">**Koleksiyon adı**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-178">**Collection name**</span></span> | <span data-ttu-id="1ff3d-179">images</span><span class="sxs-lookup"><span data-stu-id="1ff3d-179">images</span></span> |  |
    | <span data-ttu-id="1ff3d-180">**SQL sorgusu**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-180">**SQL query**</span></span> | <span data-ttu-id="1ff3d-181">select \* from c order by c._ts desc</span><span class="sxs-lookup"><span data-stu-id="1ff3d-181">select \* from c order by c._ts desc</span></span> | <span data-ttu-id="1ff3d-182">Belgeleri al, önce son belgeler.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-182">Get documents, latest documents first.</span></span> |
    | <span data-ttu-id="1ff3d-183">**Azure Cosmos DB hesabı bağlantısı**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-183">**Azure Cosmos DB account connection**</span></span> | <span data-ttu-id="1ff3d-184">Var olan bağlantı dizesini seçme</span><span class="sxs-lookup"><span data-stu-id="1ff3d-184">Select existing connection string</span></span> |  |

1. <span data-ttu-id="1ff3d-185">Giriş bağlamasını oluşturmak için **Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-185">Click **Save** to create the input binding.</span></span>

1. <span data-ttu-id="1ff3d-186">**C#**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-186">**C#**</span></span>

    1. <span data-ttu-id="1ff3d-187">İşlevin adına tıklayarak kod penceresini açın ve **run.csx** dosyasının içeriğinin tamamını [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx) dosyasının içeriğiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-187">Click the function's name to open the code window, and then replace all of **run.csx** with the content in [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span></span>

1. <span data-ttu-id="1ff3d-188">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="1ff3d-188">**JavaScript**</span></span>

    1. <span data-ttu-id="1ff3d-189">İşlevin adına tıklayarak kod penceresini açın ve **index.js** dosyasının içeriğinin tamamını [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js) dosyasının içeriğiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-189">Click the function's name to open the code window, and then replace all of **index.js** with the content in [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span></span>

1. <span data-ttu-id="1ff3d-190">Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-190">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="1ff3d-191">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-191">Click **Save**.</span></span> <span data-ttu-id="1ff3d-192">İşlevin başarıyla kaydedildiğinden ve hata olmadığından emin olmak için günlükler panelini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-192">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="1ff3d-193">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="1ff3d-193">Test the application</span></span>

1. <span data-ttu-id="1ff3d-194">Uygulamayı bir tarayıcıda açın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-194">Open the application in a browser.</span></span> <span data-ttu-id="1ff3d-195">Bir görüntü dosyası seçin ve yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-195">Select an image file and upload it.</span></span>

1. <span data-ttu-id="1ff3d-196">Birkaç saniye sonra sayfada yeni görüntünün küçük resmi görünür.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-196">After a few seconds, the thumbnail of the new image appears on the page.</span></span>

1. <span data-ttu-id="1ff3d-197">Azure portaldaki Arama kutusunu kullanarak Cosmos DB hesabınızı adıyla arayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-197">In the Azure portal, use the Search box to search for your Cosmos DB account by name.</span></span> <span data-ttu-id="1ff3d-198">Tıklayarak açın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-198">Click it to open it.</span></span>

1. <span data-ttu-id="1ff3d-199">Koleksiyonlara ve belgelere göz atmak için sol taraftaki **Veri Gezgini**'ne tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-199">Click **Data Explorer** on the left to browse collections and documents.</span></span>

1. <span data-ttu-id="1ff3d-200">**imagesdb** veritabanının altındaki **images** koleksiyonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-200">Under the **imagesdb** database, select the **images** collection.</span></span>

1. <span data-ttu-id="1ff3d-201">Yüklenen görüntü için bir belge oluşturulduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-201">Confirm that a document was created for the uploaded image.</span></span>

    ![Yüklenen görüntü için oluşturulan belgeyi gösteren Veri Gezgini](media/functions-first-serverless-web-app/4-data-explorer.png)



## <a name="summary"></a><span data-ttu-id="1ff3d-203">Özet</span><span class="sxs-lookup"><span data-stu-id="1ff3d-203">Summary</span></span>

<span data-ttu-id="1ff3d-204">Bu modülde Cosmos DB hesabı, veritabanı ve koleksiyonu oluşturmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-204">In this module, you learned how to create a Cosmos DB account, database, and collection.</span></span> <span data-ttu-id="1ff3d-205">Ayrıca Cosmos DB bağlamalarını kullanarak görüntü meta verilerini Cosmos DB koleksiyonuna kaydetmeyi ve oradan almayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-205">You also learned how to use the Cosmos DB bindings to save and retrieve image metadata in the Cosmos DB collection.</span></span> <span data-ttu-id="1ff3d-206">Bir sonraki modülde Microsoft Bilişsel Hizmetler'i kullanarak yüklenen görüntüler için otomatik resim yazısı oluşturmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="1ff3d-206">Next, you learn how to automatically generate a caption for each uploaded image using Microsoft Cognitive Services.</span></span>