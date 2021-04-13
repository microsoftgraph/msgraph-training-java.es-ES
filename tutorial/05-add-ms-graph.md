---
ms.openlocfilehash: 397d564fc3389f341e06977bd4cba861dd1c9e5b
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695810"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7187c-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7187c-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="7187c-102">Para esta aplicación, usará el SDK de [Microsoft Graph Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7187c-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="7187c-103">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="7187c-103">Get user details</span></span>

1. <span data-ttu-id="7187c-104">Cree un nuevo archivo en el **directorio ./graphtutorial/src/main/java/graphtutorial** denominado **Graph.java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="7187c-104">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetUserSnippet":::

1. <span data-ttu-id="7187c-105">Agregue la siguiente `import` instrucción en la parte superior de **App.java**.</span><span class="sxs-lookup"><span data-stu-id="7187c-105">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.User;
    ```

1. <span data-ttu-id="7187c-106">Agregue el siguiente código en **App.java justo** antes de la línea para obtener el usuario y generar el nombre para mostrar `Scanner input = new Scanner(System.in);` del usuario.</span><span class="sxs-lookup"><span data-stu-id="7187c-106">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser();
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="7187c-107">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7187c-107">Run the app.</span></span> <span data-ttu-id="7187c-108">Después de iniciar sesión en la aplicación le da la bienvenida por su nombre.</span><span class="sxs-lookup"><span data-stu-id="7187c-108">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="7187c-109">Obtener eventos del calendario desde Outlook</span><span class="sxs-lookup"><span data-stu-id="7187c-109">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="7187c-110">Agregue la siguiente función a la `Graph` clase en **Graph.java** para obtener eventos del calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="7187c-110">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="7187c-111">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="7187c-111">Consider what this code is doing.</span></span>

- <span data-ttu-id="7187c-112">La dirección URL a la que se llamará es `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="7187c-112">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="7187c-113">`QueryOption` objetos se usan para agregar los `startDateTime` parámetros `endDateTime` and, estableciendo el inicio y el final de la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="7187c-113">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="7187c-114">Se `QueryOption` usa un objeto para agregar el `$orderby` parámetro, ordenando los resultados por hora de inicio.</span><span class="sxs-lookup"><span data-stu-id="7187c-114">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="7187c-115">Un objeto se usa para agregar el encabezado, lo que hace que las horas de inicio y finalización se ajusten a la zona `HeaderOption` `Prefer: outlook.timezone` horaria del usuario.</span><span class="sxs-lookup"><span data-stu-id="7187c-115">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="7187c-116">La `select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.</span><span class="sxs-lookup"><span data-stu-id="7187c-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="7187c-117">La `top` función limita el número de eventos en la respuesta a un máximo de 25.</span><span class="sxs-lookup"><span data-stu-id="7187c-117">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="7187c-118">La función se usa para solicitar páginas adicionales de resultados si hay más de `getNextPage` 25 eventos en la semana actual.</span><span class="sxs-lookup"><span data-stu-id="7187c-118">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="7187c-119">Cree un nuevo archivo en el **directorio ./graphtutorial/src/main/java/graphtutorial** denominado **GraphToIana.java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="7187c-119">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="7187c-120">Esta clase implementa una búsqueda sencilla para convertir nombres de zona horaria de Windows en identificadores de IANA y para generar un **ZoneId** basado en un nombre de zona horaria de Windows.</span><span class="sxs-lookup"><span data-stu-id="7187c-120">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="7187c-121">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="7187c-121">Display the results</span></span>

1. <span data-ttu-id="7187c-122">Agregue las siguientes `import` instrucciones en **App.java**.</span><span class="sxs-lookup"><span data-stu-id="7187c-122">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDateTime;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.DateTimeParseException;
    import java.time.format.FormatStyle;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.HashSet;
    import java.util.List;
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.Event;
    ```

1. <span data-ttu-id="7187c-123">Agregue la siguiente función a la clase para dar formato a las propiedades `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph en un formato fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="7187c-123">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="7187c-124">Agregue la siguiente función a la clase para obtener los eventos del usuario y `App` generarlos en la consola.</span><span class="sxs-lookup"><span data-stu-id="7187c-124">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="7187c-125">Agregue lo siguiente justo después del `// List the calendar` comentario en la `main` función.</span><span class="sxs-lookup"><span data-stu-id="7187c-125">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="7187c-126">Guarda todos los cambios, crea la aplicación y, a continuación, ejecutala.</span><span class="sxs-lookup"><span data-stu-id="7187c-126">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="7187c-127">Elija la **opción Enumerar eventos de** calendario para ver una lista de los eventos del usuario.</span><span class="sxs-lookup"><span data-stu-id="7187c-127">Choose the **List calendar events** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 12/7/20, 2:00 PM (Pacific Standard Time)
      End: 12/7/20, 3:00 PM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/7/20, 4:00 PM (Pacific Standard Time)
      End: 12/7/20, 5:30 PM (Pacific Standard Time)
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 12/8/20, 12:00 PM (Pacific Standard Time)
      End: 12/8/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/8/20, 3:00 PM (Pacific Standard Time)
      End: 12/8/20, 4:30 PM (Pacific Standard Time)
    Subject: Company Meeting
      Organizer: Christie Cline
      Start: 12/9/20, 8:30 AM (Pacific Standard Time)
      End: 12/9/20, 11:00 AM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/9/20, 4:00 PM (Pacific Standard Time)
      End: 12/9/20, 5:30 PM (Pacific Standard Time)
    Subject: Project Team Meeting
      Organizer: Lidia Holloway
      Start: 12/10/20, 8:00 AM (Pacific Standard Time)
      End: 12/10/20, 9:30 AM (Pacific Standard Time)
    Subject: Weekly Marketing Lunch
      Organizer: Adele Vance
      Start: 12/10/20, 12:00 PM (Pacific Standard Time)
      End: 12/10/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/10/20, 3:00 PM (Pacific Standard Time)
      End: 12/10/20, 4:30 PM (Pacific Standard Time)
    Subject: Lunch?
      Organizer: Lynne Robbins
      Start: 12/11/20, 12:00 PM (Pacific Standard Time)
      End: 12/11/20, 1:00 PM (Pacific Standard Time)
    Subject: Friday Unwinder
      Organizer: Megan Bowen
      Start: 12/11/20, 4:00 PM (Pacific Standard Time)
      End: 12/11/20, 5:00 PM (Pacific Standard Time)
    ```
