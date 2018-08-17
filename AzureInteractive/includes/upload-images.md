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
ms.openlocfilehash: 56cfb4c2893977086309660f4f6941fd0d648913
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079692"
---
<span data-ttu-id="89b01-103">Oluşturmakta olduğunuz uygulama bir fotoğraf galerisidir.</span><span class="sxs-lookup"><span data-stu-id="89b01-103">The application that you're building is a photo gallery.</span></span> <span data-ttu-id="89b01-104">Görüntüleri yüklemek ve görüntülemek üzere API'leri çağırmak için istemci tarafında JavaScript kullanır.</span><span class="sxs-lookup"><span data-stu-id="89b01-104">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="89b01-105">Bu modülde, karşıya görüntü yüklemek için sınırlı süreli URL oluşturan bir sunucusuz işlevi kullanarak bir API oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="89b01-105">In this module, you create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="89b01-106">Web uygulaması, oluşturulan URL ile [Blob depolama REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) kullanarak Blob depolamaya bir görüntü yükler.</span><span class="sxs-lookup"><span data-stu-id="89b01-106">The web application uses the generated URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="89b01-107">Görüntüler için bir blob depolama kapsayıcısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="89b01-107">Create a blob storage container for images</span></span>

<span data-ttu-id="89b01-108">Uygulamanın görüntüleri yüklemesi ve barındırması için ayrı depolama kapsayıcıları gerekir.</span><span class="sxs-lookup"><span data-stu-id="89b01-108">The application requires a separate storage container to upload and host images.</span></span>

1. <span data-ttu-id="89b01-109">Cloud Shell (bash) oturumunuzun açık olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="89b01-109">Ensure you are still signed in to the Cloud Shell (bash).</span></span> <span data-ttu-id="89b01-110">Aksi takdirde **Enter focus mode** (Odak moduna gir) öğesini seçerek bir Cloud Shell penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="89b01-110">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span>

1.  <span data-ttu-id="89b01-111">Depolama hesabınızda tüm bloblara genel erişime sahip **images** adlı yeni bir kapsayıcı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="89b01-111">Create a new container named **images** in your storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-function-app"></a><span data-ttu-id="89b01-112">Azure İşlev uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="89b01-112">Create an Azure Function app</span></span>

<span data-ttu-id="89b01-113">Azure İşlevleri, sunucusuz işlevleri çalıştıran bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="89b01-113">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="89b01-114">Sunucusuz işlev, HTTP isteği gibi olaylar veya depolama kapsayıcısında bir blobun oluşturulması gibi durumlar üzerine tetiklenebilir (çağrılabilir).</span><span class="sxs-lookup"><span data-stu-id="89b01-114">A serverless function can be triggered (called) by events such as an HTTP request or when a blob is created in a storage container.</span></span>

<span data-ttu-id="89b01-115">Azure İşlev uygulaması, bir veya daha fazla sunucusuz işlev için kapsayıcı görevi görür.</span><span class="sxs-lookup"><span data-stu-id="89b01-115">An Azure Function app is a container for one or more serverless functions.</span></span>

1. <span data-ttu-id="89b01-116">Önceden oluşturduğunuz **first-serverless-app** adlı kaynak grubunda benzersiz ada sahip yeni bir Azure İşlevi uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="89b01-116">Create a new Azure Function app with a unique name in the resource group you created earlier named **first-serverless-app**.</span></span> <span data-ttu-id="89b01-117">İşlev uygulamaları için bir Depolama hesabı gerekir. Bu öğreticide var olan depolama hesabını kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="89b01-117">Function apps require a Storage account; in this tutorial, you use the existing storage account.</span></span>

    ```azurecli
    az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
    ```


## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="89b01-118">HTTP ile tetiklenen bir sunucusuz işlev oluşturma</span><span class="sxs-lookup"><span data-stu-id="89b01-118">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="89b01-119">Fotoğraf galerisi web uygulaması, Blob depolamaya görüntüyü güvenli bir şekilde yüklemek için sınırlı süreli URL oluşturma amacıyla sunucusuz işleve bir HTTP isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="89b01-119">The photo gallery web application makes an HTTP request to serverless function to generate a time-limited URL to securely upload an image to Blob storage.</span></span> <span data-ttu-id="89b01-120">İşlev, bir HTTP isteği ile tetiklenir ve Azure Depolama SDK'sını kullanarak güvenli URL'yi oluşturup döndürür.</span><span class="sxs-lookup"><span data-stu-id="89b01-120">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="89b01-121">İşlev uygulaması oluşturulduktan sonra Azure portaldaki Arama kutusunu kullanarak bulun ve tıklayarak açın.</span><span class="sxs-lookup"><span data-stu-id="89b01-121">After the Function app is created, search for it in the Azure Portal using the Search box and click to open it.</span></span>

    ![İşlev uygulamasını açma](media/functions-first-serverless-web-app/2-search-function-app.png)

