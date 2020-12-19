<!-- markdownlint-disable MD002 MD041 -->

El manifiesto de la aplicación describe cómo se integra la aplicación con Microsoft Teams y es necesario para instalar aplicaciones. En esta sección, usará App Studio en el cliente de Microsoft Teams para generar un manifiesto.

1. Si aún no tiene instalado App Studio en Teams, [instálelo ahora](/microsoftteams/platform/concepts/build-and-test/app-studio-overview).

1. Inicie App Studio en Microsoft Teams y seleccione el **Editor de manifiestos**.

1. Seleccione **crear una nueva aplicación**.

    ![Captura de pantalla del editor de manifiestos en App Studio en Microsoft Teams](images/app-studio-01.png)

1. En la página de detalles de la **aplicación** , rellene los campos obligatorios.

    > [!NOTE]
    > Puede usar los iconos predeterminados en la sección de **Personalización de marca** o cargar los suyos propios.

1. En el menú de la izquierda, seleccione **pestañas** en **capacidades**.

1. Seleccione **Agregar** en **Agregar una ficha personal**.

    ![Una captura de pantalla de la página de pestañas en App Studio](images/app-studio-02.png)

1. Rellene los campos como se indica a continuación, donde `YOUR_NGROK_URL` es la dirección URL de reenvío que ha copiado en la sección anterior. Seleccione **Guardar** cuando haya terminado.

    - **Name:**`Create event`
    - **Identificador de entidad:**`createEventTab`
    - **Dirección URL del contenido:**`YOUR_NGROK_URL/newevent`

1. Seleccione **Agregar** en **Agregar una ficha personal**.

1. Rellene los campos como se indica a continuación, donde `YOUR_NGROK_URL` es la dirección URL de reenvío que ha copiado en la sección anterior. Seleccione **Guardar** cuando haya terminado.

    - **Name:**`Graph calendar`
    - **Identificador de entidad:**`calendarTab`
    - **Dirección URL del contenido:**`YOUR_NGROK_URL`

1. En el menú de la izquierda, seleccione **dominios y permisos** en **Finalizar**.

1. Establezca el **identificador** de la aplicación AAD en el identificador de la aplicación desde el registro de la aplicación.

1. Establezca el campo de **Inicio de sesión único** en el URI del identificador de la aplicación desde el registro de la aplicación.

1. En el menú de la izquierda, seleccione **probar y distribuir** en **Finalizar**. Seleccione **Descargar**.

1. Cree un nuevo directorio en la raíz del proyecto denominado **manifest**. Extraiga el contenido del archivo ZIP descargado en este directorio.
