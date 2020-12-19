<!-- markdownlint-disable MD002 MD041 -->

Las aplicaciones de la pestaña de Microsoft Teams tienen varias opciones para autenticar al usuario y llamar a Microsoft Graph. En este ejercicio, implementará una pestaña que realiza un [Inicio de sesión único](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) para obtener un token de autenticación en el cliente y, a continuación, usa el [flujo "en nombre de](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) " en el servidor para intercambiar ese token para obtener acceso a Microsoft Graph.

Para otras alternativas, vea lo siguiente.

- [Cree una pestaña de Microsoft Teams con el kit de herramientas de Microsoft Graph](/graph/toolkit/get-started/build-a-microsoft-teams-tab). Este ejemplo es totalmente del lado cliente y usa el kit de herramientas de Microsoft Graph para controlar la autenticación y realizar llamadas a Microsoft Graph.
- [Ejemplo de autenticación de Microsoft Teams](https://github.com/OfficeDev/microsoft-teams-sample-auth-node). Este ejemplo contiene varios ejemplos que cubren distintos escenarios de autenticación.

## <a name="create-the-project"></a>Cree el proyecto

Empiece por crear una aplicación Web de ASP.NET Core.

1. Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto. Ejecute el comando siguiente.

    ```Shell
    dotnet new webapp -o GraphTutorial
    ```

1. Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.

    ```Shell
    dotnet run
    ```

1. Abra el explorador y vaya a `https://localhost:5001` . Si todo funciona, debería ver una página de núcleo de ASP.NET predeterminada.

> [!IMPORTANT]
> Si recibe una advertencia de que el certificado para **localhost** no es de confianza, puede usar .net Core CLI para instalar y confiar en el certificado de desarrollo. Consulte [forzar HTTPS en ASP.net Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) para obtener instrucciones sobre sistemas operativos específicos.

## <a name="add-nuget-packages"></a>Agregar paquetes NuGet

Antes de continuar, instale algunos paquetes NuGet adicionales que usará más adelante.

- [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) para autenticar y solicitar tokens de acceso.
- [Microsoft. Identity. Web. MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) para agregar compatibilidad con Microsoft Graph configurada con Microsoft. Identity. Web.
- [Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) para actualizar la versión de este paquete instalada por Microsoft. Identity. Web. MicrosoftGraph.
- [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) para habilitar Newtonsoft convertidores de JSON para que sean compatibles con el SDK de Microsoft Graph.
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) para traducir los identificadores de zona horaria de Windows a identificadores de IANA.

1. Ejecute los siguientes comandos en su CLI para instalar las dependencias.

    ```Shell
    dotnet add package Microsoft.Identity.Web --version 1.1.0
    dotnet add package Microsoft.Identity.Web.MicrosoftGraph --version 1.1.0
    dotnet add package Microsoft.Graph --version 3.16.0
    dotnet add package Microsoft.AspNetCore.Mvc.NewtonsoftJson
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección, creará la estructura básica de la interfaz de usuario de la aplicación.

> [!TIP]
> Puede usar cualquier editor de texto para editar los archivos de origen de este tutorial. Sin embargo, [Visual Studio Code](https://code.visualstudio.com/) proporciona características adicionales, como la depuración e IntelliSense.

1. Abra **./Pages/Shared/_Layout. cshtml** y reemplace todo el contenido por el código siguiente para actualizar el diseño global de la aplicación.

    :::code language="cshtml" source="../demo/GraphTutorial/Pages/Shared/_Layout.cshtml" id="LayoutSnippet":::

    Esto reemplaza el bootstrap con la [interfaz de usuario fluida](https://developer.microsoft.com/fluentui), agrega el [SDK de Microsoft Teams](/javascript/api/overview/msteams-client)y simplifica el diseño.

1. Abra el **site.js./wwwroot/JS/** y agregue el siguiente código.

    :::code language="javascript" source="../demo/GraphTutorial/wwwroot/js/site.js" id="ThemeSupportSnippet":::

    Esto agrega un controlador sencillo de cambios de tema para cambiar el color de texto predeterminado para los temas de contraste oscuro y alto.

1. Abra **./wwwroot/CSS/site.CSS** y reemplace su contenido por lo siguiente.

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/site.css" id="CssSnippet":::

1. Abra **./pages/index.cshtml** y reemplace su contenido por el código siguiente.

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

1. Abra **./startup.CS** y quite la `app.UseHttpsRedirection();` línea del `Configure` método. Esto es necesario para que funcione el túnel de ngrok.

## <a name="run-ngrok"></a>Ejecutar ngrok

Microsoft Teams no es compatible con el hospedaje local de aplicaciones. El servidor que hospeda la aplicación debe estar disponible en la nube con puntos de conexión HTTPS. Para la depuración local, puede usar ngrok para crear una dirección URL pública para el proyecto hospedado localmente.

1. Abra la CLI y ejecute el siguiente comando para iniciar ngrok.

    ```Shell
    ngrok http 5000
    ```

1. Una vez que se inicie ngrok, copie la dirección URL de reenvío HTTPS. Debe ser similar a `https://50153897dd4d.ngrok.io` . Necesitará este valor en pasos posteriores.

> [!IMPORTANT]
> Si usa la versión gratuita de ngrok, la dirección URL de reenvío cambia cada vez que reinicia ngrok. Se recomienda dejar ngrok en ejecución hasta que complete este tutorial para mantener la misma dirección URL. Si tiene que reiniciar ngrok, tendrá que actualizar la dirección URL en todos los lugares en los que se use y reinstalar la aplicación en Microsoft Teams.
