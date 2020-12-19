<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="40be6-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para que admita la autenticación de inicio de sesión único con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="40be6-101">In this exercise you will extend the application from the previous exercise to support single sign-on authentication with Azure AD.</span></span> <span data-ttu-id="40be6-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a la API de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="40be6-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph API.</span></span> <span data-ttu-id="40be6-103">En este paso, configurará la biblioteca [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) .</span><span class="sxs-lookup"><span data-stu-id="40be6-103">In this step you will configure the [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) library.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40be6-104">Para evitar almacenar el identificador de aplicación y el secreto en el origen, deberá usar el [Administrador de secretos de .net](/aspnet/core/security/app-secrets) para almacenar estos valores.</span><span class="sxs-lookup"><span data-stu-id="40be6-104">To avoid storing the application ID and secret in source, you will use the [.NET Secret Manager](/aspnet/core/security/app-secrets) to store these values.</span></span> <span data-ttu-id="40be6-105">El administrador de secretos solo se usa con fines de desarrollo, las aplicaciones de producción deben usar un administrador de secretos de confianza para almacenar secretos.</span><span class="sxs-lookup"><span data-stu-id="40be6-105">The Secret Manager is for development purposes only, production apps should use a trusted secret manager for storing secrets.</span></span>

1. <span data-ttu-id="40be6-106">Abra **./appsettings.jsen** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="40be6-106">Open **./appsettings.json** and replace its contents with the following.</span></span>

    :::code language="json" source="../demo/GraphTutorial/appsettings.example.json" highlight="2-8":::

1. <span data-ttu-id="40be6-107">Abra su CLI en el directorio donde se encuentra **GraphTutorial. csproj** y ejecute los siguientes comandos, sustituyendo `YOUR_APP_ID` por el identificador de la aplicación del portal de Azure y `YOUR_APP_SECRET` por el secreto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40be6-107">Open your CLI in the directory where **GraphTutorial.csproj** is located, and run the following commands, substituting `YOUR_APP_ID` with your application ID from the Azure portal, and `YOUR_APP_SECRET` with your application secret.</span></span>

    ```Shell
    dotnet user-secrets init
    dotnet user-secrets set "AzureAd:ClientId" "YOUR_APP_ID"
    dotnet user-secrets set "AzureAd:ClientSecret" "YOUR_APP_SECRET"
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="40be6-108">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="40be6-108">Implement sign-in</span></span>

<span data-ttu-id="40be6-109">En primer lugar, implemente el inicio de sesión único en el código JavaScript de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40be6-109">First, implement single sign-on in the app's JavaScript code.</span></span> <span data-ttu-id="40be6-110">Usará el SDK de [JavaScript de Microsoft Teams](/javascript/api/overview/msteams-client) para obtener un token de acceso que permita que el código JavaScript que se ejecuta en el cliente de Microsoft Teams realice llamadas Ajax a la API Web que se va a implementar más adelante.</span><span class="sxs-lookup"><span data-stu-id="40be6-110">You will use the [Microsoft Teams JavaScript SDK](/javascript/api/overview/msteams-client) to get an access token which allows the JavaScript code running in the Teams client to make AJAX calls to Web API you will implement later.</span></span>

1. <span data-ttu-id="40be6-111">Abra **./pages/index.cshtml** y agregue el siguiente código dentro de la `<script>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="40be6-111">Open **./Pages/Index.cshtml** and add the following code inside the `<script>` tag.</span></span>

    ```javascript
    (function () {
      if (microsoftTeams) {
        microsoftTeams.initialize();

        microsoftTeams.authentication.getAuthToken({
          successCallback: (token) => {
            // TEMPORARY: Display the access token for debugging
            $('#tab-container').empty();

            $('<code/>', {
              text: token,
              style: 'word-break: break-all;'
            }).appendTo('#tab-container');
          },
          failureCallback: (error) => {
            renderError(error);
          }
        });
      }
    })();

    function renderError(error) {
      $('#tab-container').empty();

      $('<h1/>', {
        text: 'Error'
      }).appendTo('#tab-container');

      $('<code/>', {
        text: JSON.stringify(error, Object.getOwnPropertyNames(error)),
        style: 'word-break: break-all;'
      }).appendTo('#tab-container');
    }
    ```

    <span data-ttu-id="40be6-112">Esto llama al `microsoftTeams.authentication.getAuthToken` para autenticarse silenciosamente como el usuario que ha iniciado sesión en Teams.</span><span class="sxs-lookup"><span data-stu-id="40be6-112">This calls the `microsoftTeams.authentication.getAuthToken` to silently authenticate as the user that is signed in to Teams.</span></span> <span data-ttu-id="40be6-113">Por lo general, no hay ningún mensaje de interfaz de usuario implicado, a menos que el usuario tenga que dar su consentimiento.</span><span class="sxs-lookup"><span data-stu-id="40be6-113">There is typically not any UI prompts involved, unless the user has to consent.</span></span> <span data-ttu-id="40be6-114">A continuación, el código muestra el token en la ficha.</span><span class="sxs-lookup"><span data-stu-id="40be6-114">The code then displays the token in the tab.</span></span>

