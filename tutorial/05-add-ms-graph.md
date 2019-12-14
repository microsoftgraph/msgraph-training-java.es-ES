---
ms.openlocfilehash: 3fb56d613dbaa56474c9af58ebdd0b157c53bf1a
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018858"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="42217-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="42217-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="42217-102">Para esta aplicación, usará el [SDK de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="42217-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="42217-103">Implementación de un proveedor de autenticación</span><span class="sxs-lookup"><span data-stu-id="42217-103">Implement an authentication provider</span></span>

<span data-ttu-id="42217-104">El SDK de Microsoft Graph para Java requiere una implementación de `IAuthenticationProvider` la interfaz para crear una `GraphServiceClient` instancia de su objeto.</span><span class="sxs-lookup"><span data-stu-id="42217-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span> <span data-ttu-id="42217-105">Empiece por crear una clase simple para agregar el token de acceso a las solicitudes salientes.</span><span class="sxs-lookup"><span data-stu-id="42217-105">Start by creating a simple class to add the access token to outgoing requests.</span></span> <span data-ttu-id="42217-106">Cree un archivo nuevo en el directorio **./graphtutorial/src/Main/Java/com/contoso** denominado **SimpleAuthProvider. Java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="42217-106">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

```java
package com.contoso;

import com.microsoft.graph.authentication.IAuthenticationProvider;
import com.microsoft.graph.http.IHttpRequest;

/**
 * SimpleAuthProvider
 */
public class SimpleAuthProvider implements IAuthenticationProvider {

    private String accessToken = null;

    public SimpleAuthProvider(String accessToken) {
        this.accessToken = accessToken;
    }

    @Override
    public void authenticateRequest(IHttpRequest request) {
        // Add the access token in the Authorization header
        request.addHeader("Authorization", "Bearer " + accessToken);
    }
}
```

## <a name="get-user-details"></a><span data-ttu-id="42217-107">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="42217-107">Get user details</span></span>

<span data-ttu-id="42217-108">En primer lugar, agregue una clase nueva que contenga toda la funcionalidad del gráfico.</span><span class="sxs-lookup"><span data-stu-id="42217-108">First, add a new class to contain all of the Graph functionality.</span></span> <span data-ttu-id="42217-109">Cree un nuevo archivo en el directorio **./graphtutorial/src/Main/Java/com/contoso** denominado **Graph. Java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="42217-109">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Graph.java** and add the following code.</span></span>

```java
package com.contoso;

import com.microsoft.graph.logger.DefaultLogger;
import com.microsoft.graph.logger.LoggerLevel;
import com.microsoft.graph.models.extensions.IGraphServiceClient;
import com.microsoft.graph.models.extensions.User;
import com.microsoft.graph.requests.extensions.GraphServiceClient;
import java.util.LinkedList;
import java.util.List;import com.microsoft.graph.models.extensions.Event;import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
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

<span data-ttu-id="42217-110">Agregue el siguiente código en **app. Java** justo antes de `Scanner input = new Scanner(System.in);` la línea para obtener el usuario y generar el nombre para mostrar del usuario.</span><span class="sxs-lookup"><span data-stu-id="42217-110">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

```java
// Greet the user
User user = Graph.getUser(accessToken);
System.out.println("Welcome " + user.displayName);
System.out.println();
```

<span data-ttu-id="42217-111">Si ejecuta la aplicación ahora, una vez que inicie sesión en la aplicación, le agradecerá por su nombre.</span><span class="sxs-lookup"><span data-stu-id="42217-111">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="42217-112">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="42217-112">Get calendar events from Outlook</span></span>

<span data-ttu-id="42217-113">Agregue las siguientes `import` instrucciones a **Graph. Java**.</span><span class="sxs-lookup"><span data-stu-id="42217-113">Add the following `import` statements to **Graph.java**.</span></span>

```java
import java.util.LinkedList;
import java.util.List;
import com.microsoft.graph.models.extensions.Event;
import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
```

<span data-ttu-id="42217-114">Agregue la siguiente función a la `Graph` clase en **Graph. Java** para obtener los eventos del calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="42217-114">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

```java
public static List<Event> getEvents(String accessToken) {
    ensureGraphClient(accessToken);

    // Use QueryOption to specify the $orderby query parameter
    final List<Option> options = new LinkedList<Option>();
    // Sort results by createdDateTime, get newest first
    options.add(new QueryOption("orderby", "createdDateTime DESC"));

    // GET /me/events
    IEventCollectionPage eventPage = graphClient
        .me()
        .events()
        .buildRequest(options)
        .select("subject,organizer,start,end")
        .get();

    return eventPage.getCurrentPage();
}
```

<span data-ttu-id="42217-115">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="42217-115">Consider what this code is doing.</span></span>

- <span data-ttu-id="42217-116">La dirección URL a la que se `/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="42217-116">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="42217-117">La `select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.</span><span class="sxs-lookup"><span data-stu-id="42217-117">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="42217-118">Un `QueryOption` se usa para ordenar los resultados por la fecha y la hora de creación, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="42217-118">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="42217-119">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="42217-119">Display the results</span></span>

<span data-ttu-id="42217-120">Para empezar, agregue las `import` siguientes instrucciones en **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="42217-120">Start by adding the following `import` statements in **App.java**.</span></span>

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.List;
```

<span data-ttu-id="42217-121">A continuación, agregue la siguiente función `App` a la clase para dar formato a las propiedades de [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph a un formato fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="42217-121">Then add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

```java
private static String formatDateTimeTimeZone(DateTimeTimeZone date) {
    LocalDateTime dateTime = LocalDateTime.parse(date.dateTime);

    return dateTime.format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT)) + " (" + date.timeZone + ")";
}
```

<span data-ttu-id="42217-122">A continuación, agregue la siguiente función a `App` la clase para obtener los eventos del usuario y enviarlos a la consola.</span><span class="sxs-lookup"><span data-stu-id="42217-122">Next, add the following function to the `App` class to get the user's events and output them to the console.</span></span>

```java
private static void listCalendarEvents(String accessToken) {
    // Get the user's events
    List<Event> events = Graph.getEvents(accessToken);

    System.out.println("Events:");

    for (Event event : events) {
        System.out.println("Subject: " + event.subject);
        System.out.println("  Organizer: " + event.organizer.emailAddress.name);
        System.out.println("  Start: " + formatDateTimeTimeZone(event.start));
        System.out.println("  End: " + formatDateTimeTimeZone(event.end));
    }

    System.out.println();
}
```

<span data-ttu-id="42217-123">Por último, agregue lo siguiente justo después `// List the calendar` del comentario en `main` la función.</span><span class="sxs-lookup"><span data-stu-id="42217-123">Finally, add the following just after the `// List the calendar` comment in the `main` function.</span></span>

```java
listCalendarEvents(accessToken);
```

<span data-ttu-id="42217-124">Guarde todos los cambios y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="42217-124">Save all of your changes and run the app.</span></span> <span data-ttu-id="42217-125">Elija la opción **lista de eventos de calendario** para ver una lista de los eventos del usuario.</span><span class="sxs-lookup"><span data-stu-id="42217-125">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
