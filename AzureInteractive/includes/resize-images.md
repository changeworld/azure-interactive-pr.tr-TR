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
ms.openlocfilehash: 88b0ac838dfa8e097a30cc6cef591377e4a95ad8
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079601"
---
<span data-ttu-id="6e938-103">Bir önceki modülde sunucusuz bir işlevin web uygulamasından Blob depolamaya güvenli görüntü yükleme amacıyla nasıl kullanılabileceğini gördünüz.</span><span class="sxs-lookup"><span data-stu-id="6e938-103">In the previous module, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="6e938-104">Bu modülde başka bir sunucusuz işlev oluşturarak yüklenen görüntüleri izleyecek ve küçük resimlerini oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="6e938-104">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="6e938-105">Küçük resimler için bir blob depolama kapsayıcısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="6e938-105">Create a blob storage container for thumbnails</span></span>

<span data-ttu-id="6e938-106">Tam boyutlu görüntüler, **images** adlı bir kapsayıcıda depolanır.</span><span class="sxs-lookup"><span data-stu-id="6e938-106">The full size images are stored in a container named **images**.</span></span> <span data-ttu-id="6e938-107">Bu görüntülerin küçük resimlerini depolamak için ayrı bir kapsayıcıya ihtiyacınız vardır.</span><span class="sxs-lookup"><span data-stu-id="6e938-107">You need another container to store thumbnails of those images.</span></span>

1. <span data-ttu-id="6e938-108">Cloud Shell (bash) oturumunuzun açık olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6e938-108">Ensure you are still signed in to the Cloud Shell (bash).</span></span>  <span data-ttu-id="6e938-109">Aksi takdirde **Enter focus mode** (Odak moduna gir) öğesini seçerek bir Cloud Shell penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="6e938-109">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="6e938-110">Depolama hesabınızda tüm bloblara genel erişime sahip **thumbnails** adlı yeni bir kapsayıcı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6e938-110">Create a new container named **thumbnails** in your storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="6e938-111">Blob ile tetiklenen bir sunucusuz işlev oluşturma</span><span class="sxs-lookup"><span data-stu-id="6e938-111">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="6e938-112">Tetikleyici, bir işlevin nasıl çağrılacağını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6e938-112">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="6e938-113">Şimdi oluşturacağınız işlev bir blob tetikleyici kullanır. Bu işlev, **images** kapsayıcısına bir blob (görüntü dosyası) yüklendiğinde otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e938-113">The function you create next uses a blob trigger: the function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="6e938-114">Bir işlevin bir tetikleyiciye sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e938-114">A function must have one trigger.</span></span> <span data-ttu-id="6e938-115">Tetikleyiciler genellikle işlevi tetikleyen yük olan ilgili verilere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6e938-115">Triggers have associated data, which is usually the payload that triggered the function.</span></span>

<span data-ttu-id="6e938-116">Bağlamalar, bir işlevin Azure'da veya üçüncü taraf hizmetlerde veri okuma veya yazma şeklini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6e938-116">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="6e938-117">Bu işlev, onu tetikleyen görüntünün küçük resim sürümünü oluşturur ve küçük resmi *thumbnails* kapsayıcısına kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6e938-117">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="6e938-118">İşlev uygulamanızı Azure portalda açın.</span><span class="sxs-lookup"><span data-stu-id="6e938-118">Open your function app in the Azure Portal.</span></span>

