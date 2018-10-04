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
ms.openlocfilehash: 0f86f2698a3a0c1e20272c335b63faf03b4b92d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460033"
---
Bu öğreticide HTML tabanlı kullanıcı arabirimine sahip basit bir web uygulaması dağıtacaksınız. Sunucusuz arka uç, uygulamanın görüntüleri yüklemesini ve otomatik olarak bunları açıklayan resim yazıları almasını sağlar.

![Web uygulamasını çalıştırma](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

Aşağıdaki diyagramda, uygulamada kullanılan Azure hizmetleri gösterilmiştir:

1. Blob Depolama, statik web içeriğini (HTML, CSS, JS) sunar ve görüntüleri depolar.
2. Azure İşlevleri görüntü yükleme, yeniden boyutlandırma ve meta veri depolama işlemlerini yönetir.
3. Cosmos DB, görüntü meta verilerini depolar.
4. Logic Apps, Görüntü İşleme API'sinden resim yazılarını alır.
5. Azure Active Directory, kullanıcı kimlik doğrulamasını yönetir.

![Çözüm mimarisi diyagramı](media/functions-first-serverless-web-app/0-architecture.jpg)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Blob depolamasını statik bir web sitesini ve yüklenen görüntüleri barındıracak şekilde yapılandırma.
> * Azure İşlevleri'ni kullanarak Azure Blob depolamasına görüntü yükleme.
> * Azure İşlevleri'ni kullanarak görüntüleri yeniden boyutlandırma.
> * Görüntü meta verilerini Azure Cosmos DB'de depolama.
> * Bilişsel Hizmetler Görüntü İşleme API'sini kullanarak görüntü resim yazılarını otomatik olarak oluşturma.
> * Azure Active Directory ile kullanıcıların kimliğini doğrulayarak web uygulamasının güvenliğini sağlama.
