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
ms.openlocfilehash: 426a7287458a48d1bda220ad1a5f067be2ce77d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460050"
---
<span data-ttu-id="16a33-103">Azure App Service Kimlik Doğrulama, Azure İşlev uygulamasında kullanıma hazır kimlik doğrulaması desteği sunar.</span><span class="sxs-lookup"><span data-stu-id="16a33-103">Azure App Service Authentication enables turn-key authentication support in an Azure Function app.</span></span> <span data-ttu-id="16a33-104">Facebook, Twitter, Microsoft Account, Google ve Azure Active Directory ile sorunsuz tümleştirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="16a33-104">It integrates seamlessly with Facebook, Twitter, Microsoft Account, Google, and Azure Active Directory.</span></span> <span data-ttu-id="16a33-105">Web uygulamanızın arka uç API'lerini korumak için App Service Kimlik Doğrulama hizmeti ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="16a33-105">You'll add App Service Authentication to protect the backend APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="16a33-106">App Service Kimlik Doğrulamayı Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="16a33-106">Enable App Service Authentication</span></span>

1. <span data-ttu-id="16a33-107">İşlev uygulamasını Azure portalda açın.</span><span class="sxs-lookup"><span data-stu-id="16a33-107">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="16a33-108">**Platform özellikleri** bölümünde **Kimlik Doğrulama/Yetkilendirme**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="16a33-108">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Kimlik Doğrulama/Yetkilendirme'yi seçin](media/functions-first-serverless-web-app/6-authorization.jpg)

1. <span data-ttu-id="16a33-110">Aşağıdaki değerleri seçin:</span><span class="sxs-lookup"><span data-stu-id="16a33-110">Select the following values:</span></span>
    
    | <span data-ttu-id="16a33-111">Ayar</span><span class="sxs-lookup"><span data-stu-id="16a33-111">Setting</span></span>      |  <span data-ttu-id="16a33-112">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="16a33-112">Suggested value</span></span>   | <span data-ttu-id="16a33-113">Açıklama</span><span class="sxs-lookup"><span data-stu-id="16a33-113">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="16a33-114">**App Service Kimlik Doğrulama**</span><span class="sxs-lookup"><span data-stu-id="16a33-114">**App Service Authentication**</span></span> | <span data-ttu-id="16a33-115">Açık</span><span class="sxs-lookup"><span data-stu-id="16a33-115">On</span></span> | <span data-ttu-id="16a33-116">Kimlik doğrulamayı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="16a33-116">Enable authentication.</span></span> |
    | <span data-ttu-id="16a33-117">**İsteğin kimliği doğrulanmadığında gerçekleştirilecek eylem**</span><span class="sxs-lookup"><span data-stu-id="16a33-117">**Action when request is not authenticated**</span></span> | <span data-ttu-id="16a33-118">Azure Active Directory ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="16a33-118">Log in with Azure Active Directory</span></span> | <span data-ttu-id="16a33-119">Yapılandırılan kimlik doğrulama yöntemlerinden (aşağıda) birini seçin.</span><span class="sxs-lookup"><span data-stu-id="16a33-119">Select a configured authentication method (below).</span></span> |
    | <span data-ttu-id="16a33-120">**Kimlik Doğrulama Sağlayıcıları**</span><span class="sxs-lookup"><span data-stu-id="16a33-120">**Authentication Providers**</span></span> | <span data-ttu-id="16a33-121">Aşağıya bakın</span><span class="sxs-lookup"><span data-stu-id="16a33-121">See below</span></span> | <span data-ttu-id="16a33-122">Aşağıya bakın</span><span class="sxs-lookup"><span data-stu-id="16a33-122">See below</span></span> |
    | <span data-ttu-id="16a33-123">**Belirteç deposu**</span><span class="sxs-lookup"><span data-stu-id="16a33-123">**Token store**</span></span> | <span data-ttu-id="16a33-124">Açık</span><span class="sxs-lookup"><span data-stu-id="16a33-124">On</span></span> | <span data-ttu-id="16a33-125">App Service'in belirteçleri depolamasını ve yönetmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="16a33-125">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="16a33-126">**İzin verilen dış yönlendirme URL'leri**</span><span class="sxs-lookup"><span data-stu-id="16a33-126">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="16a33-127">Uygulamanızın URL'si, örneğin: https://firstserverlessweb.z4.web.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="16a33-127">The URL of your application, for example: https://firstserverlessweb.z4.web.core.windows.net/</span></span> | <span data-ttu-id="16a33-128">Kullanıcının kimliği doğrulandıktan sonra App Service'in yönlendirme için kullanabileceğin URL'ler.</span><span class="sxs-lookup"><span data-stu-id="16a33-128">URL(s) that App Service is allowed to redirect to after a user is authenticated.</span></span> |

