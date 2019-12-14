---
ms.openlocfilehash: 3fb56d613dbaa56474c9af58ebdd0b157c53bf1a
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018858"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará el [SDK de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.

## <a name="implement-an-authentication-provider"></a>Implementación de un proveedor de autenticación

El SDK de Microsoft Graph para Java requiere una implementación de `IAuthenticationProvider` la interfaz para crear una `GraphServiceClient` instancia de su objeto. Empiece por crear una clase simple para agregar el token de acceso a las solicitudes salientes. Cree un archivo nuevo en el directorio **./graphtutorial/src/Main/Java/com/contoso** denominado **SimpleAuthProvider. Java** y agregue el código siguiente.

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

## <a name="get-user-details"></a>Obtener detalles del usuario

En primer lugar, agregue una clase nueva que contenga toda la funcionalidad del gráfico. Cree un nuevo archivo en el directorio **./graphtutorial/src/Main/Java/com/contoso** denominado **Graph. Java** y agregue el código siguiente.

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

Agregue el siguiente código en **app. Java** justo antes de `Scanner input = new Scanner(System.in);` la línea para obtener el usuario y generar el nombre para mostrar del usuario.

```java
// Greet the user
User user = Graph.getUser(accessToken);
System.out.println("Welcome " + user.displayName);
System.out.println();
```

Si ejecuta la aplicación ahora, una vez que inicie sesión en la aplicación, le agradecerá por su nombre.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos de calendario de Outlook

Agregue las siguientes `import` instrucciones a **Graph. Java**.

```java
import java.util.LinkedList;
import java.util.List;
import com.microsoft.graph.models.extensions.Event;
import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
```

Agregue la siguiente función a la `Graph` clase en **Graph. Java** para obtener los eventos del calendario del usuario.

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

Tenga en cuenta lo que está haciendo este código.

- La dirección URL a la que se `/me/events`llamará es.
- La `select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.
- Un `QueryOption` se usa para ordenar los resultados por la fecha y la hora de creación, con el elemento más reciente en primer lugar.

## <a name="display-the-results"></a>Mostrar los resultados

Para empezar, agregue las `import` siguientes instrucciones en **app. Java**.

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.List;
```

A continuación, agregue la siguiente función `App` a la clase para dar formato a las propiedades de [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph a un formato fácil de uso.

```java
private static String formatDateTimeTimeZone(DateTimeTimeZone date) {
    LocalDateTime dateTime = LocalDateTime.parse(date.dateTime);

    return dateTime.format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT)) + " (" + date.timeZone + ")";
}
```

A continuación, agregue la siguiente función a `App` la clase para obtener los eventos del usuario y enviarlos a la consola.

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

Por último, agregue lo siguiente justo después `// List the calendar` del comentario en `main` la función.

```java
listCalendarEvents(accessToken);
```

Guarde todos los cambios y ejecute la aplicación. Elija la opción **lista de eventos de calendario** para ver una lista de los eventos del usuario.

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
