<span data-ttu-id="8ac05-101">Azure Blob depolama, statik dosyaları barındırmak için kullanılabilecek düşük maliyetli ve yüksek oranda ölçeklenebilir bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="8ac05-101">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="8ac05-102">Bu öğreticide bu hizmeti kullanarak oluşturduğunuz web uygulaması için statik içerik (HTML, JavaScript, CSS gibi) sunacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8ac05-102">For this tutorial, you use it to serve static content (for example, HTML, JavaScript, CSS) for the web app that you build.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="8ac05-103">Depolama hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="8ac05-103">Create a Storage account</span></span>

<span data-ttu-id="8ac05-104">Depolama hesabı tabloları, kuyrukları, dosyaları, blobları (nesneleri) ve sanal makine disklerini depolamanızı sağlayan bir Azure kaynağıdır.</span><span class="sxs-lookup"><span data-stu-id="8ac05-104">A Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="8ac05-105">**Enter focus mode** (Odak moduna gir) düğmesini seçerek Cloud Shell (Bash) oturumu açın.</span><span class="sxs-lookup"><span data-stu-id="8ac05-105">Log in to the Cloud Shell (Bash), by selecting the **Enter focus mode** button.</span></span> <span data-ttu-id="8ac05-106">Tarayıcı pencerenizin genişliğine bağlı olarak bu düğme sayfanın sağ üst veya alt kısmında olabilir.</span><span class="sxs-lookup"><span data-stu-id="8ac05-106">This button is at the top right or the bottom of the page, depending on how wide your browser window is.</span></span> <span data-ttu-id="8ac05-107">Odak modu, Cloud Shell penceresini tarayıcı penceresinin sağ tarafına yerleştirerek öğreticide gösterilen komutları kolayca yürütmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="8ac05-107">Focus mode docks a Cloud Shell window on the right side of your browser window, so you can easily execute commands that are shown in the tutorial.</span></span>

1. <span data-ttu-id="8ac05-108">Azure'da Kaynak Grubu, kullanım kolaylığı sağlamak için ilgili Azure kaynaklarının yer aldığı bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="8ac05-108">In Azure, a Resource Group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="8ac05-109">**first-serverless-app** adlı yeni bir kaynak grubu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8ac05-109">Create a new resource group named **first-serverless-app**.</span></span>

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. <span data-ttu-id="8ac05-110">Bu öğreticideki statik içerik (HTML, CSS ve JavaScript dosyaları) Blob Depolamada barındırılır.</span><span class="sxs-lookup"><span data-stu-id="8ac05-110">The static content (HTML, CSS, and JavaScript files) for this tutorial is hosted in Blob Storage.</span></span> <span data-ttu-id="8ac05-111">Blob Depolama için bir Depolama hesabı gerekir.</span><span class="sxs-lookup"><span data-stu-id="8ac05-111">Blob Storage requires a Storage account.</span></span> <span data-ttu-id="8ac05-112">Kaynak grubunda bir Depolama hesabı (genel amaçlı V2) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8ac05-112">Create a Storage account (general purpose V2) in the resource group.</span></span> <span data-ttu-id="8ac05-113">`<storage account name>` yerine benzersiz bir ad yazın.</span><span class="sxs-lookup"><span data-stu-id="8ac05-113">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. <span data-ttu-id="8ac05-114">[Azure portalın](https://portal.azure.com) üst tarafındaki Arama çubuğunu kullanarak yeni oluşturduğunuz depolama hesabını bulun ve açın.</span><span class="sxs-lookup"><span data-stu-id="8ac05-114">Using the Search bar at the top of the [Azure portal](https://portal.azure.com), find the storage account you just created and open it.</span></span>

