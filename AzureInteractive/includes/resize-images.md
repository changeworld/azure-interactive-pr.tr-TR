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
Bir önceki modülde sunucusuz bir işlevin web uygulamasından Blob depolamaya güvenli görüntü yükleme amacıyla nasıl kullanılabileceğini gördünüz. Bu modülde başka bir sunucusuz işlev oluşturarak yüklenen görüntüleri izleyecek ve küçük resimlerini oluşturacaksınız.

## <a name="create-a-blob-storage-container-for-thumbnails"></a>Küçük resimler için bir blob depolama kapsayıcısı oluşturma

Tam boyutlu görüntüler, **images** adlı bir kapsayıcıda depolanır. Bu görüntülerin küçük resimlerini depolamak için ayrı bir kapsayıcıya ihtiyacınız vardır.

1. Cloud Shell (bash) oturumunuzun açık olduğundan emin olun.  Aksi takdirde **Enter focus mode** (Odak moduna gir) öğesini seçerek bir Cloud Shell penceresi açın. 

1. Depolama hesabınızda tüm bloblara genel erişime sahip **thumbnails** adlı yeni bir kapsayıcı oluşturun.

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a>Blob ile tetiklenen bir sunucusuz işlev oluşturma

Tetikleyici, bir işlevin nasıl çağrılacağını tanımlar. Şimdi oluşturacağınız işlev bir blob tetikleyici kullanır. Bu işlev, **images** kapsayıcısına bir blob (görüntü dosyası) yüklendiğinde otomatik olarak çağrılır. Bir işlevin bir tetikleyiciye sahip olması gerekir. Tetikleyiciler genellikle işlevi tetikleyen yük olan ilgili verilere sahiptir.

Bağlamalar, bir işlevin Azure'da veya üçüncü taraf hizmetlerde veri okuma veya yazma şeklini tanımlar. Bu işlev, onu tetikleyen görüntünün küçük resim sürümünü oluşturur ve küçük resmi *thumbnails* kapsayıcısına kaydeder.

1. İşlev uygulamanızı Azure portalda açın.

1. İşlev uygulaması penceresinin sol tarafındaki gezinti bölmesinde **İşlevler**'in üzerine gelin ve **+** simgesine tıklayarak yeni bir sunucusuz işlev oluşturmaya başlayın. Hızlı başlangıç sayfası açılırsa işlev şablonlarının listesini görmek için **Özel işlev**'e tıklayın.

1. **BlobTrigger** şablonunu bulun ve seçin.

1. Bu değerleri kullanarak görüntüler yüklendiğinde küçük resim oluşturan bir işlev oluşturun.

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | ---|
    | **Dil** | C# veya JavaScript | Tercih ettiğiniz dili seçin. |
    | **İşlevinizi adlandırın** | ResizeImage | Uygulamanın işlevi keşfedebilmesi için bu adı aynı şekilde yazın. |
    | **Path** | images/{name} | **images** kapsayıcısında bir dosya göründüğünde işlevi yürütün. |
    | **Depolama hesabı bilgileri** | AZURE_STORAGE_CONNECTION_STRING | Önceden oluşturduğunuz ortam değişkenini bağlantı dizesiyle birlikte kullanın. |

    ![Yeni işlev için ayarları girme](media/functions-first-serverless-web-app/3-new-function.png)

1. İşlevi oluşturmak için **Oluştur**'a tıklayın.

1. İşlev oluşturulduktan sonra **Tümleştir**'e tıklayarak tetikleyicisini, giriş ve çıkış bağlamalarını görüntüleyebilirsiniz.