1. <span data-ttu-id="6e938-119">İşlev uygulaması penceresinin sol tarafındaki gezinti bölmesinde **İşlevler**'in üzerine gelin ve **+** simgesine tıklayarak yeni bir sunucusuz işlev oluşturmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-119">In the function app window's left hand navigation, hover over **Functions** and click **+** to start creating a new serverless function.</span></span> <span data-ttu-id="6e938-120">Hızlı başlangıç sayfası açılırsa işlev şablonlarının listesini görmek için **Özel işlev**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-120">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="6e938-121">**BlobTrigger** şablonunu bulun ve seçin.</span><span class="sxs-lookup"><span data-stu-id="6e938-121">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="6e938-122">Bu değerleri kullanarak görüntüler yüklendiğinde küçük resim oluşturan bir işlev oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6e938-122">Use these values to create a function that creates thumbnails as images are uploaded.</span></span>

    | <span data-ttu-id="6e938-123">Ayar</span><span class="sxs-lookup"><span data-stu-id="6e938-123">Setting</span></span>      |  <span data-ttu-id="6e938-124">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="6e938-124">Suggested value</span></span>   | <span data-ttu-id="6e938-125">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6e938-125">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6e938-126">**Dil**</span><span class="sxs-lookup"><span data-stu-id="6e938-126">**Language**</span></span> | <span data-ttu-id="6e938-127">C# veya JavaScript</span><span class="sxs-lookup"><span data-stu-id="6e938-127">C# or JavaScript</span></span> | <span data-ttu-id="6e938-128">Tercih ettiğiniz dili seçin.</span><span class="sxs-lookup"><span data-stu-id="6e938-128">Choose your preferred language.</span></span> |
    | <span data-ttu-id="6e938-129">**İşlevinizi adlandırın**</span><span class="sxs-lookup"><span data-stu-id="6e938-129">**Name your function**</span></span> | <span data-ttu-id="6e938-130">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="6e938-130">ResizeImage</span></span> | <span data-ttu-id="6e938-131">Uygulamanın işlevi keşfedebilmesi için bu adı aynı şekilde yazın.</span><span class="sxs-lookup"><span data-stu-id="6e938-131">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="6e938-132">**Path**</span><span class="sxs-lookup"><span data-stu-id="6e938-132">**Path**</span></span> | <span data-ttu-id="6e938-133">images/{name}</span><span class="sxs-lookup"><span data-stu-id="6e938-133">images/{name}</span></span> | <span data-ttu-id="6e938-134">**images** kapsayıcısında bir dosya göründüğünde işlevi yürütün.</span><span class="sxs-lookup"><span data-stu-id="6e938-134">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="6e938-135">**Depolama hesabı bilgileri**</span><span class="sxs-lookup"><span data-stu-id="6e938-135">**Storage account information**</span></span> | <span data-ttu-id="6e938-136">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="6e938-136">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="6e938-137">Önceden oluşturduğunuz ortam değişkenini bağlantı dizesiyle birlikte kullanın.</span><span class="sxs-lookup"><span data-stu-id="6e938-137">Use the environment variable previously created with the connection string.</span></span> |

    ![Yeni işlev için ayarları girme](media/functions-first-serverless-web-app/3-new-function.png)

