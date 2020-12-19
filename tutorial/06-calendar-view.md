<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="9e7b8-101">En esta sección, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-101">In this section you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="9e7b8-102">Para esta aplicación, usará la [biblioteca de cliente de Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

# <a name="get-a-calendar-view"></a><span data-ttu-id="9e7b8-103">Obtener una vista de calendario</span><span class="sxs-lookup"><span data-stu-id="9e7b8-103">Get a calendar view</span></span>

<span data-ttu-id="9e7b8-104">Una vista de calendario es un conjunto de eventos del calendario del usuario que se producen entre dos puntos de tiempo.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-104">A calendar view is a set of events from the user's calendar that occur between two points of time.</span></span> <span data-ttu-id="9e7b8-105">Lo usará para obtener los eventos del usuario para la semana actual.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-105">You'll use this to get the user's events for the current week.</span></span>

1. <span data-ttu-id="9e7b8-106">Abra **./Controllers/CalendarController.CS** y agregue la siguiente función a la clase **CalendarController** .</span><span class="sxs-lookup"><span data-stu-id="9e7b8-106">Open **./Controllers/CalendarController.cs** and add the following function to the **CalendarController** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetStartOfWeekSnippet":::

1. <span data-ttu-id="9e7b8-107">Agregue la siguiente función para controlar las excepciones devueltas desde llamadas de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-107">Add the following function to handle exceptions returned from Microsoft Graph calls.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="HandleGraphExceptionSnippet":::

1. <span data-ttu-id="9e7b8-108">Reemplace la función `Get` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-108">Replace the existing `Get` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetSnippet" highlight="2,14-57":::

    <span data-ttu-id="9e7b8-109">Revise los cambios.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-109">Review the changes.</span></span> <span data-ttu-id="9e7b8-110">Esta nueva versión de la función:</span><span class="sxs-lookup"><span data-stu-id="9e7b8-110">This new version of the function:</span></span>

    - <span data-ttu-id="9e7b8-111">Devuelve `IEnumerable<Event>` en lugar de `string` .</span><span class="sxs-lookup"><span data-stu-id="9e7b8-111">Returns `IEnumerable<Event>` instead of `string`.</span></span>
    - <span data-ttu-id="9e7b8-112">Obtiene la configuración del buzón del usuario mediante Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-112">Gets the user's mailbox settings using Microsoft Graph.</span></span>
    - <span data-ttu-id="9e7b8-113">Usa la zona horaria del usuario para calcular el inicio y el final de la semana actual.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-113">Uses the user's time zone to calculate the start and end of the current week.</span></span>
    - <span data-ttu-id="9e7b8-114">Obtiene una vista de calendario</span><span class="sxs-lookup"><span data-stu-id="9e7b8-114">Gets a calendar view</span></span>
        - <span data-ttu-id="9e7b8-115">Usa la `.Header()` función para incluir un `Prefer: outlook.timezone` encabezado, lo que hace que los eventos devueltos tengan sus horas de inicio y finalización convertidas en la zona horaria del usuario.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-115">Uses the `.Header()` function to include a `Prefer: outlook.timezone` header, which causes the returned events to have their start and end times converted to the user's timezone.</span></span>
        - <span data-ttu-id="9e7b8-116">Usa la `.Top()` función para solicitar como máximo 50 eventos.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-116">Uses the `.Top()` function to request at most 50 events.</span></span>
        - <span data-ttu-id="9e7b8-117">Usa la `.Select()` función para solicitar solo los campos que usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-117">Uses the `.Select()` function to request just the fields used by the app.</span></span>
        - <span data-ttu-id="9e7b8-118">Usa la `OrderBy()` función para ordenar los resultados por la hora de inicio.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-118">Uses the `OrderBy()` function to sort the results by the start time.</span></span>

1. <span data-ttu-id="9e7b8-119">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-119">Save your changes and restart the app.</span></span> <span data-ttu-id="9e7b8-120">Actualice la pestaña en Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-120">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="9e7b8-121">La aplicación muestra una lista JSON de los eventos.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-121">The app displays a JSON listing of the events.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="9e7b8-122">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="9e7b8-122">Display the results</span></span>

<span data-ttu-id="9e7b8-123">Ahora puede mostrar la lista de eventos de una forma más sencilla de uso.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-123">Now you can display the list of events in a more user friendly way.</span></span>

1. <span data-ttu-id="9e7b8-124">Abra **./pages/index.cshtml** y agregue las siguientes funciones dentro de la `<script>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-124">Open **./Pages/Index.cshtml** and add the following functions inside the `<script>` tag.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderHelpersSnippet":::

1. <span data-ttu-id="9e7b8-125">Reemplace la función `renderCalendar` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-125">Replace the existing `renderCalendar` function with the following.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderCalendarSnippet":::

1. <span data-ttu-id="9e7b8-126">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-126">Save your changes and restart the app.</span></span> <span data-ttu-id="9e7b8-127">Actualice la pestaña en Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-127">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="9e7b8-128">La aplicación muestra los eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="9e7b8-128">The app displays events on the user's calendar.</span></span>

    ![Una captura de pantalla de la aplicación que muestra el calendario del usuario](images/calendar-view.png)