1. <span data-ttu-id="16a33-129">**Azure Active Directory**'yi seçerek **Azure Active Directory Ayarları**'nı açın.</span><span class="sxs-lookup"><span data-stu-id="16a33-129">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="16a33-130">**Yönetim Modu** olarak **Hızlı**'yı seçin ve aşağıdaki bilgileri girin.</span><span class="sxs-lookup"><span data-stu-id="16a33-130">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="16a33-131">Ayar</span><span class="sxs-lookup"><span data-stu-id="16a33-131">Setting</span></span>      |  <span data-ttu-id="16a33-132">Önerilen değer</span><span class="sxs-lookup"><span data-stu-id="16a33-132">Suggested value</span></span>   | <span data-ttu-id="16a33-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="16a33-133">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="16a33-134">**Yönetim modu**</span><span class="sxs-lookup"><span data-stu-id="16a33-134">**Management mode**</span></span> | <span data-ttu-id="16a33-135">Hızlı, Yeni AD uygulaması oluştur</span><span class="sxs-lookup"><span data-stu-id="16a33-135">Express, Create new AD app</span></span> | <span data-ttu-id="16a33-136">Otomatik olarak bir hizmet sorumlusu ve Azure Active Directory kimlik doğrulaması ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="16a33-136">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="16a33-137">**Uygulama oluşturma**</span><span class="sxs-lookup"><span data-stu-id="16a33-137">**Create app**</span></span> | <span data-ttu-id="16a33-138">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="16a33-138">my-serverless-webapp</span></span> | <span data-ttu-id="16a33-139">Benzersiz bir uygulama adı girin.</span><span class="sxs-lookup"><span data-stu-id="16a33-139">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="16a33-140">Azure Active Directory ayarlarını kaydetmek için **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="16a33-140">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Kimlik Doğrulama/Yetkilendirme ve Azure Active Directory ayarları](media/functions-first-serverless-web-app/6-create-aad.png)

1. <span data-ttu-id="16a33-142">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="16a33-142">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="16a33-143">Kimlik doğrulamasını etkinleştirmek için web uygulamasını değiştirme</span><span class="sxs-lookup"><span data-stu-id="16a33-143">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="16a33-144">Cloud Shell'de geçerli dizinin **www/dist** klasörü olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="16a33-144">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="16a33-145">İşlev uygulamanızda kimlik doğrulamasını etkinleştirmek için **settings.js** dosyasına aşağıdaki kod satırını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="16a33-145">To enable authentication in your function app, append the following line of code to **settings.js**.</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="16a33-146">Değişikliği yapın ve aşağıdaki komutu veya VIM gibi bir komut satırı düzenleyicisi kullanarak sonucu görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="16a33-146">Make the change and view the result by using the following commands or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="16a33-147">Dosyada değişiklik yapıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="16a33-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="16a33-148">Dosyayı Blob depolamaya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="16a33-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="16a33-149">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="16a33-149">Test the application</span></span>

1. <span data-ttu-id="16a33-150">Uygulamayı bir tarayıcıda açın.</span><span class="sxs-lookup"><span data-stu-id="16a33-150">Open the application in a browser.</span></span> <span data-ttu-id="16a33-151">**Oturum aç**'a tıklayın ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="16a33-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="16a33-152">Bir görüntü dosyası seçin ve yükleyin.</span><span class="sxs-lookup"><span data-stu-id="16a33-152">Select an image file and upload it.</span></span>

    ![Oturum açma sayfası](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="16a33-154">Özet</span><span class="sxs-lookup"><span data-stu-id="16a33-154">Summary</span></span>

<span data-ttu-id="16a33-155">Bu modülde Azure App Service Kimlik Doğrulama hizmetini kullanarak uygulamaya kimlik doğrulama özelliği eklemeyi öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="16a33-155">In this module, you learned how to add authentication to the applcation using Azure App Service Authentication.</span></span>