1. <span data-ttu-id="40be6-115">Guarde los cambios e inicie la aplicación ejecutando el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="40be6-115">Save your changes and start your application by running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="40be6-116">Si ha reiniciado ngrok y la dirección URL de ngrok ha cambiado, asegúrese de actualizar el valor de ngrok en el siguiente punto **antes** de realizar la prueba.</span><span class="sxs-lookup"><span data-stu-id="40be6-116">If you have restarted ngrok and your ngrok URL has changed, be sure to update the ngrok value in the following place **before** you test.</span></span>
    >
    > - <span data-ttu-id="40be6-117">El URI de redireccionamiento en el registro de la aplicación</span><span class="sxs-lookup"><span data-stu-id="40be6-117">The redirect URI in your app registration</span></span>
    > - <span data-ttu-id="40be6-118">El URI del identificador de aplicación en el registro de la aplicación</span><span class="sxs-lookup"><span data-stu-id="40be6-118">The application ID URI in your app registration</span></span>
    > - <span data-ttu-id="40be6-119">`contentUrl` en manifest.jsen</span><span class="sxs-lookup"><span data-stu-id="40be6-119">`contentUrl` in manifest.json</span></span>
    > - <span data-ttu-id="40be6-120">`validDomains` en manifest.jsen</span><span class="sxs-lookup"><span data-stu-id="40be6-120">`validDomains` in manifest.json</span></span>
    > - <span data-ttu-id="40be6-121">`resource` en manifest.jsen</span><span class="sxs-lookup"><span data-stu-id="40be6-121">`resource` in manifest.json</span></span>

1. <span data-ttu-id="40be6-122">Cree un archivo ZIP con **manifest.jsen**, **color.png** y **outline.png**.</span><span class="sxs-lookup"><span data-stu-id="40be6-122">Create a ZIP file with **manifest.json**, **color.png**, and **outline.png**.</span></span>

1. <span data-ttu-id="40be6-123">En Microsoft Teams, seleccione **aplicaciones** en la barra de la izquierda, seleccione **cargar una aplicación personalizada** y, a continuación, seleccione **subir a mí o a mis equipos**.</span><span class="sxs-lookup"><span data-stu-id="40be6-123">In Microsoft Teams, select **Apps** in the left-hand bar, select **Upload a custom app**, then select **Upload for me or my teams**.</span></span>

    ![Captura de pantalla del vínculo cargar una aplicación personalizada en Microsoft Teams](images/upload-custom-app.png)

1. <span data-ttu-id="40be6-125">Vaya al archivo ZIP que ha creado previamente y seleccione **abrir**.</span><span class="sxs-lookup"><span data-stu-id="40be6-125">Browse to the ZIP file you created previously and select **Open**.</span></span>

1. <span data-ttu-id="40be6-126">Revise la información de la aplicación y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="40be6-126">Review the application information and select **Add**.</span></span>

1. <span data-ttu-id="40be6-127">La aplicación se abre en Microsoft Teams y muestra un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="40be6-127">The application opens in Teams and displays an access token.</span></span>

