<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="663aa-101">Las aplicaciones de la pestaña de Microsoft Teams tienen varias opciones para autenticar al usuario y llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="663aa-101">Microsoft Teams tab applications have multiple options to authenticate the user and call Microsoft Graph.</span></span> <span data-ttu-id="663aa-102">En este ejercicio, implementará una pestaña que realiza un [Inicio de sesión único](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) para obtener un token de autenticación en el cliente y, a continuación, usa el [flujo "en nombre de](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) " en el servidor para intercambiar ese token para obtener acceso a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="663aa-102">In this exercise, you'll implement a tab that does [single sign-on](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) to get an auth token on the client, then uses the [on-behalf-of flow](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) on the server to exchange that token to get access to Microsoft Graph.</span></span>

<span data-ttu-id="663aa-103">Para otras alternativas, vea lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="663aa-103">For other alternatives, see the following.</span></span>

- <span data-ttu-id="663aa-104">[Cree una pestaña de Microsoft Teams con el kit de herramientas de Microsoft Graph](/graph/toolkit/get-started/build-a-microsoft-teams-tab).</span><span class="sxs-lookup"><span data-stu-id="663aa-104">[Build a Microsoft Teams tab with the Microsoft Graph Toolkit](/graph/toolkit/get-started/build-a-microsoft-teams-tab).</span></span> <span data-ttu-id="663aa-105">Este ejemplo es totalmente del lado cliente y usa el kit de herramientas de Microsoft Graph para controlar la autenticación y realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="663aa-105">This sample is completely client-side, and uses the Microsoft Graph Toolkit to handle authentication and making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="663aa-106">[Ejemplo de autenticación de Microsoft Teams](https://github.com/OfficeDev/microsoft-teams-sample-auth-node).</span><span class="sxs-lookup"><span data-stu-id="663aa-106">[Microsoft Teams Authentication Sample](https://github.com/OfficeDev/microsoft-teams-sample-auth-node).</span></span> <span data-ttu-id="663aa-107">Este ejemplo contiene varios ejemplos que cubren distintos escenarios de autenticación.</span><span class="sxs-lookup"><span data-stu-id="663aa-107">This sample contains multiple examples covering different authentication scenarios.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="663aa-108">Cree el proyecto</span><span class="sxs-lookup"><span data-stu-id="663aa-108">Create the project</span></span>

<span data-ttu-id="663aa-109">Empiece por crear una aplicación Web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="663aa-109">Start by creating an ASP.NET Core web app.</span></span>

1. <span data-ttu-id="663aa-110">Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="663aa-110">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="663aa-111">Ejecute el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="663aa-111">Run the following command.</span></span>

    ```Shell
    dotnet new webapp -o GraphTutorial
    ```

1. <span data-ttu-id="663aa-112">Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="663aa-112">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

1. <span data-ttu-id="663aa-113">Abra el explorador y vaya a `https://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="663aa-113">Open your browser and browse to `https://localhost:5001`.</span></span> <span data-ttu-id="663aa-114">Si todo funciona, debería ver una página de núcleo de ASP.NET predeterminada.</span><span class="sxs-lookup"><span data-stu-id="663aa-114">If everything is working, you should see a default ASP.NET Core page.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="663aa-115">Si recibe una advertencia de que el certificado para **localhost** no es de confianza, puede usar .net Core CLI para instalar y confiar en el certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="663aa-115">If you receive a warning that the certificate for **localhost** is un-trusted you can use the .NET Core CLI to install and trust the development certificate.</span></span> <span data-ttu-id="663aa-116">Consulte [forzar HTTPS en ASP.net Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) para obtener instrucciones sobre sistemas operativos específicos.</span><span class="sxs-lookup"><span data-stu-id="663aa-116">See [Enforce HTTPS in ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) for instructions for specific operating systems.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="663aa-117">Agregar paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="663aa-117">Add NuGet packages</span></span>

<span data-ttu-id="663aa-118">Antes de continuar, instale algunos paquetes NuGet adicionales que usará más adelante.</span><span class="sxs-lookup"><span data-stu-id="663aa-118">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="663aa-119">[Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) para autenticar y solicitar tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="663aa-119">[Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) for authenticating and requesting access tokens.</span></span>
- <span data-ttu-id="663aa-120">[Microsoft. Identity. Web. MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) para agregar compatibilidad con Microsoft Graph configurada con Microsoft. Identity. Web.</span><span class="sxs-lookup"><span data-stu-id="663aa-120">[Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) for adding Microsoft Graph support configured with Microsoft.Identity.Web.</span></span>
- <span data-ttu-id="663aa-121">[Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) para actualizar la versión de este paquete instalada por Microsoft. Identity. Web. MicrosoftGraph.</span><span class="sxs-lookup"><span data-stu-id="663aa-121">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) to update the version of this package installed by Microsoft.Identity.Web.MicrosoftGraph.</span></span>
- <span data-ttu-id="663aa-122">[Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) para habilitar Newtonsoft convertidores de JSON para que sean compatibles con el SDK de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="663aa-122">[Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) to enable Newtonsoft JSON converters for compatibility with the Microsoft Graph SDK.</span></span>
- <span data-ttu-id="663aa-123">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) para traducir los identificadores de zona horaria de Windows a identificadores de IANA.</span><span class="sxs-lookup"><span data-stu-id="663aa-123">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