1. <span data-ttu-id="89b01-123">İşlev uygulaması penceresinin sol tarafındaki gezinti bölmesinde **İşlevler**'in üzerine gelin ve **+** simgesine tıklayarak yeni bir sunucusuz işlev oluşturmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-123">In the function app window's left hand navigation, hover over **Functions** and click **+** to start creating a new serverless function.</span></span>

    ![Yeni bir işlev oluşturma](media/functions-first-serverless-web-app/2-new-function.png)

1. <span data-ttu-id="89b01-125">İşlev şablonlarının listesini görmek için **Özel işlev**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-125">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="89b01-126">**HttpTrigger** şablonunu bulun ve kullanmak istediğiniz dile (C# veya JavaScript) tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-126">Find the **HttpTrigger** template and click the language to use (C# or JavaScript).</span></span>

1. <span data-ttu-id="89b01-127">Blob yükleme URL'si üreten bir işlev oluşturmak için bu değerleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="89b01-127">Use these values to create a function that generates a blob upload URL.</span></span>

    | <span data-ttu-id="89b01-128">Ayar</span><span class="sxs-lookup"><span data-stu-id="89b01-128">Setting</span></span>      |  <span data-ttu-id="89b01-129">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="89b01-129">Suggested value</span></span>   | <span data-ttu-id="89b01-130">Açıklama</span><span class="sxs-lookup"><span data-stu-id="89b01-130">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="89b01-131">**Dil**</span><span class="sxs-lookup"><span data-stu-id="89b01-131">**Language**</span></span> | <span data-ttu-id="89b01-132">C# veya JavaScript</span><span class="sxs-lookup"><span data-stu-id="89b01-132">C# or JavaScript</span></span> | <span data-ttu-id="89b01-133">Kullanmak istediğiniz dili seçin.</span><span class="sxs-lookup"><span data-stu-id="89b01-133">Select the language you want to use.</span></span> |
    | <span data-ttu-id="89b01-134">**İşlevinizi adlandırın**</span><span class="sxs-lookup"><span data-stu-id="89b01-134">**Name your function**</span></span> | <span data-ttu-id="89b01-135">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="89b01-135">GetUploadUrl</span></span> | <span data-ttu-id="89b01-136">Uygulamanın işlevi keşfedebilmesi için bu adı aynı şekilde yazın.</span><span class="sxs-lookup"><span data-stu-id="89b01-136">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="89b01-137">**Yetkilendirme düzeyi**</span><span class="sxs-lookup"><span data-stu-id="89b01-137">**Authorization level**</span></span> | <span data-ttu-id="89b01-138">Anonim</span><span class="sxs-lookup"><span data-stu-id="89b01-138">Anonymous</span></span> | <span data-ttu-id="89b01-139">İşleve herkesin erişmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="89b01-139">Allow the function to be accessed publicly.</span></span> |

    ![Yeni bir HTTP ile tetiklenen işlev için ayarları girin](media/functions-first-serverless-web-app/2-new-function-httptrigger.png)

1. <span data-ttu-id="89b01-141">İşlevi oluşturmak için **Oluştur**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-141">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="89b01-142">**C#**</span><span class="sxs-lookup"><span data-stu-id="89b01-142">**C#**</span></span> 

    1. <span data-ttu-id="89b01-143">İşlevin kaynak kodu göründüğünde **run.csx** dosyasının içeriğinin tamamını [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) dosyasının içeriğiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="89b01-143">When the function's source code appears, replace all of **run.csx** with the content of [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span></span>

1. <span data-ttu-id="89b01-144">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="89b01-144">**JavaScript**</span></span> 

    1. <span data-ttu-id="89b01-145">(JavaScript) Bu işlev, güvenli URL'yi oluşturmak için gerekli olan paylaşılan erişim imzası (SAS) belirtecini üretmek için `azure-storage` npm paketine ihtiyaç duyar.</span><span class="sxs-lookup"><span data-stu-id="89b01-145">(JavaScript) This function requires the `azure-storage` package from npm to generate the shared access signature (SAS) token required to build the secure URL.</span></span> <span data-ttu-id="89b01-146">npm paketini yüklemek için sol gezinti bölmesinde İşlev Uygulamasının adına ve ardından **Platform özellikleri**'ne tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-146">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="89b01-147">(JavaScript) Konsol penceresini açmak için **Konsol**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-147">(JavaScript) Click **Console** to reveal a console window.</span></span>

        ![Konsol penceresi açma](media/functions-first-serverless-web-app/2-open-console.jpg)

    1. <span data-ttu-id="89b01-149">(JavaScript) `cd d:\home\site\wwwroot` komutunu çalıştırarak geçerli dizinin **d:\home\site\wwwroot** olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="89b01-149">(JavaScript) Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

    1. <span data-ttu-id="89b01-150">(JavaScript) `npm init -y` komutunu çalıştırarak boş bir **package.json** dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="89b01-150">(JavaScript) Run the command `npm init -y` to create an empty **package.json** file.</span></span>

    1. <span data-ttu-id="89b01-151">(JavaScript) Konsolda `npm install --save azure-storage` komutunu çalıştırarak paketi yükleyin ve **package.json** içine kaydedin.</span><span class="sxs-lookup"><span data-stu-id="89b01-151">(JavaScript) Run the command `npm install --save azure-storage` in the console to install the package and save it in **package.json**.</span></span> <span data-ttu-id="89b01-152">İşlemin tamamlanması bir veya iki dakika kadar sürebilir.</span><span class="sxs-lookup"><span data-stu-id="89b01-152">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="89b01-153">(JavaScript) Sol gezinti bölmesinde işlev adına (**GetUploadUrl**) tıklayarak işlevi açın, **index.js** dosyasının içeriğinin tamamını [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) dosyasının içeriğiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="89b01-153">(JavaScript) Click on the function name (**GetUploadUrl**) in the left navigation to reveal the function, replace all of **index.js** with the content of [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span></span>
    
        ![Güncelleştirmeden sonra index.js](media/functions-first-serverless-web-app/2-paste-js.jpg)

1. <span data-ttu-id="89b01-155">Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-155">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="89b01-156">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-156">Click **Save**.</span></span> <span data-ttu-id="89b01-157">İşlevin başarıyla derlendiğinden emin olmak için günlükler panelini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="89b01-157">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="89b01-158">İşlev, Blob depolamaya dosya yüklemek için kullanılan paylaşılan erişim imzası (SAS) URL'sini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="89b01-158">The function generates what is called a shared access signature (SAS) URL that is used to upload a file to Blob storage.</span></span> <span data-ttu-id="89b01-159">SAS URL'si yalnızca kısa bir süre boyunca geçerlidir ve yalnızca tek bir dosyanın yüklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="89b01-159">The SAS URL is valid for a short period of time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="89b01-160">[Paylaşılan erişim imzalarını kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) hakkında daha fazla bilgi için Blob depolama belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="89b01-160">Consult the Blob storage documentation for more information on [using shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="89b01-161">Depolama bağlantısı dizesi için ortam değişkeni ekleme</span><span class="sxs-lookup"><span data-stu-id="89b01-161">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="89b01-162">Oluşturduğunuz işlevin SAS URL'sini oluşturabilmesi için Depolama hesabına ait bir bağlantı dizesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="89b01-162">The function you just created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="89b01-163">Bağlantı dizesini işlevin gövdesine sabit kodlamak yerine uygulama ayarı olarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="89b01-163">Instead of hardcoding the connection string in the function's body, it can be stored as an application setting.</span></span> <span data-ttu-id="89b01-164">Ortam değişkenlerinde olduğu gibi uygulama ayarlarına da İşlev uygulamasındaki tüm işlevler tarafından erişim sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="89b01-164">Application settings are accessible as environment variables by all functions in the Function app.</span></span>

1. <span data-ttu-id="89b01-165">Cloud Shell'de Depolama hesabı bağlantı dizesini sorgulayın ve **STORAGE_CONNECTION_STRING** adlı bir Bash değişkenine kaydedin.</span><span class="sxs-lookup"><span data-stu-id="89b01-165">In the Cloud Shell, query the Storage account connection string and save it to a bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="89b01-166">Değişkenin başarıyla ayarlandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-166">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="89b01-167">Bir önceki adımda kaydedilen değeri kullanarak **AZURE_STORAGE_CONNECTION_STRING** adlı yeni bir uygulama ayarı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="89b01-167">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    <span data-ttu-id="89b01-168">Komut çıktısında yeni uygulama ayarının doğru değerle birlikte göründüğünü onaylayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-168">Confirm that the command's output contains the new application setting with the correct value.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="89b01-169">Sunucusuz işlevi test etme</span><span class="sxs-lookup"><span data-stu-id="89b01-169">Test the serverless function</span></span>

<span data-ttu-id="89b01-170">Azure portal, işlevleri oluşturma ve düzenlemeye ek olarak test etmek için de yerleşik bir araç sunar.</span><span class="sxs-lookup"><span data-stu-id="89b01-170">In addition to creating and editing functions, the Azure portal also provides an built-in tool for testing functions.</span></span>

1. <span data-ttu-id="89b01-171">HTTP sunucusuz işlevini test etmek için kod penceresinin sağ tarafındaki **Test** sekmesine tıklayarak test panelini açın.</span><span class="sxs-lookup"><span data-stu-id="89b01-171">To test the HTTP serverless function, click on the **Test** tab on the right of the code window to expand the test panel.</span></span>

1. <span data-ttu-id="89b01-172">**Http yöntemi** değerini **GET** olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="89b01-172">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="89b01-173">**Sorgu** bölümünde *Parametre ekle*'ye tıklayın ve aşağıdaki parametreyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="89b01-173">Under **Query**, click *Add parameter*\* and add the following parameter:</span></span>

    | <span data-ttu-id="89b01-174">ad</span><span class="sxs-lookup"><span data-stu-id="89b01-174">name</span></span>      |  <span data-ttu-id="89b01-175">değer</span><span class="sxs-lookup"><span data-stu-id="89b01-175">value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="89b01-176">filename</span><span class="sxs-lookup"><span data-stu-id="89b01-176">filename</span></span> | <span data-ttu-id="89b01-177">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="89b01-177">image1.jpg</span></span> |

1. <span data-ttu-id="89b01-178">Test panelinde **Çalıştır**'a tıklayarak işleve bir HTTP isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="89b01-178">Click **Run** in the test panel to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="89b01-179">İşlev bir yükleme URL'si döndürür.</span><span class="sxs-lookup"><span data-stu-id="89b01-179">The function returns an upload URL in the output.</span></span> <span data-ttu-id="89b01-180">İşlevin yürütüldüğü günlükler panelinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="89b01-180">The function execution appears in the logs panel.</span></span>

    ![İşlevin başarıyla çalıştığını gösteren günlükler](media/functions-first-serverless-web-app/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a><span data-ttu-id="89b01-182">İşlev uygulamasında CORS'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="89b01-182">Configure CORS in the function app</span></span>

<span data-ttu-id="89b01-183">Uygulamanın ön ucu Blob depolamada barındırıldığından Azure İşlevi uygulamasından farklı bir etki alanı adına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="89b01-183">Because the app's frontend is hosted in Blob storage, it has a different domain name than the Azure Function app.</span></span> <span data-ttu-id="89b01-184">İstemci tarafı JavaScript kodunun oluşturduğunuz işlevi başarıyla çağırmasını sağlamak için işlev uygulamasında çıkış noktaları arası kaynak paylaşımının (CORS) yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="89b01-184">For the client-side JavaScript to successfully call the function you just created, the function app needs to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="89b01-185">İşlev uygulaması penceresinin sol gezinti çubuğunda işlev uygulamasının adına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-185">In the left navigation bar of the function app window, click on the name of your function app.</span></span>

1. <span data-ttu-id="89b01-186">**Platform özellikleri**'ne tıklayarak gelişmiş özellikleri açın.</span><span class="sxs-lookup"><span data-stu-id="89b01-186">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="89b01-187">**API** bölümünde **CORS**'yi tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-187">Under **API**, click **CORS**.</span></span>

    ![CORS seçimi](media/functions-first-serverless-web-app/2-open-cors.jpg)

1. <span data-ttu-id="89b01-189">Uygulama URL'si için bir önceki modülde elde ettiğiniz izin verme kaynağını ekleyin ve sondaki **/** karakterini atın (örneğin: `https://firstserverlessweb.z4.web.core.windows.net`).</span><span class="sxs-lookup"><span data-stu-id="89b01-189">Add an allow origin for the application URL from the previous module, omitting the trailing **/** (for example: `https://firstserverlessweb.z4.web.core.windows.net`).</span></span>

    ![Sunucusuz web uygulaması URL'sinin eklendiğini gösteren CORS ayarları](media/functions-first-serverless-web-app/2-add-cors.png)

1. <span data-ttu-id="89b01-191">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-191">Click **Save**.</span></span>

1. <span data-ttu-id="89b01-192">C#</span><span class="sxs-lookup"><span data-stu-id="89b01-192">C#</span></span>

   1. <span data-ttu-id="89b01-193">(C#) `GetUploadUrl` işlevine geri dönün ve **Tümleştir** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="89b01-193">(C#) Navigate back to the `GetUploadUrl` function, and then select the **Integrate** tab.</span></span>

   1. <span data-ttu-id="89b01-194">(C#) Seçili HTTP metotları bölümünde **OPTIONS** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="89b01-194">(C#) Under Selected HTTP methods, select **OPTIONS**.</span></span>

      <span data-ttu-id="89b01-195">**GET**, **POST** ve **OPTIONS** seçili olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="89b01-195">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="89b01-196">CORS, C# işlevleri için varsayılan olarak seçilmeyen OPTIONS metodunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="89b01-196">CORS uses the OPTIONS method, which is not selected by default for C# functions.</span></span>  

   1. <span data-ttu-id="89b01-197">(C#) **Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-197">(C#) Click **Save**.</span></span>

1. <span data-ttu-id="89b01-198">Portalda işlev uygulamasına gidin, **Genel bakış** sekmesini seçin ve **Yeniden başlat**'a tıklayarak CORS değişikliklerinin uygulanmasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-198">Still in the portal, navigate to the function app, select the **Overview** tab, and then click **Restart** to make sure that the changes for CORS take effect.</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="89b01-199">Depolama hesabında CORS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="89b01-199">Configure CORS in the Storage account</span></span>

<span data-ttu-id="89b01-200">Uygulama dosyaları yüklemek için Blob Depolama’ya istemci tarafında JavaScript çağrıları da gönderdiğinden, Depolama hesabında da CORS yapılandırmasını yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="89b01-200">Because the app also makes client-side JavaScript calls to Blob Storage to upload files, you also have to configure the Storage account for CORS.</span></span>

1. <span data-ttu-id="89b01-201">Tüm kaynakların Depolama hesabına dosya yüklemesine izin vermek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="89b01-201">Run the following command to allow all origins to upload files to the Storage account.</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="89b01-202">Web uygulamasını görüntü yükleyecek şekilde değiştirme</span><span class="sxs-lookup"><span data-stu-id="89b01-202">Modify the web app to upload images</span></span>

<span data-ttu-id="89b01-203">Web uygulaması, ayarları **settings.js** adlı bir dosyadan alır.</span><span class="sxs-lookup"><span data-stu-id="89b01-203">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="89b01-204">Aşağıdaki adımlarda dosyayı Cloud Shell kullanarak oluşturacak, ardından İşlev uygulamasının URL'sine `window.apiBaseUrl`, Azure Blob Depolama uç noktasının URL'sine de `window.blobBaseUrl` ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="89b01-204">In the following steps, you create the file using Cloud Shell, then set `window.apiBaseUrl` to the URL of the Function app and `window.blobBaseUrl` to the URL of the Azure Blob Storage endpoint.</span></span>

1. <span data-ttu-id="89b01-205">Cloud Shell'de geçerli dizinin **www/dist** klasörü olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="89b01-205">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="89b01-206">İşlev uygulamasının URL'sini sorgulayın ve **FUNCTION_APP_URL** adlı bir Bash değişkeninde depolayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-206">Query the function app's URL and store it in a bash variable named **FUNCTION_APP_URL**.</span></span>

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    <span data-ttu-id="89b01-207">Değişkenin doğru ayarlandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-207">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. <span data-ttu-id="89b01-208">İşlev uygulamanıza yapılan API çağrılarının temel URI değerini ayarlamak için **settings.js** dosyasını oluşturun ve işlev uygulaması URL'sine aşağıdaki gibi ekleme yapın.</span><span class="sxs-lookup"><span data-stu-id="89b01-208">To set the base URI of API calls to your function app, create **settings.js** and add the function app URL like the following.</span></span>

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    <span data-ttu-id="89b01-209">Bu değişikliği aşağıdaki komutu çalıştırarak veya VIM gibi bir komut satırı düzenleyicisi kullanarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="89b01-209">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    <span data-ttu-id="89b01-210">Dosyaya başarıyla yazıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-210">Confirm the file was successfully written.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="89b01-211">Blob Depolama temel URL'sini sorgulayın ve **BLOB_BASE_URL** adlı bir Bash değişkenine kaydedin.</span><span class="sxs-lookup"><span data-stu-id="89b01-211">Query the Blob Storage base URL and store it in a bash variable named **BLOB_BASE_URL**.</span></span>

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    <span data-ttu-id="89b01-212">Değişkenin doğru ayarlandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-212">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. <span data-ttu-id="89b01-213">İşlev uygulamanıza yapılan API çağrılarının temel URI değerini ayarlamak için depolama URL'sini aşağıdaki kod satırında olduğu gibi **settings.js** dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="89b01-213">To set the base URI of API calls to your function app, append the storage URL like the following line of code to **settings.js**.</span></span>

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    <span data-ttu-id="89b01-214">Bu değişikliği aşağıdaki komutu çalıştırarak veya VIM gibi bir komut satırı düzenleyicisi kullanarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="89b01-214">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    <span data-ttu-id="89b01-215">Dosyanın başarıyla yazıldığını ve 2 satır içerdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-215">Confirm the file was successfully written and it now contains 2 lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="89b01-216">Dosyayı Blob depolamaya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="89b01-216">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a><span data-ttu-id="89b01-217">Web uygulamasını test etme</span><span class="sxs-lookup"><span data-stu-id="89b01-217">Test the web application</span></span>

<span data-ttu-id="89b01-218">Bu noktada galeri uygulaması Blob depolamaya görüntü yükleyebilir, ancak henüz görüntüleri gösteremez.</span><span class="sxs-lookup"><span data-stu-id="89b01-218">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="89b01-219">Henüz mevcut olmayan ve daha sonraki modüllerde oluşturacağınız bir `GetImages` işlevini çağırmaya çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="89b01-219">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="89b01-220">Bu çağrı başarısız olacak ve web sayfası "Analyzing..." (Analiz ediliyor) adımında takılmış gibi görünecektir ancak seçtiğiniz görüntü başarıyla yüklenmiş olacaktır.</span><span class="sxs-lookup"><span data-stu-id="89b01-220">That call will fail, and the web page will appear to be stuck on "Analyzing...", but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="89b01-221">Azure portaldan **images** kapsayıcısının içeriğini kontrol ederek görüntünün başarıyla yüklendiğini doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="89b01-221">You can verify an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="89b01-222">Bir tarayıcı penceresinden uygulamaya göz atın.</span><span class="sxs-lookup"><span data-stu-id="89b01-222">In a browser window, browse to the application.</span></span> <span data-ttu-id="89b01-223">Bir görüntü dosyası seçin ve yükleyin.</span><span class="sxs-lookup"><span data-stu-id="89b01-223">Select an image file and upload it.</span></span> <span data-ttu-id="89b01-224">Yükleme tamamlanır, ancak henüz görüntüleri gösterme özelliğini eklemediğimiz için yüklenen fotoğraf görüntülenmez.</span><span class="sxs-lookup"><span data-stu-id="89b01-224">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="89b01-225">(Web sayfası "Analyzing image..." (Görüntü analiz ediliyor) adımında takılmış gibi görünür. Bunu daha sonra düzelteceksiniz.)</span><span class="sxs-lookup"><span data-stu-id="89b01-225">(The web page appears to be stuck on "Analyzing image..."; you'll fix that later.)</span></span>

1. <span data-ttu-id="89b01-226">Cloud Shell'de görüntünün **images** kapsayıcısına yüklendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="89b01-226">In the Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="89b01-227">Bir sonraki öğreticiye geçmeden önce **images** kapsayıcısındaki tüm dosyaları silin.</span><span class="sxs-lookup"><span data-stu-id="89b01-227">Before moving on to the next tutorial, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="89b01-228">Özet</span><span class="sxs-lookup"><span data-stu-id="89b01-228">Summary</span></span>

<span data-ttu-id="89b01-229">Bu modülde bir Azure İşlevi uygulaması oluşturdunuz ve bir web uygulamasının Blob depolamaya görüntü yüklemesini sağlayan sunucusuz işlevi kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="89b01-229">In this module, you created an Azure Function app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="89b01-230">Bir sonraki modülde, Blob tarafından tetiklenen bir sunucusuz işlevi kullanarak yüklenen görüntüler için küçük resim oluşturmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="89b01-230">Next, you learn how to create thumbnails for the uploaded images using a Blob triggered serverless function.</span></span>
