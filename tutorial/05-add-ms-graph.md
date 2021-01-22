---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919532"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d662d-101">En este ejercicio incorporará Microsoft Graph en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d662d-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="d662d-102">Para esta aplicación, usará el SDK de [Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d662d-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="d662d-103">Implementar un proveedor de autenticación</span><span class="sxs-lookup"><span data-stu-id="d662d-103">Implement an authentication provider</span></span>

<span data-ttu-id="d662d-104">El SDK de Microsoft Graph para Java requiere una implementación de `IAuthenticationProvider` la interfaz para crear una instancia de su `GraphServiceClient` objeto.</span><span class="sxs-lookup"><span data-stu-id="d662d-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="d662d-105">Cree un archivo en el directorio **./graphtutorial/src/main/java/graphtutorial** denominado **SimpleAuthProvider.java** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d662d-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="d662d-106">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="d662d-106">Get user details</span></span>

1. <span data-ttu-id="d662d-107">Cree un nuevo archivo en el directorio **./graphtutorial/src/main/java/graphtutorial** denominado **Graph.java** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d662d-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;

    /**
     * Graph
     */
    public class Graph {

        private static IGraphServiceClient graphClient = null;
        private static SimpleAuthProvider authProvider = null;

        private static void ensureGraphClient(String accessToken) {
            if (graphClient == null) {
                // Create the auth provider
                authProvider = new SimpleAuthProvider(accessToken);

                // Create default logger to only log errors
                DefaultLogger logger = new DefaultLogger();
                logger.setLoggingLevel(LoggerLevel.ERROR);

                // Build a Graph client
                graphClient = GraphServiceClient.builder()
                    .authenticationProvider(authProvider)
                    .logger(logger)
                    .buildClient();
            }
        }

        public static User getUser(String accessToken) {
            ensureGraphClient(accessToken);

            // GET /me to get authenticated user
            User me = graphClient
                .me()
                .buildRequest()
                .select("displayName,mailboxSettings")
                .get();

            return me;
        }
    }
    ```

1. <span data-ttu-id="d662d-108">Agregue la siguiente `import` instrucción en la parte superior de **App.java**.</span><span class="sxs-lookup"><span data-stu-id="d662d-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="d662d-109">Agregue el siguiente código en **App.java** justo antes de la línea para obtener el usuario y generar el nombre para `Scanner input = new Scanner(System.in);` mostrar del usuario.</span><span class="sxs-lookup"><span data-stu-id="d662d-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="d662d-110">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d662d-110">Run the app.</span></span> <span data-ttu-id="d662d-111">Después de iniciar sesión en la aplicación, le da la bienvenida por su nombre.</span><span class="sxs-lookup"><span data-stu-id="d662d-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="d662d-112">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="d662d-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="d662d-113">Agregue la siguiente función a la `Graph` clase en **Graph.java** para obtener eventos del calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="d662d-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="d662d-114">Ten en cuenta lo que hace este código.</span><span class="sxs-lookup"><span data-stu-id="d662d-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="d662d-115">La dirección URL a la que se llamará es `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="d662d-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="d662d-116">`QueryOption` se usan para agregar los `startDateTime` parámetros `endDateTime` and, estableciendo el inicio y el final de la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="d662d-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="d662d-117">Un `QueryOption` objeto se usa para agregar el `$orderby` parámetro, ordenando los resultados por hora de inicio.</span><span class="sxs-lookup"><span data-stu-id="d662d-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="d662d-118">Un objeto se usa para agregar el encabezado, lo que hace que las horas de inicio y finalización se ajusten a la `HeaderOption` `Prefer: outlook.timezone` zona horaria del usuario.</span><span class="sxs-lookup"><span data-stu-id="d662d-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="d662d-119">La `select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.</span><span class="sxs-lookup"><span data-stu-id="d662d-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="d662d-120">La `top` función limita el número de eventos en la respuesta a un máximo de 25.</span><span class="sxs-lookup"><span data-stu-id="d662d-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="d662d-121">La función se usa para solicitar páginas de resultados adicionales si hay más de `getNextPage` 25 eventos en la semana actual.</span><span class="sxs-lookup"><span data-stu-id="d662d-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="d662d-122">Cree un nuevo archivo en el directorio **./graphtutorial/src/main/java/graphtutorial** denominado **GraphToIana.java** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d662d-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="d662d-123">Esta clase implementa una búsqueda simple para convertir los nombres de zona horaria de Windows en identificadores IANA y para generar un **ZoneId** basado en un nombre de zona horaria de Windows.</span><span class="sxs-lookup"><span data-stu-id="d662d-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="d662d-124">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="d662d-124">Display the results</span></span>

1. <span data-ttu-id="d662d-125">Agregue las siguientes `import` instrucciones en **App.java**.</span><span class="sxs-lookup"><span data-stu-id="d662d-125">Add the following `import` statements in **App.java**.</span></span>

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
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="d662d-126">Agregue la siguiente función a la clase para dar formato a las propiedades `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph en un formato fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="d662d-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="d662d-127">Agregue la siguiente función a la clase para obtener los eventos del usuario y `App` generarlos en la consola.</span><span class="sxs-lookup"><span data-stu-id="d662d-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="d662d-128">Agregue lo siguiente justo después del `// List the calendar` comentario en la `main` función.</span><span class="sxs-lookup"><span data-stu-id="d662d-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="d662d-129">Guarda todos los cambios, compila la aplicación y, a continuación, ejecutala.</span><span class="sxs-lookup"><span data-stu-id="d662d-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="d662d-130">Elija la **opción Enumerar eventos de** calendario para ver una lista de los eventos del usuario.</span><span class="sxs-lookup"><span data-stu-id="d662d-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