1. <span data-ttu-id="663aa-124">Ejecute los siguientes comandos en su CLI para instalar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="663aa-124">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package Microsoft.Identity.Web --version 1.1.0
    dotnet add package Microsoft.Identity.Web.MicrosoftGraph --version 1.1.0
    dotnet add package Microsoft.Graph --version 3.16.0
    dotnet add package Microsoft.AspNetCore.Mvc.NewtonsoftJson
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a><span data-ttu-id="663aa-125">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="663aa-125">Design the app</span></span>

<span data-ttu-id="663aa-126">En esta sección, creará la estructura básica de la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="663aa-126">In this section you will create the basic UI structure of the application.</span></span>

> [!TIP]
> <span data-ttu-id="663aa-127">Puede usar cualquier editor de texto para editar los archivos de origen de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="663aa-127">You can use any text editor to edit the source files for this tutorial.</span></span> <span data-ttu-id="663aa-128">Sin embargo, [Visual Studio Code](https://code.visualstudio.com/) proporciona características adicionales, como la depuración e IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="663aa-128">However, [Visual Studio Code](https://code.visualstudio.com/) provides additional features, such as debugging and Intellisense.</span></span>

1. <span data-ttu-id="663aa-129">Abra **./Pages/Shared/_Layout. cshtml** y reemplace todo el contenido por el código siguiente para actualizar el diseño global de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="663aa-129">Open **./Pages/Shared/_Layout.cshtml** and replace its entire contents with the following code to update the global layout of the app.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Pages/Shared/_Layout.cshtml" id="LayoutSnippet":::

    <span data-ttu-id="663aa-130">Esto reemplaza el bootstrap con la [interfaz de usuario fluida](https://developer.microsoft.com/fluentui), agrega el [SDK de Microsoft Teams](/javascript/api/overview/msteams-client)y simplifica el diseño.</span><span class="sxs-lookup"><span data-stu-id="663aa-130">This replaces Bootstrap with [Fluent UI](https://developer.microsoft.com/fluentui), adds the [Microsoft Teams SDK](/javascript/api/overview/msteams-client), and simplifies the layout.</span></span>

1. <span data-ttu-id="663aa-131">Abra el **site.js./wwwroot/JS/** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="663aa-131">Open **./wwwroot/js/site.js** and add the following code.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/wwwroot/js/site.js" id="ThemeSupportSnippet":::

    <span data-ttu-id="663aa-132">Esto agrega un controlador sencillo de cambios de tema para cambiar el color de texto predeterminado para los temas de contraste oscuro y alto.</span><span class="sxs-lookup"><span data-stu-id="663aa-132">This adds a simple theme change handler to change the default text color for dark and high contrast themes.</span></span>

1. <span data-ttu-id="663aa-133">Abra **./wwwroot/CSS/site.CSS** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="663aa-133">Open **./wwwroot/css/site.css** and replace its contents with the following.</span></span>

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/site.css" id="CssSnippet":::

1. <span data-ttu-id="663aa-134">Abra **./pages/index.cshtml** y reemplace su contenido por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="663aa-134">Open **./Pages/Index.cshtml** and replace its contents with the following code.</span></span>

    ```cshtml
    @page
    @model IndexModel
    @{
      ViewData["Title"] = "Home page";
    }

    <div id="tab-container">
      <h1 class="ms-fontSize-24 ms-fontWeight-semibold">Loading...</h1>
    </div>

    @section Scripts
    {
      <script>
      </script>
    }
    ```

1. <span data-ttu-id="663aa-135">Abra **./startup.CS** y quite la `app.UseHttpsRedirection();` línea del `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="663aa-135">Open **./Startup.cs** and remove the `app.UseHttpsRedirection();` line in the `Configure` method.</span></span> <span data-ttu-id="663aa-136">Esto es necesario para que funcione el túnel de ngrok.</span><span class="sxs-lookup"><span data-stu-id="663aa-136">This is necessary for ngrok tunneling to work.</span></span>

## <a name="run-ngrok"></a><span data-ttu-id="663aa-137">Ejecutar ngrok</span><span class="sxs-lookup"><span data-stu-id="663aa-137">Run ngrok</span></span>

<span data-ttu-id="663aa-138">Microsoft Teams no es compatible con el hospedaje local de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="663aa-138">Microsoft Teams does not support local hosting for apps.</span></span> <span data-ttu-id="663aa-139">El servidor que hospeda la aplicación debe estar disponible en la nube con puntos de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="663aa-139">The server hosting your app must be available from the cloud using HTTPS endpoints.</span></span> <span data-ttu-id="663aa-140">Para la depuración local, puede usar ngrok para crear una dirección URL pública para el proyecto hospedado localmente.</span><span class="sxs-lookup"><span data-stu-id="663aa-140">For debugging locally, you can use ngrok to create a public URL for your locally-hosted project.</span></span>

1. <span data-ttu-id="663aa-141">Abra la CLI y ejecute el siguiente comando para iniciar ngrok.</span><span class="sxs-lookup"><span data-stu-id="663aa-141">Open your CLI and run the following command to start ngrok.</span></span>

    ```Shell
    ngrok http 5000
    ```

1. <span data-ttu-id="663aa-142">Una vez que se inicie ngrok, copie la dirección URL de reenvío HTTPS.</span><span class="sxs-lookup"><span data-stu-id="663aa-142">Once ngrok starts, copy the HTTPS Forwarding URL.</span></span> <span data-ttu-id="663aa-143">Debe ser similar a `https://50153897dd4d.ngrok.io` .</span><span class="sxs-lookup"><span data-stu-id="663aa-143">It should look like `https://50153897dd4d.ngrok.io`.</span></span> <span data-ttu-id="663aa-144">Necesitará este valor en pasos posteriores.</span><span class="sxs-lookup"><span data-stu-id="663aa-144">You'll need this value in later steps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="663aa-145">Si usa la versión gratuita de ngrok, la dirección URL de reenvío cambia cada vez que reinicia ngrok.</span><span class="sxs-lookup"><span data-stu-id="663aa-145">If you are using the free version of ngrok, the forwarding URL changes every time you restart ngrok.</span></span> <span data-ttu-id="663aa-146">Se recomienda dejar ngrok en ejecución hasta que complete este tutorial para mantener la misma dirección URL.</span><span class="sxs-lookup"><span data-stu-id="663aa-146">It's recommended that you leave ngrok running until you complete this tutorial to keep the same URL.</span></span> <span data-ttu-id="663aa-147">Si tiene que reiniciar ngrok, tendrá que actualizar la dirección URL en todos los lugares en los que se use y reinstalar la aplicación en Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="663aa-147">If you have to restart ngrok, you'll need to update your URL everywhere that it is used and reinstall the app in Microsoft Teams.</span></span>
