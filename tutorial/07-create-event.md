<!-- markdownlint-disable MD002 MD041 -->

En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.

## <a name="create-the-new-event-tab"></a>Crear la nueva pestaña de eventos

1. Cree un nuevo archivo en el directorio **./páginas** denominado **NewEvent. cshtml** y agregue el código siguiente.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.cshtml":::

    Esto implementa un formulario sencillo y agrega JavaScript para enviar los datos del formulario a la API Web.

## <a name="implement-the-web-api"></a>Implementar la API Web

1. Cree un nuevo directorio denominado **Models** en la raíz del proyecto.

1. Cree un archivo nuevo en el directorio **./Models** denominado **NewEvent.CS** y agregue el siguiente código.

    :::code language="csharp" source="../demo/GraphTutorial/Models/NewEvent.cs" id="NewEventSnippet":::

1. Abra **./Controllers/CalendarController.CS** y agregue la siguiente `using` instrucción en la parte superior del archivo.

    ```csharp
    using GraphTutorial.Models;
    ```

1. Agregue la siguiente función a la clase **CalendarController** .

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="PostSnippet":::

    Esto permite que HTTP POST a la API Web con los campos del formulario.

1. Guarde todos los cambios y reinicie la aplicación. Actualice la aplicación en Microsoft Teams y seleccione la pestaña **crear evento** . Rellene el formulario y seleccione **crear** para agregar un evento al calendario del usuario.

    ![Captura de pantalla de la pestaña evento de creación](images/create-event.png)
