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
ms.openlocfilehash: 194a25dbf9abda80379aa5aab408ac4ffe9ab7f5
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460067"
---
Azure Cosmos DB, Microsoft’un sunucusuz, genel olarak dağıtılan çok modelli veritabanıdır. Bu modülde Cosmos DB'de görüntü meta verilerini JSON belgesi olarak depolamayı ve almayı öğreneceksiniz.

## <a name="create-a-cosmos-db-account-database-and-collection"></a>Cosmos DB hesabı, veritabanı ve koleksiyonu oluşturma

Cosmos DB hesabı, Cosmos DB veritabanlarını içeren bir Azure kaynağıdır.

1. Cloud Shell oturumunuzun açık olduğundan emin olun.  Aksi takdirde **Enter focus mode** (Odak moduna gir) öğesini seçerek bir Cloud Shell penceresi açın. 

1. Bu öğreticideki diğer kaynaklarla aynı kaynak grubunda benzersiz ada sahip bir Cosmos DB hesabı oluşturun.

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. Cosmos DB hesabı oluşturulduktan sonra hesapta **imagesdb** adlı yeni bir veritabanı oluşturun.

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. Veritabanı oluşturulduktan sonra veritabanında 400 istek birimi (RU) performansa sahip **images** adlı yeni bir koleksiyon oluşturun.

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-cosmos-db-when-a-thumbnail-is-created"></a>Küçük resim oluşturulduğunda Cosmos DB'ye belge kaydetme

Cosmos DB çıkışı bağlaması, Azure İşlevlerini kullanarak Cosmos DB koleksiyonunda belge oluşturmanızı sağlar. Aşağıdaki adımlarda **ResizeImage** işlevindeki Cosmos DB çıkışını yapılandıracak ve işlevi kaydedilecek bir belge (nesne) oluşturacak şekilde değiştireceksiniz.

1. İşlev uygulamasını Azure portalda açın.

1. Sol gezinti bölmesinde **ResizeImage** işlevini genişletin ve **Tümleştir**'i seçin.

1. **Çıkışlar** bölümünde **Yeni Çıkış**'a tıklayın.

