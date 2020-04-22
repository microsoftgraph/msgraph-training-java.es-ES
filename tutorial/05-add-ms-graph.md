---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612071"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="6bf80-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6bf80-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="6bf80-102">Para esta aplicación, usará el [SDK de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="6bf80-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="6bf80-103">Implementación de un proveedor de autenticación</span><span class="sxs-lookup"><span data-stu-id="6bf80-103">Implement an authentication provider</span></span>

<span data-ttu-id="6bf80-104">El SDK de Microsoft Graph para Java requiere una implementación de `IAuthenticationProvider` la interfaz para crear una `GraphServiceClient` instancia de su objeto.</span><span class="sxs-lookup"><span data-stu-id="6bf80-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="6bf80-105">Cree un archivo nuevo en el directorio **./graphtutorial/src/Main/Java/graphtutorial** denominado **SimpleAuthProvider. Java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="6bf80-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="6bf80-106">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="6bf80-106">Get user details</span></span>

1. <span data-ttu-id="6bf80-107">Cree un nuevo archivo en el directorio **./graphtutorial/src/Main/Java/graphtutorial** denominado **Graph. Java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="6bf80-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.util.LinkedList;
    import java.util.List;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;

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
                .get();

            return me;
        }
    }
    ```

1. <span data-ttu-id="6bf80-108">Agregue la siguiente `import` instrucción en la parte superior de **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="6bf80-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="6bf80-109">Agregue el siguiente código en **app. Java** justo antes de `Scanner input = new Scanner(System.in);` la línea para obtener el usuario y generar el nombre para mostrar del usuario.</span><span class="sxs-lookup"><span data-stu-id="6bf80-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. <span data-ttu-id="6bf80-110">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6bf80-110">Run the app.</span></span> <span data-ttu-id="6bf80-111">Después de iniciar sesión en la aplicación, le damos la bienvenida por nombre.</span><span class="sxs-lookup"><span data-stu-id="6bf80-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="6bf80-112">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="6bf80-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="6bf80-113">Agregue la siguiente función a la `Graph` clase en **Graph. Java** para obtener los eventos del calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="6bf80-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="6bf80-114">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="6bf80-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="6bf80-115">La dirección URL a la que se `/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="6bf80-115">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="6bf80-116">La `select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.</span><span class="sxs-lookup"><span data-stu-id="6bf80-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="6bf80-117">Un `QueryOption` se usa para ordenar los resultados por la fecha y la hora de creación, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="6bf80-117">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="6bf80-118">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="6bf80-118">Display the results</span></span>

1. <span data-ttu-id="6bf80-119">Agregue las siguientes `import` instrucciones en **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="6bf80-119">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="6bf80-120">Agregue la siguiente función a la `App` clase para dar formato a las propiedades de [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph a un formato fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="6bf80-120">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="6bf80-121">Agregue la siguiente función a la `App` clase para obtener los eventos del usuario y enviarlos a la consola.</span><span class="sxs-lookup"><span data-stu-id="6bf80-121">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="6bf80-122">Agregue lo siguiente justo después del `// List the calendar` comentario en la `main` función.</span><span class="sxs-lookup"><span data-stu-id="6bf80-122">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken);
    ```

1. <span data-ttu-id="6bf80-123">Guarde todos los cambios, compile la aplicación y, a continuación, ejecútela.</span><span class="sxs-lookup"><span data-stu-id="6bf80-123">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="6bf80-124">Elija la opción **lista de eventos de calendario** para ver una lista de los eventos del usuario.</span><span class="sxs-lookup"><span data-stu-id="6bf80-124">Choose the **List calendar events** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM (UTC)
      End: 5/22/19, 4:00 PM (UTC)
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM (UTC)
      End: 5/24/19, 8:00 PM (UTC)
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM (UTC)
      End: 5/26/19, 7:00 PM (UTC)
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM (UTC)
      End: 5/27/19, 10:30 PM (UTC)
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM (UTC)
      End: 5/28/19, 5:00 PM (UTC)
    ```
