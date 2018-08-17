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
ms.openlocfilehash: d1f9a07ce3d3b096b498e48b5c4f68c3454b2b37
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079711"
---
Azure App Service Kimlik Doğrulama, Azure İşlev uygulamasında kullanıma hazır kimlik doğrulaması desteği sunar. Facebook, Twitter, Microsoft Account, Google ve Azure Active Directory ile sorunsuz tümleştirme sağlar. Web uygulamanızın arka uç API'lerini korumak için App Service Kimlik Doğrulama hizmeti ekleyeceksiniz.

## <a name="enable-app-service-authentication"></a>App Service Kimlik Doğrulamayı Etkinleştirme

1. İşlev uygulamasını Azure portalda açın.

1. **Platform özellikleri** bölümünde **Kimlik Doğrulama/Yetkilendirme**'yi seçin.

    ![Kimlik Doğrulama/Yetkilendirme'yi seçin](media/functions-first-serverless-web-app/6-authorization.jpg)

1. Aşağıdaki değerleri seçin:
    
    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | ---|
    | **App Service Kimlik Doğrulama** | Açık | Kimlik doğrulamayı etkinleştirin. |
    | **İsteğin kimliği doğrulanmadığında gerçekleştirilecek eylem** | Azure Active Directory ile oturum açma | Yapılandırılan kimlik doğrulama yöntemlerinden (aşağıda) birini seçin. |
    | **Kimlik Doğrulama Sağlayıcıları** | Aşağıya bakın | Aşağıya bakın |
    | **Belirteç deposu** | Açık | App Service'in belirteçleri depolamasını ve yönetmesini sağlar. |
    | **İzin verilen dış yönlendirme URL'leri** | Uygulamanızın URL'si, örneğin: https://firstserverlessweb.z4.web.core.windows.net/ | Kullanıcının kimliği doğrulandıktan sonra App Service'in yönlendirme için kullanabileceğin URL'ler. |

1. **Azure Active Directory**'yi seçerek **Azure Active Directory Ayarları**'nı açın.

    1. **Yönetim Modu** olarak **Hızlı**'yı seçin ve aşağıdaki bilgileri girin.
    
        | Ayar      |  Önerilen değer   | Açıklama                                        |
        | --- | --- | ---|
        | **Yönetim modu** | Hızlı, Yeni AD uygulaması oluştur | Otomatik olarak bir hizmet sorumlusu ve Azure Active Directory kimlik doğrulaması ayarlayın. |
        | **Uygulama oluşturma** | my-serverless-webapp | Benzersiz bir uygulama adı girin. |
    
    1. Azure Active Directory ayarlarını kaydetmek için **Tamam**'a tıklayın.

    ![Kimlik Doğrulama/Yetkilendirme ve Azure Active Directory ayarları](media/functions-first-serverless-web-app/6-create-aad.png)

1. **Kaydet**’e tıklayın.


## <a name="modify-the-web-app-to-enable-authentication"></a>Kimlik doğrulamasını etkinleştirmek için web uygulamasını değiştirme

1. Cloud Shell'de geçerli dizinin **www/dist** klasörü olduğundan emin olun.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. İşlev uygulamanızda kimlik doğrulamasını etkinleştirmek için **settings.js** dosyasına aşağıdaki kod satırını ekleyin.

    `window.authEnabled = true`

    Değişikliği yapın ve aşağıdaki komutu veya VIM gibi bir komut satırı düzenleyicisi kullanarak sonucu görüntüleyin.

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    Dosyada değişiklik yapıldığını doğrulayın.

    ```azurecli
    cat settings.js
    ```

1. Dosyayı Blob depolamaya yükleyin.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a>Uygulamayı test etme

1. Uygulamayı bir tarayıcıda açın. **Oturum aç**'a tıklayın ve oturum açın.

1. Bir görüntü dosyası seçin ve yükleyin.

    ![Oturum açma sayfası](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a>Özet

Bu modülde Azure App Service Kimlik Doğrulama hizmetini kullanarak uygulamaya kimlik doğrulama özelliği eklemeyi öğrendiniz.
