---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661070"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7ef61-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ef61-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="7ef61-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7ef61-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="7ef61-103">En este paso, integrará la [biblioteca de autenticación de Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7ef61-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="7ef61-104">Cree un nuevo directorio denominado **graphtutorial** en el directorio **./src/Main/Resources**</span><span class="sxs-lookup"><span data-stu-id="7ef61-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="7ef61-105">Cree un nuevo archivo en el directorio **./src/Main/Resources/graphtutorial** denominado **OAuth. Properties** y agregue el siguiente texto en ese archivo.</span><span class="sxs-lookup"><span data-stu-id="7ef61-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="7ef61-106">Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación que creó en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="7ef61-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="7ef61-107">El valor de `app.scopes` contiene los ámbitos de permisos que necesita la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7ef61-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="7ef61-108">**User. Read** permite que la aplicación tenga acceso al perfil del usuario.</span><span class="sxs-lookup"><span data-stu-id="7ef61-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="7ef61-109">**MailboxSettings. Read** permite que la aplicación obtenga acceso a la configuración del buzón del usuario, incluida la zona horaria configurada por el usuario.</span><span class="sxs-lookup"><span data-stu-id="7ef61-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="7ef61-110">**Calendars. ReadWrite** permite que la aplicación enumere el calendario del usuario y agregue nuevos eventos al calendario.</span><span class="sxs-lookup"><span data-stu-id="7ef61-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7ef61-111">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el archivo **OAuth. Properties** del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7ef61-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="7ef61-112">Abra **app. Java** y agregue las siguientes `import` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="7ef61-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="7ef61-113">Agregue el siguiente código justo antes de la `Scanner input = new Scanner(System.in);` línea para cargar el archivo **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="7ef61-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="7ef61-114">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="7ef61-114">Implement sign-in</span></span>

1. <span data-ttu-id="7ef61-115">Cree un nuevo archivo en el directorio **./graphtutorial/src/Main/Java/graphtutorial** denominado **Authentication. Java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="7ef61-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="7ef61-116">En **app. Java**, agregue el siguiente código justo antes de la `Scanner input = new Scanner(System.in);` línea para obtener un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="7ef61-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="7ef61-117">Agregue la siguiente línea después del `// Display access token` Comentario.</span><span class="sxs-lookup"><span data-stu-id="7ef61-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="7ef61-118">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7ef61-118">Run the app.</span></span> <span data-ttu-id="7ef61-119">La aplicación muestra una dirección URL y un código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="7ef61-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="7ef61-120">Abra un explorador y vaya a la dirección URL que se muestra.</span><span class="sxs-lookup"><span data-stu-id="7ef61-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="7ef61-121">Escriba el código proporcionado e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="7ef61-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="7ef61-122">Una vez finalizado, vuelva a la aplicación y elija el **1. Muestra** la opción de token de acceso para mostrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="7ef61-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="7ef61-123">Los tokens de acceso para las cuentas de Microsoft profesional o educativa pueden analizarse con fines de solución de problemas en [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="7ef61-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="7ef61-124">Los tokens de acceso para las cuentas personales de Microsoft usan un formato propio y no se pueden analizar.</span><span class="sxs-lookup"><span data-stu-id="7ef61-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
