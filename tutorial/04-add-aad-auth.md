---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661070"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la [biblioteca de autenticación de Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) en la aplicación.

1. Cree un nuevo directorio denominado **graphtutorial** en el directorio **./src/Main/Resources**

1. Cree un nuevo archivo en el directorio **./src/Main/Resources/graphtutorial** denominado **OAuth. Properties** y agregue el siguiente texto en ese archivo. Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación que creó en Azure portal.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    El valor de `app.scopes` contiene los ámbitos de permisos que necesita la aplicación.

    - **User. Read** permite que la aplicación tenga acceso al perfil del usuario.
    - **MailboxSettings. Read** permite que la aplicación obtenga acceso a la configuración del buzón del usuario, incluida la zona horaria configurada por el usuario.
    - **Calendars. ReadWrite** permite que la aplicación enumere el calendario del usuario y agregue nuevos eventos al calendario.

    > [!IMPORTANT]
    > Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el archivo **OAuth. Properties** del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.

1. Abra **app. Java** y agregue las siguientes `import` instrucciones.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Agregue el siguiente código justo antes de la `Scanner input = new Scanner(System.in);` línea para cargar el archivo **OAuth. Properties** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

1. Cree un nuevo archivo en el directorio **./graphtutorial/src/Main/Java/graphtutorial** denominado **Authentication. Java** y agregue el código siguiente.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. En **app. Java**, agregue el siguiente código justo antes de la `Scanner input = new Scanner(System.in);` línea para obtener un token de acceso.

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. Agregue la siguiente línea después del `// Display access token` Comentario.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Ejecute la aplicación. La aplicación muestra una dirección URL y un código de dispositivo.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Abra un explorador y vaya a la dirección URL que se muestra. Escriba el código proporcionado e inicie sesión. Una vez finalizado, vuelva a la aplicación y elija el **1. Muestra** la opción de token de acceso para mostrar el token de acceso.

> [!TIP]
> Los tokens de acceso para las cuentas de Microsoft profesional o educativa pueden analizarse con fines de solución de problemas en [https://jwt.ms](https://jwt.ms) . Los tokens de acceso para las cuentas personales de Microsoft usan un formato propio y no se pueden analizar.
