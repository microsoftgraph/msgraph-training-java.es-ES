---
ms.openlocfilehash: 377db05d9b238d3f6f6e45032f0b85b9217df3db
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018861"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la [biblioteca de autenticación de Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) en la aplicación.

Cree un nuevo directorio denominado **recursos** en el directorio **./graphtutorial/src/Main** . A continuación, cree un nuevo directorio llamado **com** en el directorio de **recursos** y, a continuación, un directorio nuevo denominado **contoso** en el directorio **com** . Por último, cree un nuevo archivo en el directorio **./graphtutorial/src/Main/Resources/com/contoso** denominado **OAuth. Properties**y agregue el siguiente texto en ese archivo.

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación que creó en Azure portal.

> [!IMPORTANT]
> Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el archivo **OAuth. Properties** del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.

Abra **app. Java** y agregue las siguientes `import` instrucciones.

```java
import java.io.IOException;
import java.util.Properties;
```

A continuación, agregue el siguiente código justo `Scanner input = new Scanner(System.in);` antes de la línea para cargar el archivo **OAuth. Properties** .

```java
// Load OAuth settings
final Properties oAuthProperties = new Properties();
try {
    oAuthProperties.load(App.class.getResourceAsStream("oAuth.properties"));
} catch (IOException e) {
    System.out.println("Unable to read OAuth configuration. Make sure you have a properly formatted oAuth.properties file. See README for details.");
    return;
}

final String appId = oAuthProperties.getProperty("app.id");
final String[] appScopes = oAuthProperties.getProperty("app.scopes").split(",");
```

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

Cree un nuevo archivo en el directorio **./graphtutorial/src/Main/Java/com/contoso** denominado **Authentication. Java** y agregue el código siguiente.

```java
package com.contoso;
import java.net.MalformedURLException;
import java.util.Set;
import java.util.function.Consumer;

import com.microsoft.aad.msal4j.DeviceCode;
import com.microsoft.aad.msal4j.DeviceCodeFlowParameters;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.PublicClientApplication;

/**
 * Authentication
 */
public class Authentication {

    private static String applicationId;
    // Set authority to allow only organizational accounts
    // Device code flow only supports organizational accounts
    private final static String authority = "https://login.microsoftonline.com/common/";

    public static void initialize(String applicationId) {
        Authentication.applicationId = applicationId;
    }

    public static String getUserAccessToken(String[] scopes) {
        if (applicationId == null) {
            System.out.println("You must initialize Authentication before calling getUserAccessToken");
            return null;
        }

        Set<String> scopeSet = Set.of(scopes);

        PublicClientApplication app;
        try {
            // Build the MSAL application object with
            // app ID and authority
            app = PublicClientApplication.builder(applicationId)
                .authority(authority)
                .build();
        } catch (MalformedURLException e) {
            return null;
        }

        // Create consumer to receive the DeviceCode object
        // This method gets executed during the flow and provides
        // the URL the user logs into and the device code to enter
        Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) -> {
            // Print the login information to the console
            System.out.println(deviceCode.message());
        };

        // Request a token, passing the requested permission scopes
        IAuthenticationResult result = app.acquireToken(
            DeviceCodeFlowParameters
                .builder(scopeSet, deviceCodeConsumer)
                .build()
        ).exceptionally(ex -> {
            System.out.println("Unable to authenticate - " + ex.getMessage());
            return null;
        }).join();

        if (result != null) {
            return result.accessToken();
        }

        return null;
    }
}
```

En **app. Java**, agregue el siguiente código justo antes de `Scanner input = new Scanner(System.in);` la línea para obtener un token de acceso.

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

A continuación, agregue la siguiente línea `// Display access token` después del comentario.

```java
System.out.println("Access token: " + accessToken);
```

Compile y ejecute la aplicación. La aplicación muestra una dirección URL y un código de dispositivo.

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

Abra un explorador y vaya a la dirección URL que se muestra. Escriba el código proporcionado e inicie sesión. Una vez finalizado, vuelva a la aplicación y elija el **1. Muestra** la opción de token de acceso para mostrar el token de acceso.