1. Yeni bir çıkış bağlaması oluşturmak için **Yeni çıkış**'a tıklayın.

    ![Tümleştir sekmesinde Yeni Çıkış'ı seçin](media/functions-first-serverless-web-app/3-new-output.jpg)

1. **Azure Blob Depolama**’yı seçip **Seç**’e tıklayın. **Seç** düğmesini görmek için aşağı kaydırmanız gerekebilir.

    ![Azure Blob depolamayı seçme](media/functions-first-serverless-web-app/3-storage-output.jpg)

1. Aşağıdaki değerleri girin.

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | ---|
    | **Blob parametresi adı** | thumbnail | İşlev, küçük resmi yazmak için parametreyi bu adla birlikte kullanır. |
    | **İşlev dönüş değeri kullan** | Hayır |  |
    | **Path** | thumbnails/{name} | Küçük resimler **thumbnails** adlı bir kapsayıcıya yazılır. |
    | **Depolama hesabı bağlantısı** | AZURE_STORAGE_CONNECTION_STRING | Önceden oluşturduğunuz ortam değişkenini bağlantı dizesiyle birlikte kullanın. |

    ![Blob çıkış bağlaması için ayarları girme](media/functions-first-serverless-web-app/3-blob-output.png)

1. **JavaScript**

    1. (JavaScript) Pencerenin sağ üst köşesindeki **Gelişmiş düzenleyici**'ye tıklayarak bağlamaları temsil eden JSON kodunu gösterin.

    1. (JavaScript) `blobTrigger` bağlamasında `dataType` adına ve `binary` değerine sahip bir özellik ekleyin. Bu değer bağlamayı, blob içeriğini işleve ikili veri olarak iletecek şekilde yapılandırır.

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

1. Yeni bağlamayı oluşturmak için **Kaydet**’e tıklayın.

1. **C#**

    1. (C#) Sol gezinti bölmesinden **ResizeImage** işlevinin adını seçerek işlevin kaynak kodunu açın.

    1. (C#) İşlevin küçük resimleri oluşturabilmesi için **ImageResizer** adlı bir NuGet paketi gerekir. NuGet paketleri, C# işlevlerine **project.json** dosyası kullanılarak eklenir. Dosyayı oluşturmak için sağ taraftaki **Dosyaları Görüntüle**'ye tıklayarak işlevi oluşturan dosyaları gösterin.
    
    1. (C#) **Ekle**'ye tıklayarak **project.json** adlı yeni bir dosya oluşturun.
    
    1. (C#) [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) dosyasının içeriğini yeni oluşturulan dosyaya kopyalayın. Dosyayı kaydedin. Dosya güncelleştirildiğinde paketler otomatik olarak geri yüklenir.
    
        ![ImageResizer eklenmiş project.json dosyası](media/functions-first-serverless-web-app/3-project-json.png)
    
    1. (C#) **Dosyaları Görüntüle** bölümünde **run.csx** dosyasını seçin ve içeriğini [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx) dosyasının içeriğiyle değiştirin.

1. **JavaScript** 

    1. (JavaScript) Bu işlevin, fotoğrafı yeniden boyutlandırması için `jimp` npm paketi gerekir. npm paketini yüklemek için sol gezinti bölmesinde İşlev Uygulamasının adına ve ardından **Platform özellikleri**'ne tıklayın.

    1. (JavaScript) Konsol penceresini açmak için **Konsol**'a tıklayın.

    1. (JavaScript) Konsolda `npm install jimp` komutunu çalıştırın. İşlemin tamamlanması bir veya iki dakika kadar sürebilir.

    1. (JavaScript) Sol gezinti bölmesinde **ResizeImage** işlev adına tıklayarak işlevi açın, **index.js** dosyasının içeriğinin tamamını [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js) dosyasının içeriğiyle değiştirin.

1. Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.

1. **Kaydet**’e tıklayın. İşlevin başarıyla kaydedildiğinden ve hata olmadığından emin olmak için günlükler panelini kontrol edin.


## <a name="test-the-serverless-function"></a>Sunucusuz işlevi test etme

1. Uygulamayı bir tarayıcıda açın. Bir görüntü dosyası seçin ve yükleyin. Yükleme tamamlanır, ancak henüz görüntüleri gösterme özelliğini eklemediğimiz için yüklenen fotoğraf görüntülenmez.

1. Cloud Shell'de görüntünün **images** kapsayıcısına yüklendiğini doğrulayın.

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. **thumbnails** adlı kapsayıcıda küçük resmin oluşturulduğunu doğrulayın.

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. Küçük resmin URL'sini alın.

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    URL'yi tarayıcıda açın ve küçük resmin doğru şekilde oluşturulduğunu doğrulayın.

1. Bir sonraki öğreticiye geçmeden önce **images** ve **thumbnails** kapsayıcılarındaki tüm dosyaları silin.

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```
    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```


## <a name="summary"></a>Özet

Bu modülde, Blob depolama kapsayıcısına görüntü yüklendiğinde küçük resim oluşturan bir sunucusuz işlev oluşturdunuz. Bir sonraki modülde Azure Cosmos DB'yi kullanarak görüntü meta verilerini depolamayı ve listelemeyi öğreneceksiniz.