1. <span data-ttu-id="8ac05-115">Sol gezinti bölmesinde **Statik web sitesi (önizleme)** öğesini seçerek statik web sitesi barındırma için bir kapsayıcı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8ac05-115">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="8ac05-116">Statik web sitesini etkinleştirmek için **Etkin**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="8ac05-116">Select **Enabled** to enable static website.</span></span>
    - <span data-ttu-id="8ac05-117">Dizin belge adı olarak *index.html* yazın.</span><span class="sxs-lookup"><span data-stu-id="8ac05-117">Enter *index.html* as the index document name.</span></span> <span data-ttu-id="8ac05-118">Alanda gri renkte *index.html* yazılıdır ancak bu metin yalnızca örnek olarak verilmiştir, bu alana gerekli değeri girmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8ac05-118">The field already has *index.html* in a gray font but this is only example text; you still have to enter that value in the field.</span></span>
    - <span data-ttu-id="8ac05-119">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8ac05-119">Click **Save**.</span></span>
    
    ![Statik web sitesi ayarlarını girin](media/functions-first-serverless-web-app/1-storage-static-website.png)

1. <span data-ttu-id="8ac05-121">**Birincil Uç Nokta** değerini öğreticide çalışırken kolay bir şekilde kopyalayabileceğiniz bir yere kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8ac05-121">Save the **Primary Endpoint** somewhere you can conveniently copy it from while working through the tutorial.</span></span> <span data-ttu-id="8ac05-122">Bu, web uygulamanızın URL'sidir.</span><span class="sxs-lookup"><span data-stu-id="8ac05-122">This is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="8ac05-123">Web uygulamasını yükleme</span><span class="sxs-lookup"><span data-stu-id="8ac05-123">Upload the web application</span></span>

1. <span data-ttu-id="8ac05-124">Bu öğreticide oluşturduğunuz uygulamanın kaynak dosyaları, [GitHub deposunda](https://github.com/Azure-Samples/functions-first-serverless-web-application) bulunur.</span><span class="sxs-lookup"><span data-stu-id="8ac05-124">The source files for the application that you build in this tutorial are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="8ac05-125">Cloud Shell'de ana dizininizde olduğunuzdan emin olun ve bu depoyu kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="8ac05-125">Make sure that you are in your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="8ac05-126">Depo `/home/<username>/functions-first-serverless-web-application` dizinine kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="8ac05-126">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`.</span></span>

1. <span data-ttu-id="8ac05-127">İstemci tarafı web uygulaması **www** klasöründe bulunur ve Vue.js JavaScript framework'ü kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="8ac05-127">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="8ac05-128">Uygulamanın bağımlılıklarını yüklemek ve uygulamayı derlemek için ilgili klasöre geçin ve npm komutlarını çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="8ac05-128">Change into the folder and run npm commands to install the application's dependencies and build the application.</span></span> <span data-ttu-id="8ac05-129">Bu komutların sonuncusunun tamamlanması birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="8ac05-129">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="8ac05-130">Uygulama, **dist** klasöründe oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8ac05-130">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="8ac05-131">Geçerli dizini **dist** olarak değiştirin ve uygulamayı **$web** Blob kapsayıcısına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="8ac05-131">Change the current directory to **dist** and upload the application to the **$web** Blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="8ac05-132">Uygulamayı görüntülemek için depolama statik web sitelerinin birincil uç nokta URL'sini web tarayıcısında açın.</span><span class="sxs-lookup"><span data-stu-id="8ac05-132">Open the Storage static websites primary endpoint URL in a web browser to view the application.</span></span>

    ![İlk sunucusuz web uygulaması ana sayfası](media/functions-first-serverless-web-app/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="8ac05-134">Özet</span><span class="sxs-lookup"><span data-stu-id="8ac05-134">Summary</span></span>

<span data-ttu-id="8ac05-135">Bu modülde Depolama hesabı içeren **first-serverless-app** adlı bir kaynak grubu oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="8ac05-135">In this module, you created a resource group named **first-serverless-app** containing a Storage account.</span></span> <span data-ttu-id="8ac05-136">Depolama hesabındaki **$web** adlı blob kapsayıcısı web uygulamanızın statik içeriğini depolar ve içeriğin genel kullanıma sunulmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="8ac05-136">A blob container named **$web** in the Storage account stores the static content for your web application and makes the content available publicly.</span></span> <span data-ttu-id="8ac05-137">Bir sonraki modülde sunucusuz işlev kullanarak bu web uygulamasından Blob depolamaya görüntü yüklemeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="8ac05-137">Next, you learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>
