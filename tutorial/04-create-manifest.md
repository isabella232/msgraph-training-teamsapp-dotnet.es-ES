<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="db7b9-101">El manifiesto de la aplicación describe cómo se integra la aplicación con Microsoft Teams y es necesario para instalar aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="db7b9-101">The app manifest describes how the app integrates with Microsoft Teams and is required to install apps.</span></span> <span data-ttu-id="db7b9-102">En esta sección, usará App Studio en el cliente de Microsoft Teams para generar un manifiesto.</span><span class="sxs-lookup"><span data-stu-id="db7b9-102">In this section you'll use App Studio in the Microsoft Teams client to generate a manifest.</span></span>

1. <span data-ttu-id="db7b9-103">Si aún no tiene instalado App Studio en Teams, [instálelo ahora](/microsoftteams/platform/concepts/build-and-test/app-studio-overview).</span><span class="sxs-lookup"><span data-stu-id="db7b9-103">If you do not already have App Studio installed in Teams, [install it now](/microsoftteams/platform/concepts/build-and-test/app-studio-overview).</span></span>

1. <span data-ttu-id="db7b9-104">Inicie App Studio en Microsoft Teams y seleccione el **Editor de manifiestos**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-104">Launch App Studio in Microsoft Teams and select the **Manifest editor**.</span></span>

1. <span data-ttu-id="db7b9-105">Seleccione **crear una nueva aplicación**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-105">Select **Create a new app**.</span></span>

    ![Captura de pantalla del editor de manifiestos en App Studio en Microsoft Teams](images/app-studio-01.png)

1. <span data-ttu-id="db7b9-107">En la página de detalles de la **aplicación** , rellene los campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="db7b9-107">On the **App details** page, fill in the required fields.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db7b9-108">Puede usar los iconos predeterminados en la sección de **Personalización de marca** o cargar los suyos propios.</span><span class="sxs-lookup"><span data-stu-id="db7b9-108">You can use the default icons in the **Branding** section or upload your own.</span></span>

1. <span data-ttu-id="db7b9-109">En el menú de la izquierda, seleccione **pestañas** en **capacidades**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-109">On the left-hand menu, select **Tabs** under **Capabilities**.</span></span>

1. <span data-ttu-id="db7b9-110">Seleccione **Agregar** en **Agregar una ficha personal**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-110">Select **Add** under **Add a personal tab**.</span></span>

    ![Una captura de pantalla de la página de pestañas en App Studio](images/app-studio-02.png)

1. <span data-ttu-id="db7b9-112">Rellene los campos como se indica a continuación, donde `YOUR_NGROK_URL` es la dirección URL de reenvío que ha copiado en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="db7b9-112">Fill in the fields as follows, where `YOUR_NGROK_URL` is the forwarding URL you copied in the previous section.</span></span> <span data-ttu-id="db7b9-113">Seleccione **Guardar** cuando haya terminado.</span><span class="sxs-lookup"><span data-stu-id="db7b9-113">Select **Save** when done.</span></span>

    - <span data-ttu-id="db7b9-114">**Name:**`Create event`</span><span class="sxs-lookup"><span data-stu-id="db7b9-114">**Name:** `Create event`</span></span>
    - <span data-ttu-id="db7b9-115">**Identificador de entidad:**`createEventTab`</span><span class="sxs-lookup"><span data-stu-id="db7b9-115">**Entity ID:** `createEventTab`</span></span>
    - <span data-ttu-id="db7b9-116">**Dirección URL del contenido:**`YOUR_NGROK_URL/newevent`</span><span class="sxs-lookup"><span data-stu-id="db7b9-116">**Content URL:** `YOUR_NGROK_URL/newevent`</span></span>

1. <span data-ttu-id="db7b9-117">Seleccione **Agregar** en **Agregar una ficha personal**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-117">Select **Add** under **Add a personal tab**.</span></span>

1. <span data-ttu-id="db7b9-118">Rellene los campos como se indica a continuación, donde `YOUR_NGROK_URL` es la dirección URL de reenvío que ha copiado en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="db7b9-118">Fill in the fields as follows, where `YOUR_NGROK_URL` is the forwarding URL you copied in the previous section.</span></span> <span data-ttu-id="db7b9-119">Seleccione **Guardar** cuando haya terminado.</span><span class="sxs-lookup"><span data-stu-id="db7b9-119">Select **Save** when done.</span></span>

    - <span data-ttu-id="db7b9-120">**Name:**`Graph calendar`</span><span class="sxs-lookup"><span data-stu-id="db7b9-120">**Name:** `Graph calendar`</span></span>
    - <span data-ttu-id="db7b9-121">**Identificador de entidad:**`calendarTab`</span><span class="sxs-lookup"><span data-stu-id="db7b9-121">**Entity ID:** `calendarTab`</span></span>
    - <span data-ttu-id="db7b9-122">**Dirección URL del contenido:**`YOUR_NGROK_URL`</span><span class="sxs-lookup"><span data-stu-id="db7b9-122">**Content URL:** `YOUR_NGROK_URL`</span></span>

1. <span data-ttu-id="db7b9-123">En el menú de la izquierda, seleccione **dominios y permisos** en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-123">On the left-hand menu, select **Domains and permissions** under **Finish**.</span></span>

1. <span data-ttu-id="db7b9-124">Establezca el **identificador** de la aplicación AAD en el identificador de la aplicación desde el registro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db7b9-124">Set the **AAD App ID** to the application ID from your app registration.</span></span>

1. <span data-ttu-id="db7b9-125">Establezca el campo de **Inicio de sesión único** en el URI del identificador de la aplicación desde el registro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db7b9-125">Set the **Single-Sign-On** field to the application ID URI from your app registration.</span></span>

1. <span data-ttu-id="db7b9-126">En el menú de la izquierda, seleccione **probar y distribuir** en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-126">On the left-hand menu, select **Test and distribute** under **Finish**.</span></span> <span data-ttu-id="db7b9-127">Seleccione **Descargar**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-127">Select **Download**.</span></span>

1. <span data-ttu-id="db7b9-128">Cree un nuevo directorio en la raíz del proyecto denominado **manifest**.</span><span class="sxs-lookup"><span data-stu-id="db7b9-128">Create a new directory in the root of the project named **Manifest**.</span></span> <span data-ttu-id="db7b9-129">Extraiga el contenido del archivo ZIP descargado en este directorio.</span><span class="sxs-lookup"><span data-stu-id="db7b9-129">Extract the contents of the downloaded ZIP file to this directory.</span></span>
