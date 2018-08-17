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
<span data-ttu-id="d3fc0-103">Azure App Service Kimlik Doğrulama, Azure İşlev uygulamasında kullanıma hazır kimlik doğrulaması desteği sunar.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-103">Azure App Service Authentication enables turn-key authentication support in an Azure Function app.</span></span> <span data-ttu-id="d3fc0-104">Facebook, Twitter, Microsoft Account, Google ve Azure Active Directory ile sorunsuz tümleştirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-104">It integrates seamlessly with Facebook, Twitter, Microsoft Account, Google, and Azure Active Directory.</span></span> <span data-ttu-id="d3fc0-105">Web uygulamanızın arka uç API'lerini korumak için App Service Kimlik Doğrulama hizmeti ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-105">You'll add App Service Authentication to protect the backend APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="d3fc0-106">App Service Kimlik Doğrulamayı Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="d3fc0-106">Enable App Service Authentication</span></span>

1. <span data-ttu-id="d3fc0-107">İşlev uygulamasını Azure portalda açın.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-107">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="d3fc0-108">**Platform özellikleri** bölümünde **Kimlik Doğrulama/Yetkilendirme**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-108">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Kimlik Doğrulama/Yetkilendirme'yi seçin](media/functions-first-serverless-web-app/6-authorization.jpg)

1. <span data-ttu-id="d3fc0-110">Aşağıdaki değerleri seçin:</span><span class="sxs-lookup"><span data-stu-id="d3fc0-110">Select the following values:</span></span>
    
    | <span data-ttu-id="d3fc0-111">Ayar</span><span class="sxs-lookup"><span data-stu-id="d3fc0-111">Setting</span></span>      |  <span data-ttu-id="d3fc0-112">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="d3fc0-112">Suggested value</span></span>   | <span data-ttu-id="d3fc0-113">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d3fc0-113">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="d3fc0-114">**App Service Kimlik Doğrulama**</span><span class="sxs-lookup"><span data-stu-id="d3fc0-114">**App Service Authentication**</span></span> | <span data-ttu-id="d3fc0-115">Açık</span><span class="sxs-lookup"><span data-stu-id="d3fc0-115">On</span></span> | <span data-ttu-id="d3fc0-116">Kimlik doğrulamayı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-116">Enable authentication.</span></span> |
    | <span data-ttu-id="d3fc0-117">**İsteğin kimliği doğrulanmadığında gerçekleştirilecek eylem**</span><span class="sxs-lookup"><span data-stu-id="d3fc0-117">**Action when request is not authenticated**</span></span> | <span data-ttu-id="d3fc0-118">Azure Active Directory ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="d3fc0-118">Log in with Azure Active Directory</span></span> | <span data-ttu-id="d3fc0-119">Yapılandırılan kimlik doğrulama yöntemlerinden (aşağıda) birini seçin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-119">Select a configured authentication method (below).</span></span> |
    | <span data-ttu-id="d3fc0-120">**Kimlik Doğrulama Sağlayıcıları**</span><span class="sxs-lookup"><span data-stu-id="d3fc0-120">**Authentication Providers**</span></span> | <span data-ttu-id="d3fc0-121">Aşağıya bakın</span><span class="sxs-lookup"><span data-stu-id="d3fc0-121">See below</span></span> | <span data-ttu-id="d3fc0-122">Aşağıya bakın</span><span class="sxs-lookup"><span data-stu-id="d3fc0-122">See below</span></span> |
    | <span data-ttu-id="d3fc0-123">**Belirteç deposu**</span><span class="sxs-lookup"><span data-stu-id="d3fc0-123">**Token store**</span></span> | <span data-ttu-id="d3fc0-124">Açık</span><span class="sxs-lookup"><span data-stu-id="d3fc0-124">On</span></span> | <span data-ttu-id="d3fc0-125">App Service'in belirteçleri depolamasını ve yönetmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-125">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="d3fc0-126">**İzin verilen dış yönlendirme URL'leri**</span><span class="sxs-lookup"><span data-stu-id="d3fc0-126">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="d3fc0-127">Uygulamanızın URL'si, örneğin: https://firstserverlessweb.z4.web.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="d3fc0-127">The URL of your application, for example: https://firstserverlessweb.z4.web.core.windows.net/</span></span> | <span data-ttu-id="d3fc0-128">Kullanıcının kimliği doğrulandıktan sonra App Service'in yönlendirme için kullanabileceğin URL'ler.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-128">URL(s) that App Service is allowed to redirect to after a user is authenticated.</span></span> |

