---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695796"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, extenderá la aplicación desde el ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la Biblioteca de autenticación de [Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) en la aplicación.

1. Cree un nuevo directorio denominado **graphtutorial** en **el directorio ./src/main/resources.**

1. Cree un nuevo archivo en **el directorio ./src/main/resources/graphtutorial** denominado **oAuth.properties** y agregue el siguiente texto en ese archivo. Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación que creó en Azure Portal.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    El valor de `app.scopes` contiene los ámbitos de permisos que requiere la aplicación.

    - **User.Read permite** que la aplicación acceda al perfil del usuario.
    - **MailboxSettings.Read permite** que la aplicación obtenga acceso a la configuración desde el buzón del usuario, incluida la zona horaria configurada por el usuario.
    - **Calendars.ReadWrite** permite a la aplicación enumerar el calendario del usuario y agregar nuevos eventos al calendario.

    > [!IMPORTANT]
    > Si usas el control de código fuente como git, ahora sería un buen momento para excluir el archivo **oAuth.properties** del control de código fuente para evitar la pérdida involuntaria del identificador de la aplicación.

1. Abra **App.java** y agregue las `import` siguientes instrucciones.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Agregue el siguiente código justo antes de la `Scanner input = new Scanner(System.in);` línea para cargar el archivo **oAuth.properties.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

1. Cree un nuevo archivo en el **directorio ./graphtutorial/src/main/java/graphtutorial** denominado **Graph.java** y agregue el código siguiente.

    ```java
    package graphtutorial;

    import java.net.URL;
    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import okhttp3.Request;

    import com.azure.identity.DeviceCodeCredential;
    import com.azure.identity.DeviceCodeCredentialBuilder;

    import com.microsoft.graph.authentication.TokenCredentialAuthProvider;
    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.Attendee;
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.EmailAddress;
    import com.microsoft.graph.models.Event;
    import com.microsoft.graph.models.ItemBody;
    import com.microsoft.graph.models.User;
    import com.microsoft.graph.models.AttendeeType;
    import com.microsoft.graph.models.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.GraphServiceClient;
    import com.microsoft.graph.requests.EventCollectionPage;
    import com.microsoft.graph.requests.EventCollectionRequestBuilder;

    public class Graph {

        private static GraphServiceClient<Request> graphClient = null;
        private static TokenCredentialAuthProvider authProvider = null;

        public static void initializeGraphAuth(String applicationId, List<String> scopes) {
            // Create the auth provider
            final DeviceCodeCredential credential = new DeviceCodeCredentialBuilder()
                .clientId(applicationId)
                .challengeConsumer(challenge -> System.out.println(challenge.getMessage()))
                .build();

            authProvider = new TokenCredentialAuthProvider(scopes, credential);

            // Create default logger to only log errors
            DefaultLogger logger = new DefaultLogger();
            logger.setLoggingLevel(LoggerLevel.ERROR);

            // Build a Graph client
            graphClient = GraphServiceClient.builder()
                .authenticationProvider(authProvider)
                .logger(logger)
                .buildClient();
        }

        public static String getUserAccessToken()
        {
            try {
                URL meUrl = new URL("https://graph.microsoft.com/v1.0/me");
                return authProvider.getAuthorizationTokenAsync(meUrl).get();
            } catch(Exception ex) {
                return null;
            }
        }
    }
    ```

1. En **App.java,** agregue el siguiente código justo antes de la `Scanner input = new Scanner(System.in);` línea para obtener un token de acceso.

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. Agregue la siguiente línea después del `// Display access token` comentario.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Ejecute la aplicación. La aplicación muestra una dirección URL y un código de dispositivo.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Abra un explorador y vaya a la dirección URL mostrada. Escriba el código proporcionado e inicie sesión. Una vez completado, vuelva a la aplicación y elija **el 1. Muestra la opción de token** de acceso para mostrar el token de acceso.

> [!TIP]
> Los tokens de acceso para cuentas profesionales o educativas de Microsoft se pueden analizar para solucionar problemas en [https://jwt.ms](https://jwt.ms) . Los tokens de acceso para cuentas personales de Microsoft usan un formato propietario y no se pueden analizar.
