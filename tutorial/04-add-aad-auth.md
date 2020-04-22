---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612064"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="07732-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07732-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="07732-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="07732-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="07732-103">En este paso, integrará la [biblioteca de autenticación de Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="07732-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="07732-104">Cree un nuevo directorio denominado **graphtutorial** en el directorio **./src/Main/Resources**</span><span class="sxs-lookup"><span data-stu-id="07732-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="07732-105">Cree un nuevo archivo en el directorio **./src/Main/Resources/graphtutorial** denominado **OAuth. Properties**y agregue el siguiente texto en ese archivo.</span><span class="sxs-lookup"><span data-stu-id="07732-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="07732-106">Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación que creó en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="07732-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="07732-107">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el archivo **OAuth. Properties** del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="07732-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="07732-108">Abra **app. Java** y agregue las siguientes `import` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="07732-108">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="07732-109">Agregue el siguiente código justo antes de `Scanner input = new Scanner(System.in);` la línea para cargar el archivo **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="07732-109">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="07732-110">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="07732-110">Implement sign-in</span></span>

1. <span data-ttu-id="07732-111">Cree un nuevo archivo en el directorio **./graphtutorial/src/Main/Java/graphtutorial** denominado **Authentication. Java** y agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="07732-111">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="07732-112">En **app. Java**, agregue el siguiente código justo antes de `Scanner input = new Scanner(System.in);` la línea para obtener un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="07732-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="07732-113">Agregue la siguiente línea después del `// Display access token` comentario.</span><span class="sxs-lookup"><span data-stu-id="07732-113">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="07732-114">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="07732-114">Run the app.</span></span> <span data-ttu-id="07732-115">La aplicación muestra una dirección URL y un código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="07732-115">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="07732-116">Abra un explorador y vaya a la dirección URL que se muestra.</span><span class="sxs-lookup"><span data-stu-id="07732-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="07732-117">Escriba el código proporcionado e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="07732-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="07732-118">Una vez finalizado, vuelva a la aplicación y elija el **1. Muestra** la opción de token de acceso para mostrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="07732-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
