<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, creará un nuevo registro de aplicaciones Web de Azure AD con el centro de administración de Azure Active Directory.

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com). Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.

    ![Una captura de pantalla de los registros de la aplicación ](./images/aad-portal-app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **registrar una aplicación** , establezca los valores como sigue, donde `YOUR_NGROK_URL` es la dirección URL de reenvío ngrok que copió en la sección anterior.

    - Establezca **Nombre** como `Teams Graph Tutorial`.
    - Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.
    - En **URI de redirección**, establezca la primera lista desplegable en `Web` y establezca el valor `YOUR_NGROK_URL/authcomplete`.

    ![Captura de pantalla de la página registrar una aplicación](./images/aad-register-an-app.png)

1. Seleccione **Registrar**. En la página Tutorial de Microsoft **Graph** , copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. Seleccione **Autenticación** en **Administrar**. Busque la sección **concesión implícita** y habilite **tokens de acceso** y **tokens de identificador**. Seleccione **Guardar**.

1. Seleccione **Certificados y secretos** en **Administrar**. Seleccione el botón **Nuevo secreto de cliente**. Escriba un valor en **Descripción** y seleccione una de las opciones para **Expires** y seleccione **Agregar**.

1. Copie el valor del secreto de cliente antes de salir de esta página. Lo necesitará en el siguiente paso.

    > [!IMPORTANT]
    > El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.

1. Seleccione **permisos de API** en **administrar** y, a continuación, seleccione **Agregar un permiso**.

1. Seleccione **Microsoft Graph** y, a continuación, **permisos delegados**.

1. Seleccione los siguientes permisos y, a continuación, seleccione **Agregar permisos**.

    - **Calendars. ReadWrite** : permite que la aplicación Lea y escriba en el calendario del usuario.
    - **MailboxSettings. Read** -this permitirá que la aplicación obtenga la zona horaria, el formato de fecha y el formato de hora del usuario de la configuración del buzón.

    ![Una captura de pantalla de los permisos configurados](images/aad-configured-permissions.png)

## <a name="configure-teams-single-sign-on"></a>Configurar el inicio de sesión único de Teams

En esta sección, actualizará el registro de la aplicación para que admita el [Inicio de sesión único en Microsoft Teams](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso).

1. Seleccione **exponer una API**. Seleccione el vínculo **establecer** junto a **URI de identificador de aplicación**. Inserte el nombre de dominio de la dirección URL de reenvío de ngrok (con una barra diagonal "/" anexada al final) entre las dos barras diagonales hacia delante y el GUID. El identificador completo debe tener un aspecto similar a: `api://50153897dd4d.ngrok.io/ae7d8088-3422-4c8c-a351-6ded0f21d615` .

1. En la sección **ámbitos definidos por esta API** , seleccione **Agregar un ámbito**. Rellene los campos como se indica a continuación y seleccione **Agregar ámbito**.

    - **Nombre de ámbito:**`access_as_user`
    - **¿Quién puede dar su consentimiento?: administradores y usuarios**
    - **Nombre para mostrar de consentimiento de administración:**`Access the app as the user`
    - **Descripción del consentimiento del administrador:**`Allows Teams to call the app's web APIs as the current user.`
    - **Nombre para mostrar del consentimiento del usuario:**`Access the app as you`
    - **Descripción del consentimiento del usuario:**`Allows Teams to call the app's web APIs as you.`
    - **Estado: habilitado**

    ![Captura de pantalla del formulario para agregar un ámbito](images/aad-add-scope.png)

1. En la sección **aplicaciones cliente autorizadas** , seleccione **Agregar una aplicación cliente**. Escriba un identificador de cliente de la siguiente lista, habilite el ámbito en **ámbitos autorizados** y seleccione **Agregar aplicación**. Repita este proceso para cada uno de los identificadores de cliente de la lista.

    - `1fec8e78-bce4-4aaf-ab1b-5451cc387264` (Aplicación para equipos móviles o de escritorio)
    - `5e3ce6c0-2b1f-4285-8d4b-75ee78787346` (Aplicación Web de Teams)
