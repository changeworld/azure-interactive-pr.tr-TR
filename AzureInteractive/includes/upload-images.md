---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 10/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3779c2e130afa7ee8d5879f30a924e258b7a41e9
ms.sourcegitcommit: fdb43556b8dcf67cb39c18e532b5fab7ac53eaee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49315985"
---
Oluşturmakta olduğunuz uygulama bir fotoğraf galerisidir. Görüntüleri yüklemek ve görüntülemek üzere API'leri çağırmak için istemci tarafında JavaScript kullanır. Bu modülde, karşıya görüntü yüklemek için sınırlı süreli URL oluşturan bir sunucusuz işlevi kullanarak bir API oluşturacaksınız. Web uygulaması, oluşturulan URL ile [Blob depolama REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) kullanarak Blob depolamaya bir görüntü yükler.

## <a name="create-a-blob-storage-container-for-images"></a>Görüntüler için bir blob depolama kapsayıcısı oluşturma

Uygulamanın görüntüleri yüklemesi ve barındırması için ayrı depolama kapsayıcıları gerekir.

1. Cloud Shell (bash) oturumunuzun açık olduğundan emin olun. Aksi takdirde **Enter focus mode** (Odak moduna gir) öğesini seçerek bir Cloud Shell penceresi açın.

1.  Depolama hesabınızda tüm bloblara genel erişime sahip **images** adlı yeni bir kapsayıcı oluşturun.

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

Azure İşlevleri, sunucusuz işlevleri çalıştıran bir hizmettir. Sunucusuz işlev, HTTP isteği gibi olaylar veya depolama kapsayıcısında bir blobun oluşturulması gibi durumlar üzerine tetiklenebilir (çağrılabilir).

Azure İşlev uygulaması, bir veya daha fazla sunucusuz işlev için kapsayıcı görevi görür.

Önceden oluşturduğunuz **first-serverless-app** adlı kaynak grubunda benzersiz ada sahip yeni bir Azure İşlevi uygulaması oluşturun. İşlev uygulamaları için bir Depolama hesabı gerekir. Bu öğreticide var olan depolama hesabını kullanacaksınız.

```azurecli
az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
```

## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

Bu öğreticideki işlev uygulaması için İşlevler çalışma zamanının 1.x sürümü gereklidir. `FUNCTIONS_EXTENSION_VERSION` uygulama ayarını `~1` olarak ayarladığınızda, işlev uygulaması en son 1.x sürümüne sabitlenir. [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set) komutuyla uygulama ayarlarını belirleyin.

Aşağıdaki Azure CLI komutunda <app_name>, işlev uygulamanızın adıdır.

```azurecli
az functionapp config appsettings set --name <function app name> --g first-serverless-app --settings FUNCTIONS_EXTENSION_VERSION=~1
```

## <a name="create-an-http-triggered-serverless-function"></a>HTTP ile tetiklenen bir sunucusuz işlev oluşturma

Fotoğraf galerisi web uygulaması, Blob depolamaya görüntüyü güvenli bir şekilde yüklemek için sınırlı süreli URL oluşturma amacıyla sunucusuz işleve bir HTTP isteği gönderir. İşlev, bir HTTP isteği ile tetiklenir ve Azure Depolama SDK'sını kullanarak güvenli URL'yi oluşturup döndürür.

1. İşlev uygulaması oluşturulduktan sonra Azure portaldaki Arama kutusunu kullanarak bulun ve tıklayarak açın.

    ![İşlev uygulamasını açma](media/functions-first-serverless-web-app/2-search-function-app.png)

1. İşlev uygulaması penceresinin sol tarafındaki gezinti bölmesinde **İşlevler**'in üzerine gelin ve **+** simgesine tıklayarak yeni bir sunucusuz işlev oluşturmaya başlayın.

    ![Yeni bir işlev oluşturma](media/functions-first-serverless-web-app/2-new-function.png)

1. İşlev şablonlarının listesini görmek için **Özel işlev**'e tıklayın.