1. **Azure Cosmos DB** öğesini bulun ve seçin. Ardından **Seç**'e tıklayın.

    ![Yeni Çıkış'ı seçin](media/functions-first-serverless-web-app/4-new-output.jpg)

1. **Azure Cosmos DB çıkışı** bölümündeki alanları aşağıdaki değerlerle doldurun.

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | ---|
    | **Belge parametre adı** | **İşlev dönüş değeri kullan**'ı seçin | Metin kutusunun değeri otomatik olarak **$return** olur. |
    | **Veritabanı adı** | imagesdb | Oluşturduğunuz veritabanının adını kullanın. |
    | **Koleksiyon adı** | images | Oluşturduğunuz koleksiyonun adını kullanın. |

1. **Azure Cosmos DB hesabı bağlantısı**'nın yanında **Yeni**'ye tıklayın. Daha önce oluşturduğunuz Cosmos DB hesabını seçin.

    ![Azure Cosmos DB çıkış bağlaması için ayarları girin](media/functions-first-serverless-web-app/4-cosmos-db-output.png)

1. Cosmos DB çıkış bağlamasını oluşturmak için **Kaydet**'e tıklayın.

1. Sol taraftaki **ResizeImage** işlevinin adına tıklayarak işlevi açın.

1. **C#**

    1. (C#) İşlevin dönüş türünü **void** yerine **object** olarak değiştirin.

    1. (C#) İşlevin sonuna aşağıdaki kod bloğunu ekleyerek kaydedilecek belgenin döndürülmesini sağlayın:
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![Değişikliklerden sonra run.csx dosyasının ResizeImages işlevi bölümü](media/functions-first-serverless-web-app/4-update-function.png)

1. **JavaScript**

    1. (JavaScript) Cosmos DB'ye kaydedilecek belgeyi döndürmek için `else` yan tümcesindeki `context.done()` deyimini değiştirin.

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

1. Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.

1. **Kaydet**’e tıklayın. İşlevin başarıyla kaydedildiğinden ve hata olmadığından emin olmak için günlükler panelini kontrol edin.


## <a name="create-a-function-to-list-images-from-cosmos-db"></a>Cosmos DB'deki görüntüleri listelemek için işlev oluşturma

Web uygulamasının Cosmos DB'den görüntü meta verilerini alması için bir API gerekir. Aşağıdaki adımlarda: Veritabanı koleksiyonunu sorgulamak için Cosmos DB giriş bağlaması kullanan bir HTTP ile tetiklenen işlev oluşturacaksınız.

1. İşlev uygulamanızda sol taraftaki **İşlevler**'in üzerine gelin ve **+** simgesine tıklayarak yeni bir işlev oluşturun.

1. **HttpTrigger** şablonunu bulun ve seçin.

1. Görüntü alma URL'si üreten bir işlev oluşturmak için bu değerleri kullanın.

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | ---|
    | **İşlevinizi adlandırın** | GetImages | Uygulamanın işlevi keşfedebilmesi için bu adı aynı şekilde yazın. |
    | **Yetkilendirme düzeyi** | Anonim | İşleve herkesin erişmesine izin verin. |

1. **Oluştur**’a tıklayın.

1. Yeni işlev oluşturulduktan sonra sol gezinti bölmesinde işlev adının altında **Tümleştir**'e tıklayın.

1. **Yeni Giriş**'e tıklayın ve **Azure Cosmos DB**'yi seçin. 

    ![Yeni Giriş'i seçin](media/functions-first-serverless-web-app/4-new-input.jpg)

1. **Seç**'e tıklayın.

1. Aşağıdaki değerleri girin:

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | ---|
    | **Belge parametre adı** | documents | İşlevdeki parametre adı ile eşleşir. |
    | **Veritabanı adı** | imagesdb |  |
    | **Koleksiyon adı** | images |  |
    | **SQL sorgusu** | select * from c order by c._ts desc | Belgeleri al, önce son belgeler. |
    | **Azure Cosmos DB hesabı bağlantısı** | Var olan bağlantı dizesini seçme |  |

1. Giriş bağlamasını oluşturmak için **Kaydet**’e tıklayın.

1. **C#**

    1. İşlevin adına tıklayarak kod penceresini açın ve **run.csx** dosyasının içeriğinin tamamını [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx) dosyasının içeriğiyle değiştirin.

1. **JavaScript**

    1. İşlevin adına tıklayarak kod penceresini açın ve **index.js** dosyasının içeriğinin tamamını [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js) dosyasının içeriğiyle değiştirin.

1. Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.

1. **Kaydet**’e tıklayın. İşlevin başarıyla kaydedildiğinden ve hata olmadığından emin olmak için günlükler panelini kontrol edin.


## <a name="test-the-application"></a>Uygulamayı test etme

1. Uygulamayı bir tarayıcıda açın. Bir görüntü dosyası seçin ve yükleyin.

1. Birkaç saniye sonra sayfada yeni görüntünün küçük resmi görünür.

1. Azure portaldaki Arama kutusunu kullanarak Cosmos DB hesabınızı adıyla arayın. Tıklayarak açın.

1. Koleksiyonlara ve belgelere göz atmak için sol taraftaki **Veri Gezgini**'ne tıklayın.

1. **imagesdb** veritabanının altındaki **images** koleksiyonunu seçin.

1. Yüklenen görüntü için bir belge oluşturulduğunu doğrulayın.

    ![Yüklenen görüntü için oluşturulan belgeyi gösteren Veri Gezgini](media/functions-first-serverless-web-app/4-data-explorer.png)



## <a name="summary"></a>Özet

Bu modülde Cosmos DB hesabı, veritabanı ve koleksiyonu oluşturmayı öğrendiniz. Ayrıca Cosmos DB bağlamalarını kullanarak görüntü meta verilerini Cosmos DB koleksiyonuna kaydetmeyi ve oradan almayı öğrendiniz. Bir sonraki modülde Microsoft Bilişsel Hizmetler'i kullanarak yüklenen görüntüler için otomatik resim yazısı oluşturmayı öğreneceksiniz.
