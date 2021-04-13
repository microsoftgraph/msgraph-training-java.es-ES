---
ms.openlocfilehash: 397d564fc3389f341e06977bd4cba861dd1c9e5b
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695810"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará el SDK de [Microsoft Graph Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.

## <a name="get-user-details"></a>Obtener detalles del usuario

1. Cree un nuevo archivo en el **directorio ./graphtutorial/src/main/java/graphtutorial** denominado **Graph.java** y agregue el código siguiente.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetUserSnippet":::

1. Agregue la siguiente `import` instrucción en la parte superior de **App.java**.

    ```java
    import com.microsoft.graph.models.User;
    ```

1. Agregue el siguiente código en **App.java justo** antes de la línea para obtener el usuario y generar el nombre para mostrar `Scanner input = new Scanner(System.in);` del usuario.

    ```java
    // Greet the user
    User user = Graph.getUser();
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Ejecute la aplicación. Después de iniciar sesión en la aplicación le da la bienvenida por su nombre.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos del calendario desde Outlook

1. Agregue la siguiente función a la `Graph` clase en **Graph.java** para obtener eventos del calendario del usuario.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Tenga en cuenta lo que está haciendo este código.

- La dirección URL a la que se llamará es `/me/calendarview`.
  - `QueryOption` objetos se usan para agregar los `startDateTime` parámetros `endDateTime` and, estableciendo el inicio y el final de la vista de calendario.
  - Se `QueryOption` usa un objeto para agregar el `$orderby` parámetro, ordenando los resultados por hora de inicio.
  - Un objeto se usa para agregar el encabezado, lo que hace que las horas de inicio y finalización se ajusten a la zona `HeaderOption` `Prefer: outlook.timezone` horaria del usuario.
  - La `select` función limita los campos devueltos para cada evento a solo aquellos que la aplicación usará realmente.
  - La `top` función limita el número de eventos en la respuesta a un máximo de 25.
- La función se usa para solicitar páginas adicionales de resultados si hay más de `getNextPage` 25 eventos en la semana actual.

1. Cree un nuevo archivo en el **directorio ./graphtutorial/src/main/java/graphtutorial** denominado **GraphToIana.java** y agregue el código siguiente.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Esta clase implementa una búsqueda sencilla para convertir nombres de zona horaria de Windows en identificadores de IANA y para generar un **ZoneId** basado en un nombre de zona horaria de Windows.

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
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.Event;
    ```

1. Agregue la siguiente función a la clase para dar formato a las propiedades `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph en un formato fácil de usar.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Agregue la siguiente función a la clase para obtener los eventos del usuario y `App` generarlos en la consola.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Agregue lo siguiente justo después del `// List the calendar` comentario en la `main` función.

    ```java
    listCalendarEvents(user.mailboxSettings.timeZone);
    ```

1. Guarda todos los cambios, crea la aplicación y, a continuación, ejecutala. Elija la **opción Enumerar eventos de** calendario para ver una lista de los eventos del usuario.

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