1. **HttpTrigger** şablonunu bulun ve kullanmak istediğiniz dile (C# veya JavaScript) tıklayın.

1. Blob yükleme URL'si üreten bir işlev oluşturmak için bu değerleri kullanın.

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | ---|
    | **Dil** | C# veya JavaScript | Kullanmak istediğiniz dili seçin. |
    | **İşlevinizi adlandırın** | GetUploadUrl | Uygulamanın işlevi keşfedebilmesi için bu adı aynı şekilde yazın. |
    | **Yetkilendirme düzeyi** | Anonim | İşleve herkesin erişmesine izin verin. |

    ![Yeni bir HTTP ile tetiklenen işlev için ayarları girin](media/functions-first-serverless-web-app/2-new-function-httptrigger.png)

1. İşlevi oluşturmak için **Oluştur**'a tıklayın.

1. **C#** 

    1. İşlevin kaynak kodu göründüğünde **run.csx** dosyasının içeriğinin tamamını [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) dosyasının içeriğiyle değiştirin.

1. **JavaScript** 

    1. (JavaScript) Bu işlev, güvenli URL'yi oluşturmak için gerekli olan paylaşılan erişim imzası (SAS) belirtecini üretmek için `azure-storage` npm paketine ihtiyaç duyar. npm paketini yüklemek için sol gezinti bölmesinde İşlev Uygulamasının adına ve ardından **Platform özellikleri**'ne tıklayın.

    1. (JavaScript) Konsol penceresini açmak için **Konsol**'a tıklayın.

        ![Konsol penceresi açma](media/functions-first-serverless-web-app/2-open-console.jpg)

    1. (JavaScript) `cd d:\home\site\wwwroot` komutunu çalıştırarak geçerli dizinin **d:\home\site\wwwroot** olduğundan emin olun.

    1. (JavaScript) `npm init -y` komutunu çalıştırarak boş bir **package.json** dosyası oluşturun.

    1. (JavaScript) Konsolda `npm install --save azure-storage` komutunu çalıştırarak paketi yükleyin ve **package.json** içine kaydedin. İşlemin tamamlanması bir veya iki dakika kadar sürebilir.

    1. (JavaScript) Sol gezinti bölmesinde işlev adına (**GetUploadUrl**) tıklayarak işlevi açın, **index.js** dosyasının içeriğinin tamamını [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) dosyasının içeriğiyle değiştirin.
    
        ![Güncelleştirmeden sonra index.js](media/functions-first-serverless-web-app/2-paste-js.jpg)

1. Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.

1. **Kaydet**’e tıklayın. İşlevin başarıyla derlendiğinden emin olmak için günlükler panelini kontrol edin.

İşlev, Blob depolamaya dosya yüklemek için kullanılan paylaşılan erişim imzası (SAS) URL'sini oluşturur. SAS URL'si yalnızca kısa bir süre boyunca geçerlidir ve yalnızca tek bir dosyanın yüklenmesine izin verir. [Paylaşılan erişim imzalarını kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) hakkında daha fazla bilgi için Blob depolama belgelerine bakın.


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a>Depolama bağlantısı dizesi için ortam değişkeni ekleme

Oluşturduğunuz işlevin SAS URL'sini oluşturabilmesi için Depolama hesabına ait bir bağlantı dizesi gerekir. Bağlantı dizesini işlevin gövdesine sabit kodlamak yerine uygulama ayarı olarak kaydedebilirsiniz. Ortam değişkenlerinde olduğu gibi uygulama ayarlarına da İşlev uygulamasındaki tüm işlevler tarafından erişim sağlanabilir.

1. Cloud Shell'de Depolama hesabı bağlantı dizesini sorgulayın ve **STORAGE_CONNECTION_STRING** adlı bir Bash değişkenine kaydedin.

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    Değişkenin başarıyla ayarlandığını doğrulayın.

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. Bir önceki adımda kaydedilen değeri kullanarak **AZURE_STORAGE_CONNECTION_STRING** adlı yeni bir uygulama ayarı oluşturun.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    Komut çıktısında yeni uygulama ayarının doğru değerle birlikte göründüğünü onaylayın.


## <a name="test-the-serverless-function"></a>Sunucusuz işlevi test etme

Azure portal, işlevleri oluşturma ve düzenlemeye ek olarak test etmek için de yerleşik bir araç sunar.

1. HTTP sunucusuz işlevini test etmek için kod penceresinin sağ tarafındaki **Test** sekmesine tıklayarak test panelini açın.

1. **Http yöntemi** değerini **GET** olarak değiştirin.

1. **Sorgu** bölümünde *Parametre ekle*'ye tıklayın ve aşağıdaki parametreyi ekleyin:

    | ad      |  değer   | 
    | --- | --- |
    | filename | image1.jpg |

1. Test panelinde **Çalıştır**'a tıklayarak işleve bir HTTP isteği gönderin.

1. İşlev bir yükleme URL'si döndürür. İşlevin yürütüldüğü günlükler panelinde görüntülenir.

    ![İşlevin başarıyla çalıştığını gösteren günlükler](media/functions-first-serverless-web-app/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a>İşlev uygulamasında CORS'yi yapılandırma

Uygulamanın ön ucu Blob depolamada barındırıldığından Azure İşlevi uygulamasından farklı bir etki alanı adına sahiptir. İstemci tarafı JavaScript kodunun oluşturduğunuz işlevi başarıyla çağırmasını sağlamak için işlev uygulamasında çıkış noktaları arası kaynak paylaşımının (CORS) yapılandırılması gerekir.

1. İşlev uygulaması penceresinin sol gezinti çubuğunda işlev uygulamasının adına tıklayın.

1. **Platform özellikleri**'ne tıklayarak gelişmiş özellikleri açın.

1. **API** bölümünde **CORS**'yi tıklayın.

    ![CORS seçimi](media/functions-first-serverless-web-app/2-open-cors.jpg)

1. Uygulama URL'si için bir önceki modülde elde ettiğiniz izin verme kaynağını ekleyin ve sondaki **/** karakterini atın (örneğin: `https://firstserverlessweb.z4.web.core.windows.net`).

    ![Sunucusuz web uygulaması URL'sinin eklendiğini gösteren CORS ayarları](media/functions-first-serverless-web-app/2-add-cors.png)

1. **Kaydet**’e tıklayın.

1. C#

   1. (C#) `GetUploadUrl` işlevine geri dönün ve **Tümleştir** sekmesini seçin.

   1. (C#) Seçili HTTP metotları bölümünde **OPTIONS** seçeneğini belirleyin.

      **GET**, **POST** ve **OPTIONS** seçili olmalıdır. CORS, C# işlevleri için varsayılan olarak seçilmeyen OPTIONS metodunu kullanır.  

   1. (C#) **Kaydet**'e tıklayın.

1. Portalda işlev uygulamasına gidin, **Genel bakış** sekmesini seçin ve **Yeniden başlat**'a tıklayarak CORS değişikliklerinin uygulanmasını sağlayın.

## <a name="configure-cors-in-the-storage-account"></a>Depolama hesabında CORS yapılandırması

Uygulama dosyaları yüklemek için Blob Depolama’ya istemci tarafında JavaScript çağrıları da gönderdiğinden, Depolama hesabında da CORS yapılandırmasını yapmanız gerekir.

1. Tüm kaynakların Depolama hesabına dosya yüklemesine izin vermek için aşağıdaki komutu çalıştırın.

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a>Web uygulamasını görüntü yükleyecek şekilde değiştirme

Web uygulaması, ayarları **settings.js** adlı bir dosyadan alır. Aşağıdaki adımlarda dosyayı Cloud Shell kullanarak oluşturacak, ardından İşlev uygulamasının URL'sine `window.apiBaseUrl`, Azure Blob Depolama uç noktasının URL'sine de `window.blobBaseUrl` ekleyeceksiniz.

1. Cloud Shell'de geçerli dizinin **www/dist** klasörü olduğundan emin olun.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. İşlev uygulamasının URL'sini sorgulayın ve **FUNCTION_APP_URL** adlı bir Bash değişkeninde depolayın.

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    Değişkenin doğru ayarlandığını doğrulayın.

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. İşlev uygulamanıza yapılan API çağrılarının temel URI değerini ayarlamak için **settings.js** dosyasını oluşturun ve işlev uygulaması URL'sine aşağıdaki gibi ekleme yapın.

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    Bu değişikliği aşağıdaki komutu çalıştırarak veya VIM gibi bir komut satırı düzenleyicisi kullanarak yapabilirsiniz.

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    Dosyaya başarıyla yazıldığını doğrulayın.

    ```azurecli
    cat settings.js
    ```

1. Blob Depolama temel URL'sini sorgulayın ve **BLOB_BASE_URL** adlı bir Bash değişkenine kaydedin.

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    Değişkenin doğru ayarlandığını doğrulayın.

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. İşlev uygulamanıza yapılan API çağrılarının temel URI değerini ayarlamak için depolama URL'sini aşağıdaki kod satırında olduğu gibi **settings.js** dosyasına ekleyin.

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    Bu değişikliği aşağıdaki komutu çalıştırarak veya VIM gibi bir komut satırı düzenleyicisi kullanarak yapabilirsiniz.

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    Dosyanın başarıyla yazıldığını ve 2 satır içerdiğini doğrulayın.

    ```azurecli
    cat settings.js
    ```

1. Dosyayı Blob depolamaya yükleyin.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a>Web uygulamasını test etme

Bu noktada galeri uygulaması Blob depolamaya görüntü yükleyebilir, ancak henüz görüntüleri gösteremez. Henüz mevcut olmayan ve daha sonraki modüllerde oluşturacağınız bir `GetImages` işlevini çağırmaya çalışacaktır. Bu çağrı başarısız olacak ve web sayfası "Analyzing..." (Analiz ediliyor) adımında takılmış gibi görünecektir ancak seçtiğiniz görüntü başarıyla yüklenmiş olacaktır.

Azure portaldan **images** kapsayıcısının içeriğini kontrol ederek görüntünün başarıyla yüklendiğini doğrulayabilirsiniz.

1. Bir tarayıcı penceresinden uygulamaya göz atın. Bir görüntü dosyası seçin ve yükleyin. Yükleme tamamlanır, ancak henüz görüntüleri gösterme özelliğini eklemediğimiz için yüklenen fotoğraf görüntülenmez. (Web sayfası "Analyzing image..." (Görüntü analiz ediliyor) adımında takılmış gibi görünür. Bunu daha sonra düzelteceksiniz.)

1. Cloud Shell'de görüntünün **images** kapsayıcısına yüklendiğini doğrulayın.

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. Bir sonraki öğreticiye geçmeden önce **images** kapsayıcısındaki tüm dosyaları silin.

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a>Özet

Bu modülde bir Azure İşlevi uygulaması oluşturdunuz ve bir web uygulamasının Blob depolamaya görüntü yüklemesini sağlayan sunucusuz işlevi kullanmayı öğrendiniz. Bir sonraki modülde, Blob tarafından tetiklenen bir sunucusuz işlevi kullanarak yüklenen görüntüler için küçük resim oluşturmayı öğreneceksiniz.
