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
Bu noktada uygulama, görüntü yüklemenizi ve görüntülemenizi sağlayan etkin bir galeridir. Bu modülde, Microsoft Bilişsel Hizmetler'in Görüntü İşleme API'sini kullanarak yüklenen görüntüler için resim yazısı oluşturmayı ve bu resim yazılarını Cosmos DB'ye görüntü meta verileriyle birlikte kaydetmeyi öğreneceksiniz.

## <a name="create-a-computer-vision-account"></a>Görüntü İşleme hesabı oluşturma

Microsoft Bilişsel Hizmetler, geliştiricilerin uygulamalarını daha akıllı hale getirmesini sağlamak için kullanımına sunulan bir hizmet koleksiyonudur. Görüntü İşleme API'si, görüntüleri gelişmiş algoritmalar kullanarak işleyen ve her görüntüyle ilgili bilgileri döndüren sunucusuz bir hizmettir. Ayda en fazla 5000 API çağrısı yapabileceğiniz ücretsiz katmana sahiptir.

1. Cloud Shell oturumunuzun açık olduğundan emin olun. Aksi takdirde **Enter focus mode** (Odak moduna gir) öğesini seçerek bir Cloud Shell penceresi açın. 

1. Kaynak grubunuzda **ComputerVision** türünde benzersiz ada sahip yeni bir Bilişsel Hizmetler hesabı oluşturun. Ücretsiz katman için SKU olarak **F0** kullanın. Mevcut bir Görüntü İşleme hesabınız varsa Standart hesap (S1) oluşturmanız gerekebilir ancak bunun için ücret alınabileceğini lütfen unutmayın.

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a>Görüntü İşleme URL'si ve anahtarı için İşlev Uygulaması ayarlarını oluşturma

Görüntü İşleme API'sini çağırmak için URL ve anahtar gerekir.

1. Görüntü İşleme API'si anahtarını ve URL'sini alın ve Bash değişkenlerine kaydedin.

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. İşlev uygulamasında **COMP_VISION_KEY** ve **COMP_VISION_URL** adlı uygulama ayarlarını oluşturun.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-computer-vision-api-from-resizeimage-function"></a>ResizeImage işlevinden Görüntü İşleme API'sini çağırma

Aşağıdaki adımlarda **ResizeImage** işlevini yüklenen görüntüleri tanımlamak ve açıklamayı Cosmos DB'ye kaydetmek için Görüntü İşleme API'sini çağıracak şekilde değiştireceksiniz.

1. İşlev uygulamanızı Azure portalda açın.

1. **C#**

    1. (C#) Sol gezinti bölmesinden **ResizeImage** işlevini bulun ve kod penceresini açın.

    1. (C#) Kodu [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) dosyasının içeriğiyle değiştirin. Bu kod, Görüntü İşleme API'sini çağırmak için `HttpClient` kullanır ve sonucu Cosmos DB'ye kaydeder.

1. **JavaScript**

    1. (JavaScript) Bu işlevin Görüntü İşleme API'sine HTTP çağrısı yapması için `axios` npm paketi gerekir. npm paketini yüklemek için sol gezinti bölmesinde İşlev Uygulamasının adına ve ardından **Platform özellikleri**'ne tıklayın.

    1. (JavaScript) Konsol penceresini açmak için **Konsol**'a tıklayın.

    1. (JavaScript) Konsolda `npm install axios` komutunu çalıştırın. İşlemin tamamlanması bir veya iki dakika kadar sürebilir.

    1. (JavaScript) Sol gezinti bölmesinde işlev adına (**ResizeImage**) tıklayarak işlevi açın, ardından **index.js** dosyasının içeriğinin tamamını [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) dosyasının içeriğiyle değiştirin.

1. Günlükler panelini genişletmek için kod penceresinin altındaki **Günlükler**'e tıklayın.

1. **Kaydet**’e tıklayın. İşlevin başarıyla kaydedildiğinden ve hata olmadığından emin olmak için günlükler panelini kontrol edin.


## <a name="test-the-application"></a>Uygulamayı test etme

1. Uygulamayı bir tarayıcıda açın. Bir görüntü dosyası seçin ve yükleyin.

1. Birkaç saniye sonra sayfada yeni görüntünün küçük resmi görünmelidir. Görüntünün üzerine giderek Görüntü İşleme tarafından oluşturulan açıklamayı görebilirsiniz.


## <a name="summary"></a>Özet

Bu modülde Microsoft Bilişsel Hizmetler Görüntü İşleme API'sini kullanarak yüklenen görüntüler için otomatik resim yazısı oluşturmayı öğrendiniz. Bir sonraki modülde Azure App Service Kimlik Doğrulama hizmetini kullanarak uygulamaya kimlik doğrulama özelliği eklemeyi öğreneceksiniz.