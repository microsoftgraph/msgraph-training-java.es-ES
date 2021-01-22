---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919532"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio incorporará Microsoft Graph en la aplicación. Para esta aplicación, usará el SDK de [Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) realizar llamadas a Microsoft Graph.

## <a name="implement-an-authentication-provider"></a>Implementar un proveedor de autenticación

El SDK de Microsoft Graph para Java requiere una implementación de `IAuthenticationProvider` la interfaz para crear una instancia de su `GraphServiceClient` objeto.

1. Cree un archivo en el directorio **./graphtutorial/src/main/java/graphtutorial** denominado **SimpleAuthProvider.java** y agregue el siguiente código.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>Obtener detalles del usuario

1. Cree un nuevo archivo en el directorio **./graphtutorial/src/main/java/graphtutorial** denominado **Graph.java** y agregue el siguiente código.

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

1. Agregue la siguiente `import` instrucción en la parte superior de **App.java**.

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. Agregue el siguiente código en **App.java** justo antes de la línea para obtener el usuario y generar el nombre para `Scanner input = new Scanner(System.in);` mostrar del usuario.

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Ejecute la aplicación. Después de iniciar sesión en la aplicación, le da la bienvenida por su nombre.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos de calendario de Outlook

1. Agregue la siguiente función a la `Graph` clase en **Graph.java** para obtener eventos del calendario del usuario.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Ten en cuenta lo que hace este código.

- La dirección URL a la que se llamará es `/me/calendarview` .
  - `QueryOption` se usan para agregar los `startDateTime` parámetros `endDateTime` and, estableciendo el inicio y el final de la vista de calendario.
  - Un `QueryOption` objeto se usa para agregar el `$orderby` parámetro, ordenando los resultados por hora de inicio.
  - Un objeto se usa para agregar el encabezado, lo que hace que las horas de inicio y finalización se ajusten a la `HeaderOption` `Prefer: outlook.timezone` zona horaria del usuario.
  - La `select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.
  - La `top` función limita el número de eventos en la respuesta a un máximo de 25.
- La función se usa para solicitar páginas de resultados adicionales si hay más de `getNextPage` 25 eventos en la semana actual.

1. Cree un nuevo archivo en el directorio **./graphtutorial/src/main/java/graphtutorial** denominado **GraphToIana.java** y agregue el siguiente código.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Esta clase implementa una búsqueda simple para convertir los nombres de zona horaria de Windows en identificadores IANA y para generar un **ZoneId** basado en un nombre de zona horaria de Windows.

## <a name="display-the-results"></a>Mostrar los resultados

1. Agregue las siguientes `import` instrucciones en **App.java**.

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

1. Agregue la siguiente función a la clase para dar formato a las propiedades `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph en un formato fácil de usar.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Agregue la siguiente función a la clase para obtener los eventos del usuario y `App` generarlos en la consola.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Agregue lo siguiente justo después del `// List the calendar` comentario en la `main` función.

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. Guarda todos los cambios, compila la aplicación y, a continuación, ejecutala. Elija la **opción Enumerar eventos de** calendario para ver una lista de los eventos del usuario.

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