<span data-ttu-id="40be6-128">Si copia el token, puede pegarlo en [JWT.ms](https://jwt.ms).</span><span class="sxs-lookup"><span data-stu-id="40be6-128">If you copy the token, you can paste it into [jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="40be6-129">Compruebe que la audiencia (la `aud` notificación) es el identificador de la aplicación y que el único ámbito (la `scp` notificación) es el ámbito de la `access_as_user` API que ha creado.</span><span class="sxs-lookup"><span data-stu-id="40be6-129">Verify that the audience (the `aud` claim) is your application ID, and the only scope (the `scp` claim) is the `access_as_user` API scope you created.</span></span> <span data-ttu-id="40be6-130">Esto significa que este token no concede acceso directo a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="40be6-130">That means that this token does not grant direct access to Microsoft Graph!</span></span> <span data-ttu-id="40be6-131">En su lugar, la API Web que se va a implementar pronto tendrá que intercambiar este token mediante el [flujo en nombre de](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) para obtener un token que funcione con llamadas de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="40be6-131">Instead, the Web API you will implement soon will need to exchange this token using the [on-behalf-of flow](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) to get a token that will work with Microsoft Graph calls.</span></span>

## <a name="configure-authentication-in-the-aspnet-core-app"></a><span data-ttu-id="40be6-132">Configuración de la autenticación en la aplicación principal de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40be6-132">Configure authentication in the ASP.NET Core app</span></span>

<span data-ttu-id="40be6-133">Empiece agregando Microsoft Identity Platform Services a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40be6-133">Start by adding the Microsoft Identity platform services to the application.</span></span>

1. <span data-ttu-id="40be6-134">Abra el archivo **./startup.CS** y agregue la siguiente `using` instrucción en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="40be6-134">Open the **./Startup.cs** file and add the following `using` statement to the top of the file.</span></span>

    ```csharp
    using Microsoft.Identity.Web;
    ```

1. <span data-ttu-id="40be6-135">Agregue la siguiente línea justo antes de la `app.UseAuthorization();` línea en la `Configure` función.</span><span class="sxs-lookup"><span data-stu-id="40be6-135">Add the following line just before the `app.UseAuthorization();` line in the `Configure` function.</span></span>

    ```csharp
    app.UseAuthentication();
    ```

1. <span data-ttu-id="40be6-136">Agregue la siguiente línea justo después `endpoints.MapRazorPages();` de la línea en la `Configure` función.</span><span class="sxs-lookup"><span data-stu-id="40be6-136">Add the following line just after the `endpoints.MapRazorPages();` line in the `Configure` function.</span></span>

    ```csharp
    endpoints.MapControllers();
    ```

1. <span data-ttu-id="40be6-137">Reemplace la función `ConfigureServices` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="40be6-137">Replace the existing `ConfigureServices` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Startup.cs" id="ConfigureServicesSnippet":::

    <span data-ttu-id="40be6-138">Este código configura la aplicación para permitir que las llamadas a las API Web se autentiquen en función del token de portador de JWT en el `Authorization` encabezado.</span><span class="sxs-lookup"><span data-stu-id="40be6-138">This code configures the application to allow calls to Web APIs to be authenticated based on the JWT bearer token in the `Authorization` header.</span></span> <span data-ttu-id="40be6-139">También agrega los servicios de adquisición de tokens que pueden intercambiar ese token mediante el flujo en nombre de.</span><span class="sxs-lookup"><span data-stu-id="40be6-139">It also adds the token acquisition services that can exchange that token via the on-behalf-of flow.</span></span>

## <a name="create-the-web-api-controller"></a><span data-ttu-id="40be6-140">Crear el controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="40be6-140">Create the Web API controller</span></span>

1. <span data-ttu-id="40be6-141">Cree un nuevo directorio en la raíz del proyecto denominado **Controllers**.</span><span class="sxs-lookup"><span data-stu-id="40be6-141">Create a new directory in the root of the project named **Controllers**.</span></span>

1. <span data-ttu-id="40be6-142">Cree un archivo nuevo en el directorio **./Controllers** denominado **CalendarController.CS** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="40be6-142">Create a new file in the **./Controllers** directory named **CalendarController.cs** and add the following code.</span></span>

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Authorization;
    using Microsoft.AspNetCore.Http;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;
    using Microsoft.Identity.Web;
    using Microsoft.Identity.Web.Resource;
    using Microsoft.Graph;
    using TimeZoneConverter;

    namespace GraphTutorial.Controllers
    {
        [ApiController]
        [Route("[controller]")]
        [Authorize]
        public class CalendarController : ControllerBase
        {
            private static readonly string[] apiScopes = new[] { "access_as_user" };

            private readonly GraphServiceClient _graphClient;
            private readonly ITokenAcquisition _tokenAcquisition;
            private readonly ILogger<CalendarController> _logger;

            public CalendarController(ITokenAcquisition tokenAcquisition, GraphServiceClient graphClient, ILogger<CalendarController> logger)
            {
                _tokenAcquisition = tokenAcquisition;
                _graphClient = graphClient;
                _logger = logger;
            }

            [HttpGet]
            public async Task<string> Get()
            {
                // This verifies that the access_as_user scope is
                // present in the bearer token, throws if not
                HttpContext.VerifyUserHasAnyAcceptedScope(apiScopes);

                // To verify that the identity libraries have authenticated
                // based on the token, log the user's name
                _logger.LogInformation($"Authenticated user: {User.GetDisplayName()}");

                try
                {
                    // TEMPORARY
                    // Get a Graph token via OBO flow
                    var token = await _tokenAcquisition
                        .GetAccessTokenForUserAsync(new[]{
                            "User.Read",
                            "MailboxSettings.Read",
                            "Calendars.ReadWrite" });

                    // Log the token
                    _logger.LogInformation($"Access token for Graph: {token}");
                    return "{ \"status\": \"OK\" }";
                }
                catch (MicrosoftIdentityWebChallengeUserException ex)
                {
                    _logger.LogError(ex, "Consent required");
                    // This exception indicates consent is required.
                    // Return a 403 with "consent_required" in the body
                    // to signal to the tab it needs to prompt for consent
                    HttpContext.Response.ContentType = "text/plain";
                    HttpContext.Response.StatusCode = (int)HttpStatusCode.Forbidden;
                    await HttpContext.Response.WriteAsync("consent_required");
                    return null;
                }
                catch (Exception ex)
                {
                    _logger.LogError(ex, "Error occurred");
                    return null;
                }
            }
        }
    }
    ```

    <span data-ttu-id="40be6-143">Esto implementa una API Web ( `GET /calendar` ) a la que se puede llamar desde la pestaña Microsoft Teams. Por ahora, simplemente intenta intercambiar el token de portador para un token de gráfico.</span><span class="sxs-lookup"><span data-stu-id="40be6-143">This implements a Web API (`GET /calendar`) that can be called from the Teams tab. For now it simply tries to exchange the bearer token for a Graph token.</span></span> <span data-ttu-id="40be6-144">La primera vez que un usuario carga la pestaña, se producirá un error porque todavía no han dado su consentimiento para permitir que la aplicación tenga acceso a Microsoft Graph en su nombre.</span><span class="sxs-lookup"><span data-stu-id="40be6-144">The first time a user loads the tab, this will fail because they have not yet consented to allow the app access to Microsoft Graph on their behalf.</span></span>

1. <span data-ttu-id="40be6-145">Abra **./pages/index.cshtml** y reemplace la `successCallback` función por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="40be6-145">Open **./Pages/Index.cshtml** and replace the `successCallback` function with the following.</span></span>

    ```javascript
    successCallback: (token) => {
      // TEMPORARY: Call the Web API
      fetch('/calendar', {
        headers: {
          'Authorization': `Bearer ${token}`
        }
      }).then(response => {
        response.text()
          .then(body => {
            $('#tab-container').empty();
            $('<code/>', {
              text: body
            }).appendTo('#tab-container');
          });
      }).catch(error => {
        console.error(error);
        renderError(error);
      });
    }
    ```

    <span data-ttu-id="40be6-146">Se llamará a la API Web y se mostrará la respuesta.</span><span class="sxs-lookup"><span data-stu-id="40be6-146">This will call the Web API and display the response.</span></span>

1. <span data-ttu-id="40be6-147">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40be6-147">Save your changes and restart the app.</span></span> <span data-ttu-id="40be6-148">Actualice la pestaña en Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="40be6-148">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="40be6-149">Debe mostrarse la página `consent_required` .</span><span class="sxs-lookup"><span data-stu-id="40be6-149">The page should display `consent_required`.</span></span>

1. <span data-ttu-id="40be6-150">Revise el resultado del registro en su CLI.</span><span class="sxs-lookup"><span data-stu-id="40be6-150">Review the log output in your CLI.</span></span> <span data-ttu-id="40be6-151">Observe dos cosas.</span><span class="sxs-lookup"><span data-stu-id="40be6-151">Notice two things.</span></span>

    - <span data-ttu-id="40be6-152">Una entrada como `Authenticated user: MeganB@contoso.com` .</span><span class="sxs-lookup"><span data-stu-id="40be6-152">An entry like `Authenticated user: MeganB@contoso.com`.</span></span> <span data-ttu-id="40be6-153">La API Web ha autenticado al usuario en función del token enviado con la solicitud de la API.</span><span class="sxs-lookup"><span data-stu-id="40be6-153">The Web API has authenticated the user based on the token sent with the API request.</span></span>
    - <span data-ttu-id="40be6-154">Una entrada como `AADSTS65001: The user or administrator has not consented to use the application with ID...` .</span><span class="sxs-lookup"><span data-stu-id="40be6-154">An entry like `AADSTS65001: The user or administrator has not consented to use the application with ID...`.</span></span> <span data-ttu-id="40be6-155">Esto es normal, ya que aún no se ha solicitado el consentimiento del usuario para los ámbitos de permisos de Microsoft Graph solicitados.</span><span class="sxs-lookup"><span data-stu-id="40be6-155">This is expected, since the user has not yet been prompted to consent for the requested Microsoft Graph permission scopes.</span></span>

## <a name="implement-consent-prompt"></a><span data-ttu-id="40be6-156">Solicitud de implementación de consentimiento</span><span class="sxs-lookup"><span data-stu-id="40be6-156">Implement consent prompt</span></span>

<span data-ttu-id="40be6-157">Debido a que la API Web no puede preguntar al usuario, la pestaña de Microsoft Teams tendrá que implementar un mensaje.</span><span class="sxs-lookup"><span data-stu-id="40be6-157">Because the Web API cannot prompt the user, the Teams tab will need to implement a prompt.</span></span> <span data-ttu-id="40be6-158">Esto solo tendrá que realizarse una vez para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="40be6-158">This will only need to be done once for each user.</span></span> <span data-ttu-id="40be6-159">Una vez que un usuario ha dado su consentimiento, no tiene que volver a conceder el consentimiento a menos que revoque explícitamente el acceso a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40be6-159">Once a user consents, they do not need to reconsent unless they explicitly revoke access to your application.</span></span>

1. <span data-ttu-id="40be6-160">Cree un archivo nuevo en el directorio **./páginas** denominado **Authenticate.cshtml.CS** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="40be6-160">Create a new file in the **./Pages** directory named **Authenticate.cshtml.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Authenticate.cshtml.cs" id="AuthenticateModelSnippet":::

1. <span data-ttu-id="40be6-161">Cree un nuevo archivo en el directorio **./páginas** denominado **authenticatee. cshtml** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="40be6-161">Create a new file in the **./Pages** directory named **Authenticate.cshtml** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authenticate.cshtml":::

1. <span data-ttu-id="40be6-162">Cree un nuevo archivo en el directorio **./páginas** denominado **AuthComplete. cshtml** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="40be6-162">Create a new file in the **./Pages** directory named **AuthComplete.cshtml** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/AuthComplete.cshtml":::

1. <span data-ttu-id="40be6-163">Abra **./pages/index.cshtml** y agregue las siguientes funciones dentro de la `<script>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="40be6-163">Open **./Pages/Index.cshtml** and add the following functions inside the `<script>` tag.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="LoadUserCalendarSnippet":::

1. <span data-ttu-id="40be6-164">Agregue la siguiente función dentro de la `<script>` etiqueta para mostrar un resultado correcto de la API Web.</span><span class="sxs-lookup"><span data-stu-id="40be6-164">Add the following function inside the `<script>` tag to display a successful result from the Web API.</span></span>

    ```javascript
    function renderCalendar(events) {
      $('#tab-container').empty();

      $('<pre/>').append($('<code/>', {
        text: JSON.stringify(events, null, 2),
        style: 'word-break: break-all;'
      })).appendTo('#tab-container');
    }
    ```

1. <span data-ttu-id="40be6-165">Reemplace el existente `successCallback` por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="40be6-165">Replace the existing `successCallback` with the following code.</span></span>

    ```javascript
    successCallback: (token) => {
      loadUserCalendar(token, (events) => {
        renderCalendar(events);
      });
    }
    ```

1. <span data-ttu-id="40be6-166">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40be6-166">Save your changes and restart the app.</span></span> <span data-ttu-id="40be6-167">Actualice la pestaña en Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="40be6-167">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="40be6-168">La pestaña debe mostrar `{ "status": "OK" }` .</span><span class="sxs-lookup"><span data-stu-id="40be6-168">The tab should display `{ "status": "OK" }`.</span></span>

1. <span data-ttu-id="40be6-169">Revise el resultado del registro.</span><span class="sxs-lookup"><span data-stu-id="40be6-169">Review the log output.</span></span> <span data-ttu-id="40be6-170">Debe ver la `Access token for Graph` entrada.</span><span class="sxs-lookup"><span data-stu-id="40be6-170">You should see the `Access token for Graph` entry.</span></span> <span data-ttu-id="40be6-171">Si analiza ese token, observará que contiene los ámbitos de Microsoft Graph configurados en **appsettings.jsactivado**.</span><span class="sxs-lookup"><span data-stu-id="40be6-171">If you parse that token, you'll notice that it contains the Microsoft Graph scopes configured in **appsettings.json**.</span></span>

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="40be6-172">Almacenamiento y actualización de tokens</span><span class="sxs-lookup"><span data-stu-id="40be6-172">Storing and refreshing tokens</span></span>

<span data-ttu-id="40be6-173">En este punto, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="40be6-173">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="40be6-174">Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="40be6-174">This is the token that allows the app to access Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="40be6-175">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="40be6-175">However, this token is short-lived.</span></span> <span data-ttu-id="40be6-176">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="40be6-176">The token expires an hour after it is issued.</span></span> <span data-ttu-id="40be6-177">Aquí es donde el token de actualización se vuelve útil.</span><span class="sxs-lookup"><span data-stu-id="40be6-177">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="40be6-178">El token de actualización permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.</span><span class="sxs-lookup"><span data-stu-id="40be6-178">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="40be6-179">Debido a que la aplicación usa la biblioteca Microsoft. Identity. Web, no es necesario implementar ninguna lógica de almacenamiento o actualización de tokens.</span><span class="sxs-lookup"><span data-stu-id="40be6-179">Because the app is using the Microsoft.Identity.Web library, you do not have to implement any token storage or refresh logic.</span></span>

<span data-ttu-id="40be6-180">La aplicación usa la caché de token en memoria, que es suficiente para las aplicaciones que no necesitan conservar los tokens cuando se reinicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40be6-180">The app uses the in-memory token cache, which is sufficient for apps that do not need to persist tokens when the app restarts.</span></span> <span data-ttu-id="40be6-181">Las aplicaciones de producción pueden usar en su lugar las [Opciones de caché distribuida](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization) de la biblioteca Microsoft. Identity. Web.</span><span class="sxs-lookup"><span data-stu-id="40be6-181">Production apps may instead use the [distributed cache options](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization) in the Microsoft.Identity.Web library.</span></span>

<span data-ttu-id="40be6-182">El `GetAccessTokenForUserAsync` método controla la expiración y la actualización de los tokens.</span><span class="sxs-lookup"><span data-stu-id="40be6-182">The `GetAccessTokenForUserAsync` method handles token expiration and refresh for you.</span></span> <span data-ttu-id="40be6-183">Primero comprueba el token almacenado en caché y, si no lo ha expirado, lo devuelve.</span><span class="sxs-lookup"><span data-stu-id="40be6-183">It first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="40be6-184">Si ha expirado, usa el token de actualización almacenado en caché para obtener uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="40be6-184">If it is expired, it uses the cached refresh token to obtain a new one.</span></span>

<span data-ttu-id="40be6-185">El **GraphServiceClient** que los controladores obtienen mediante la inyección de dependencia está preconfigurado con un proveedor de autenticación que usa `GetAccessTokenForUserAsync` para usted.</span><span class="sxs-lookup"><span data-stu-id="40be6-185">The **GraphServiceClient** that controllers get via dependency injection is pre-configured with an authentication provider that uses `GetAccessTokenForUserAsync` for you.</span></span>
