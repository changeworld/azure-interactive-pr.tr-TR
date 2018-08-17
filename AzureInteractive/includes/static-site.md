Azure Blob depolama, statik dosyaları barındırmak için kullanılabilecek düşük maliyetli ve yüksek oranda ölçeklenebilir bir hizmettir. Bu öğreticide bu hizmeti kullanarak oluşturduğunuz web uygulaması için statik içerik (HTML, JavaScript, CSS gibi) sunacaksınız.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Depolama hesabı tabloları, kuyrukları, dosyaları, blobları (nesneleri) ve sanal makine disklerini depolamanızı sağlayan bir Azure kaynağıdır.

1. **Enter focus mode** (Odak moduna gir) düğmesini seçerek Cloud Shell (Bash) oturumu açın. Tarayıcı pencerenizin genişliğine bağlı olarak bu düğme sayfanın sağ üst veya alt kısmında olabilir. Odak modu, Cloud Shell penceresini tarayıcı penceresinin sağ tarafına yerleştirerek öğreticide gösterilen komutları kolayca yürütmenizi sağlar.

1. Azure'da Kaynak Grubu, kullanım kolaylığı sağlamak için ilgili Azure kaynaklarının yer aldığı bir kapsayıcıdır. **first-serverless-app** adlı yeni bir kaynak grubu oluşturun.

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. Bu öğreticideki statik içerik (HTML, CSS ve JavaScript dosyaları) Blob Depolamada barındırılır. Blob Depolama için bir Depolama hesabı gerekir. Kaynak grubunda bir Depolama hesabı (genel amaçlı V2) oluşturun. `<storage account name>` yerine benzersiz bir ad yazın.

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. [Azure portalın](https://portal.azure.com) üst tarafındaki Arama çubuğunu kullanarak yeni oluşturduğunuz depolama hesabını bulun ve açın.

1. Sol gezinti bölmesinde **Statik web sitesi (önizleme)** öğesini seçerek statik web sitesi barındırma için bir kapsayıcı yapılandırın.
    - Statik web sitesini etkinleştirmek için **Etkin**'i seçin.
    - Dizin belge adı olarak *index.html* yazın. Alanda gri renkte *index.html* yazılıdır ancak bu metin yalnızca örnek olarak verilmiştir, bu alana gerekli değeri girmeniz gerekir.
    - **Kaydet**’e tıklayın.
    
    ![Statik web sitesi ayarlarını girin](media/functions-first-serverless-web-app/1-storage-static-website.png)

1. **Birincil Uç Nokta** değerini öğreticide çalışırken kolay bir şekilde kopyalayabileceğiniz bir yere kaydedin. Bu, web uygulamanızın URL'sidir.

## <a name="upload-the-web-application"></a>Web uygulamasını yükleme

1. Bu öğreticide oluşturduğunuz uygulamanın kaynak dosyaları, [GitHub deposunda](https://github.com/Azure-Samples/functions-first-serverless-web-application) bulunur. Cloud Shell'de ana dizininizde olduğunuzdan emin olun ve bu depoyu kopyalayın.

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    Depo `/home/<username>/functions-first-serverless-web-application` dizinine kopyalanır.

1. İstemci tarafı web uygulaması **www** klasöründe bulunur ve Vue.js JavaScript framework'ü kullanılarak oluşturulmuştur. Uygulamanın bağımlılıklarını yüklemek ve uygulamayı derlemek için ilgili klasöre geçin ve npm komutlarını çalıştırır. Bu komutların sonuncusunun tamamlanması birkaç dakika sürebilir.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    Uygulama, **dist** klasöründe oluşturulur.

1. Geçerli dizini **dist** olarak değiştirin ve uygulamayı **$web** Blob kapsayıcısına yükleyin.

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. Uygulamayı görüntülemek için depolama statik web sitelerinin birincil uç nokta URL'sini web tarayıcısında açın.

    ![İlk sunucusuz web uygulaması ana sayfası](media/functions-first-serverless-web-app/1-app-screenshot-new.png)


## <a name="summary"></a>Özet

Bu modülde Depolama hesabı içeren **first-serverless-app** adlı bir kaynak grubu oluşturdunuz. Depolama hesabındaki **$web** adlı blob kapsayıcısı web uygulamanızın statik içeriğini depolar ve içeriğin genel kullanıma sunulmasını sağlar. Bir sonraki modülde sunucusuz işlev kullanarak bu web uygulamasından Blob depolamaya görüntü yüklemeyi öğreneceksiniz.