1. <span data-ttu-id="6e938-139">İşlevi oluşturmak için **Oluştur**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-139">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="6e938-140">İşlev oluşturulduktan sonra **Tümleştir**'e tıklayarak tetikleyicisini, giriş ve çıkış bağlamalarını görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e938-140">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="6e938-141">Yeni bir çıkış bağlaması oluşturmak için **Yeni çıkış**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-141">Click **New output** to create a new output binding.</span></span>

    ![Tümleştir sekmesinde Yeni Çıkış'ı seçin](media/functions-first-serverless-web-app/3-new-output.jpg)

1. <span data-ttu-id="6e938-143">**Azure Blob Depolama**’yı seçip **Seç**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-143">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="6e938-144">**Seç** düğmesini görmek için aşağı kaydırmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="6e938-144">You may have to scroll down to see the **Select** button.</span></span>

    ![Azure Blob depolamayı seçme](media/functions-first-serverless-web-app/3-storage-output.jpg)

1. <span data-ttu-id="6e938-146">Aşağıdaki değerleri girin.</span><span class="sxs-lookup"><span data-stu-id="6e938-146">Enter the following values.</span></span>

    | <span data-ttu-id="6e938-147">Ayar</span><span class="sxs-lookup"><span data-stu-id="6e938-147">Setting</span></span>      |  <span data-ttu-id="6e938-148">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="6e938-148">Suggested value</span></span>   | <span data-ttu-id="6e938-149">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6e938-149">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6e938-150">**Blob parametresi adı**</span><span class="sxs-lookup"><span data-stu-id="6e938-150">**Blob parameter name**</span></span> | <span data-ttu-id="6e938-151">thumbnail</span><span class="sxs-lookup"><span data-stu-id="6e938-151">thumbnail</span></span> | <span data-ttu-id="6e938-152">İşlev, küçük resmi yazmak için parametreyi bu adla birlikte kullanır.</span><span class="sxs-lookup"><span data-stu-id="6e938-152">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="6e938-153">**İşlev dönüş değeri kullan**</span><span class="sxs-lookup"><span data-stu-id="6e938-153">**Use function return value**</span></span> | <span data-ttu-id="6e938-154">Hayır</span><span class="sxs-lookup"><span data-stu-id="6e938-154">No</span></span> |  |
    | <span data-ttu-id="6e938-155">**Path**</span><span class="sxs-lookup"><span data-stu-id="6e938-155">**Path**</span></span> | <span data-ttu-id="6e938-156">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="6e938-156">thumbnails/{name}</span></span> | <span data-ttu-id="6e938-157">Küçük resimler **thumbnails** adlı bir kapsayıcıya yazılır.</span><span class="sxs-lookup"><span data-stu-id="6e938-157">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="6e938-158">**Depolama hesabı bağlantısı**</span><span class="sxs-lookup"><span data-stu-id="6e938-158">**Storage account connection**</span></span> | <span data-ttu-id="6e938-159">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="6e938-159">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="6e938-160">Önceden oluşturduğunuz ortam değişkenini bağlantı dizesiyle birlikte kullanın.</span><span class="sxs-lookup"><span data-stu-id="6e938-160">Use the environment variable previously created with the connection string.</span></span> |

    ![Blob çıkış bağlaması için ayarları girme](media/functions-first-serverless-web-app/3-blob-output.png)

1. <span data-ttu-id="6e938-162">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6e938-162">**JavaScript**</span></span>

    1. <span data-ttu-id="6e938-163">(JavaScript) Pencerenin sağ üst köşesindeki **Gelişmiş düzenleyici**'ye tıklayarak bağlamaları temsil eden JSON kodunu gösterin.</span><span class="sxs-lookup"><span data-stu-id="6e938-163">(JavaScript) Click on **Advanced editor** in the top right corner of the window to reveal the JSON representing the bindings.</span></span>

    1. <span data-ttu-id="6e938-164">(JavaScript) `blobTrigger` bağlamasında `dataType` adına ve `binary` değerine sahip bir özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6e938-164">(JavaScript) In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="6e938-165">Bu değer bağlamayı, blob içeriğini işleve ikili veri olarak iletecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6e938-165">This configures the binding to pass the blob contents to the function as binary data.</span></span>

    ```json
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "images/{name}",
      "connection": "AZURE_STORAGE_CONNECTION_STRING",
      "dataType": "binary"
    }
    ```

1. <span data-ttu-id="6e938-166">Yeni bağlamayı oluşturmak için **Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-166">Click **Save** to create the new binding.</span></span>

1. <span data-ttu-id="6e938-167">**C#**</span><span class="sxs-lookup"><span data-stu-id="6e938-167">**C#**</span></span>

    1. <span data-ttu-id="6e938-168">(C#) Sol gezinti bölmesinden **ResizeImage** işlevinin adını seçerek işlevin kaynak kodunu açın.</span><span class="sxs-lookup"><span data-stu-id="6e938-168">(C#) Select the **ResizeImage** function name on the left navigation to open the function's source code.</span></span>

    1. <span data-ttu-id="6e938-169">(C#) İşlevin küçük resimleri oluşturabilmesi için **ImageResizer** adlı bir NuGet paketi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e938-169">(C#) The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="6e938-170">NuGet paketleri, C# işlevlerine **project.json** dosyası kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="6e938-170">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="6e938-171">Dosyayı oluşturmak için sağ taraftaki **Dosyaları Görüntüle**'ye tıklayarak işlevi oluşturan dosyaları gösterin.</span><span class="sxs-lookup"><span data-stu-id="6e938-171">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>
    
    1. <span data-ttu-id="6e938-172">(C#) **Ekle**'ye tıklayarak **project.json** adlı yeni bir dosya oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6e938-172">(C#) Click **Add** to add a new file named **project.json**.</span></span>
    
    1. <span data-ttu-id="6e938-173">(C#) [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) dosyasının içeriğini yeni oluşturulan dosyaya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-173">(C#) Copy the contents of [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) into the newly created file.</span></span> <span data-ttu-id="6e938-174">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6e938-174">Save the file.</span></span> <span data-ttu-id="6e938-175">Dosya güncelleştirildiğinde paketler otomatik olarak geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="6e938-175">Packages are automatically restored when the file is updated.</span></span>
    
        ![ImageResizer eklenmiş project.json dosyası](media/functions-first-serverless-web-app/3-project-json.png)
    
    1. <span data-ttu-id="6e938-177">(C#) **Dosyaları Görüntüle** bölümünde **run.csx** dosyasını seçin ve içeriğini [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx) dosyasının içeriğiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6e938-177">(C#) Select **run.csx** under **View Files** and replace its content with the content in [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span></span>

1. <span data-ttu-id="6e938-178">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6e938-178">**JavaScript**</span></span> 

    1. <span data-ttu-id="6e938-179">(JavaScript) Bu işlevin, fotoğrafı yeniden boyutlandırması için `jimp` npm paketi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e938-179">(JavaScript) This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="6e938-180">npm paketini yüklemek için sol gezinti bölmesinde İşlev Uygulamasının adına ve ardından **Platform özellikleri**'ne tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-180">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="6e938-181">(JavaScript) Konsol penceresini açmak için **Konsol**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-181">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="6e938-182">(JavaScript) Konsolda `npm install jimp` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6e938-182">(JavaScript) Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="6e938-183">İşlemin tamamlanması bir veya iki dakika kadar sürebilir.</span><span class="sxs-lookup"><span data-stu-id="6e938-183">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="6e938-184">(JavaScript) Sol gezinti bölmesinde **ResizeImage** işlev adına tıklayarak işlevi açın, **index.js** dosyasının içeriğinin tamamını [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js) dosyasının içeriğiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6e938-184">(JavaScript) Click on the **ResizeImage** function name in the left navigation to reveal the function, replace all of **index.js** with the content of [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span></span>

1. <span data-ttu-id="6e938-185">Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-185">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="6e938-186">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-186">Click **Save**.</span></span> <span data-ttu-id="6e938-187">İşlevin başarıyla kaydedildiğinden ve hata olmadığından emin olmak için günlükler panelini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="6e938-187">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="6e938-188">Sunucusuz işlevi test etme</span><span class="sxs-lookup"><span data-stu-id="6e938-188">Test the serverless function</span></span>

1. <span data-ttu-id="6e938-189">Uygulamayı bir tarayıcıda açın.</span><span class="sxs-lookup"><span data-stu-id="6e938-189">Open the application in a browser.</span></span> <span data-ttu-id="6e938-190">Bir görüntü dosyası seçin ve yükleyin.</span><span class="sxs-lookup"><span data-stu-id="6e938-190">Select an image file and upload it.</span></span> <span data-ttu-id="6e938-191">Yükleme tamamlanır, ancak henüz görüntüleri gösterme özelliğini eklemediğimiz için yüklenen fotoğraf görüntülenmez.</span><span class="sxs-lookup"><span data-stu-id="6e938-191">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="6e938-192">Cloud Shell'de görüntünün **images** kapsayıcısına yüklendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-192">In the Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="6e938-193">**thumbnails** adlı kapsayıcıda küçük resmin oluşturulduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-193">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. <span data-ttu-id="6e938-194">Küçük resmin URL'sini alın.</span><span class="sxs-lookup"><span data-stu-id="6e938-194">Obtain the thumbnail's URL.</span></span>

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    <span data-ttu-id="6e938-195">URL'yi tarayıcıda açın ve küçük resmin doğru şekilde oluşturulduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6e938-195">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="6e938-196">Bir sonraki öğreticiye geçmeden önce **images** ve **thumbnails** kapsayıcılarındaki tüm dosyaları silin.</span><span class="sxs-lookup"><span data-stu-id="6e938-196">Before moving on to the next tutorial, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```
    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="6e938-197">Özet</span><span class="sxs-lookup"><span data-stu-id="6e938-197">Summary</span></span>

<span data-ttu-id="6e938-198">Bu modülde, Blob depolama kapsayıcısına görüntü yüklendiğinde küçük resim oluşturan bir sunucusuz işlev oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="6e938-198">In this module, you created a serverless function to create a thumbnail whenever an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="6e938-199">Bir sonraki modülde Azure Cosmos DB'yi kullanarak görüntü meta verilerini depolamayı ve listelemeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6e938-199">Next, you learn how to use Azure Cosmos DB to store and list image metadata.</span></span>
