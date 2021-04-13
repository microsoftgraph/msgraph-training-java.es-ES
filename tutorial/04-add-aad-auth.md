---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695796"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0ecfe-101">En este ejercicio, extenderá la aplicación desde el ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="0ecfe-102">Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="0ecfe-103">En este paso, integrará la Biblioteca de autenticación de [Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="0ecfe-104">Cree un nuevo directorio denominado **graphtutorial** en **el directorio ./src/main/resources.**</span><span class="sxs-lookup"><span data-stu-id="0ecfe-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="0ecfe-105">Cree un nuevo archivo en **el directorio ./src/main/resources/graphtutorial** denominado **oAuth.properties** y agregue el siguiente texto en ese archivo.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="0ecfe-106">Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación que creó en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="0ecfe-107">El valor de `app.scopes` contiene los ámbitos de permisos que requiere la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="0ecfe-108">**User.Read permite** que la aplicación acceda al perfil del usuario.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="0ecfe-109">**MailboxSettings.Read permite** que la aplicación obtenga acceso a la configuración desde el buzón del usuario, incluida la zona horaria configurada por el usuario.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="0ecfe-110">**Calendars.ReadWrite** permite a la aplicación enumerar el calendario del usuario y agregar nuevos eventos al calendario.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0ecfe-111">Si usas el control de código fuente como git, ahora sería un buen momento para excluir el archivo **oAuth.properties** del control de código fuente para evitar la pérdida involuntaria del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="0ecfe-112">Abra **App.java** y agregue las `import` siguientes instrucciones.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="0ecfe-113">Agregue el siguiente código justo antes de la `Scanner input = new Scanner(System.in);` línea para cargar el archivo **oAuth.properties.**</span><span class="sxs-lookup"><span data-stu-id="0ecfe-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="0ecfe-114">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="0ecfe-114">Implement sign-in</span></span>

1. <span data-ttu-id="0ecfe-115">Cree un nuevo archivo en el **directorio ./graphtutorial/src/main/java/graphtutorial** denominado **Graph.java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

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

1. <span data-ttu-id="0ecfe-116">En **App.java,** agregue el siguiente código justo antes de la `Scanner input = new Scanner(System.in);` línea para obtener un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. <span data-ttu-id="0ecfe-117">Agregue la siguiente línea después del `// Display access token` comentario.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="0ecfe-118">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-118">Run the app.</span></span> <span data-ttu-id="0ecfe-119">La aplicación muestra una dirección URL y un código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="0ecfe-120">Abra un explorador y vaya a la dirección URL mostrada.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="0ecfe-121">Escriba el código proporcionado e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="0ecfe-122">Una vez completado, vuelva a la aplicación y elija **el 1. Muestra la opción de token** de acceso para mostrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="0ecfe-123">Los tokens de acceso para cuentas profesionales o educativas de Microsoft se pueden analizar para solucionar problemas en [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="0ecfe-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="0ecfe-124">Los tokens de acceso para cuentas personales de Microsoft usan un formato propietario y no se pueden analizar.</span><span class="sxs-lookup"><span data-stu-id="0ecfe-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