1. <span data-ttu-id="d3fc0-129">**Azure Active Directory**'yi seçerek **Azure Active Directory Ayarları**'nı açın.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-129">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="d3fc0-130">**Yönetim Modu** olarak **Hızlı**'yı seçin ve aşağıdaki bilgileri girin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-130">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="d3fc0-131">Ayar</span><span class="sxs-lookup"><span data-stu-id="d3fc0-131">Setting</span></span>      |  <span data-ttu-id="d3fc0-132">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="d3fc0-132">Suggested value</span></span>   | <span data-ttu-id="d3fc0-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d3fc0-133">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="d3fc0-134">**Yönetim modu**</span><span class="sxs-lookup"><span data-stu-id="d3fc0-134">**Management mode**</span></span> | <span data-ttu-id="d3fc0-135">Hızlı, Yeni AD uygulaması oluştur</span><span class="sxs-lookup"><span data-stu-id="d3fc0-135">Express, Create new AD app</span></span> | <span data-ttu-id="d3fc0-136">Otomatik olarak bir hizmet sorumlusu ve Azure Active Directory kimlik doğrulaması ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-136">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="d3fc0-137">**Uygulama oluşturma**</span><span class="sxs-lookup"><span data-stu-id="d3fc0-137">**Create app**</span></span> | <span data-ttu-id="d3fc0-138">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="d3fc0-138">my-serverless-webapp</span></span> | <span data-ttu-id="d3fc0-139">Benzersiz bir uygulama adı girin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-139">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="d3fc0-140">Azure Active Directory ayarlarını kaydetmek için **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-140">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Kimlik Doğrulama/Yetkilendirme ve Azure Active Directory ayarları](media/functions-first-serverless-web-app/6-create-aad.png)

1. <span data-ttu-id="d3fc0-142">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-142">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="d3fc0-143">Kimlik doğrulamasını etkinleştirmek için web uygulamasını değiştirme</span><span class="sxs-lookup"><span data-stu-id="d3fc0-143">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="d3fc0-144">Cloud Shell'de geçerli dizinin **www/dist** klasörü olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-144">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="d3fc0-145">İşlev uygulamanızda kimlik doğrulamasını etkinleştirmek için **settings.js** dosyasına aşağıdaki kod satırını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-145">To enable authentication in your function app, append the following line of code to **settings.js**.</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="d3fc0-146">Değişikliği yapın ve aşağıdaki komutu veya VIM gibi bir komut satırı düzenleyicisi kullanarak sonucu görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-146">Make the change and view the result by using the following commands or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="d3fc0-147">Dosyada değişiklik yapıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="d3fc0-148">Dosyayı Blob depolamaya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="d3fc0-149">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="d3fc0-149">Test the application</span></span>

1. <span data-ttu-id="d3fc0-150">Uygulamayı bir tarayıcıda açın.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-150">Open the application in a browser.</span></span> <span data-ttu-id="d3fc0-151">**Oturum aç**'a tıklayın ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="d3fc0-152">Bir görüntü dosyası seçin ve yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-152">Select an image file and upload it.</span></span>

    ![Oturum açma sayfası](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="d3fc0-154">Özet</span><span class="sxs-lookup"><span data-stu-id="d3fc0-154">Summary</span></span>

<span data-ttu-id="d3fc0-155">Bu modülde Azure App Service Kimlik Doğrulama hizmetini kullanarak uygulamaya kimlik doğrulama özelliği eklemeyi öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="d3fc0-155">In this module, you learned how to add authentication to the applcation using Azure App Service Authentication.</span></span>
