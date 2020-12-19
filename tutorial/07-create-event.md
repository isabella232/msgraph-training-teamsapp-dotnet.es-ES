<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4ecbb-101">En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-the-new-event-tab"></a><span data-ttu-id="4ecbb-102">Crear la nueva pestaña de eventos</span><span class="sxs-lookup"><span data-stu-id="4ecbb-102">Create the new event tab</span></span>

1. <span data-ttu-id="4ecbb-103">Cree un nuevo archivo en el directorio **./páginas** denominado **NewEvent. cshtml** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-103">Create a new file in the **./Pages** directory named **NewEvent.cshtml** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.cshtml":::

    <span data-ttu-id="4ecbb-104">Esto implementa un formulario sencillo y agrega JavaScript para enviar los datos del formulario a la API Web.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-104">This implements a simple form, and adds JavaScript to post the form data to the Web API.</span></span>

## <a name="implement-the-web-api"></a><span data-ttu-id="4ecbb-105">Implementar la API Web</span><span class="sxs-lookup"><span data-stu-id="4ecbb-105">Implement the Web API</span></span>

1. <span data-ttu-id="4ecbb-106">Cree un nuevo directorio denominado **Models** en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-106">Create a new directory named **Models** in the root of the project.</span></span>

1. <span data-ttu-id="4ecbb-107">Cree un archivo nuevo en el directorio **./Models** denominado **NewEvent.CS** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-107">Create a new file in the **./Models** directory named **NewEvent.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Models/NewEvent.cs" id="NewEventSnippet":::

1. <span data-ttu-id="4ecbb-108">Abra **./Controllers/CalendarController.CS** y agregue la siguiente `using` instrucción en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-108">Open **./Controllers/CalendarController.cs** and add the following `using` statement at the top of the file.</span></span>

    ```csharp
    using GraphTutorial.Models;
    ```

1. <span data-ttu-id="4ecbb-109">Agregue la siguiente función a la clase **CalendarController** .</span><span class="sxs-lookup"><span data-stu-id="4ecbb-109">Add the following function to the **CalendarController** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="PostSnippet":::

    <span data-ttu-id="4ecbb-110">Esto permite que HTTP POST a la API Web con los campos del formulario.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-110">This allows an HTTP POST to the Web API with the fields of the form.</span></span>

1. <span data-ttu-id="4ecbb-111">Guarde todos los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-111">Save all of your changes and restart the application.</span></span> <span data-ttu-id="4ecbb-112">Actualice la aplicación en Microsoft Teams y seleccione la pestaña **crear evento** . Rellene el formulario y seleccione **crear** para agregar un evento al calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="4ecbb-112">Refresh the app in Microsoft Teams, and select the **Create event** tab. Fill out the form and select **Create** to add an event to the user's calendar.</span></span>

    ![Captura de pantalla de la pestaña evento de creación](images/create-event.png)
