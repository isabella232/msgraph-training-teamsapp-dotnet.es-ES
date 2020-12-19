<!-- markdownlint-disable MD002 MD041 -->

En esta sección, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la [biblioteca de cliente de Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.

# <a name="get-a-calendar-view"></a>Obtener una vista de calendario

Una vista de calendario es un conjunto de eventos del calendario del usuario que se producen entre dos puntos de tiempo. Lo usará para obtener los eventos del usuario para la semana actual.

1. Abra **./Controllers/CalendarController.CS** y agregue la siguiente función a la clase **CalendarController** .

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetStartOfWeekSnippet":::

1. Agregue la siguiente función para controlar las excepciones devueltas desde llamadas de Microsoft Graph.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="HandleGraphExceptionSnippet":::

1. Reemplace la función `Get` existente por lo siguiente.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetSnippet" highlight="2,14-57":::

    Revise los cambios. Esta nueva versión de la función:

    - Devuelve `IEnumerable<Event>` en lugar de `string` .
    - Obtiene la configuración del buzón del usuario mediante Microsoft Graph.
    - Usa la zona horaria del usuario para calcular el inicio y el final de la semana actual.
    - Obtiene una vista de calendario
        - Usa la `.Header()` función para incluir un `Prefer: outlook.timezone` encabezado, lo que hace que los eventos devueltos tengan sus horas de inicio y finalización convertidas en la zona horaria del usuario.
        - Usa la `.Top()` función para solicitar como máximo 50 eventos.
        - Usa la `.Select()` función para solicitar solo los campos que usa la aplicación.
        - Usa la `OrderBy()` función para ordenar los resultados por la hora de inicio.

1. Guarde los cambios y reinicie la aplicación. Actualice la pestaña en Microsoft Teams. La aplicación muestra una lista JSON de los eventos.

## <a name="display-the-results"></a>Mostrar los resultados

Ahora puede mostrar la lista de eventos de una forma más sencilla de uso.

1. Abra **./pages/index.cshtml** y agregue las siguientes funciones dentro de la `<script>` etiqueta.

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderHelpersSnippet":::

1. Reemplace la función `renderCalendar` existente por lo siguiente.

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderCalendarSnippet":::

1. Guarde los cambios y reinicie la aplicación. Actualice la pestaña en Microsoft Teams. La aplicación muestra los eventos en el calendario del usuario.

    ![Una captura de pantalla de la aplicación que muestra el calendario del usuario](images/calendar-view.png)
