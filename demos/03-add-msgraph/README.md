---
ms.openlocfilehash: a890c296f4269b674356118463578c9b79001b0e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634720"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="71e53-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="71e53-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71e53-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="71e53-102">Prerequisites</span></span>

<span data-ttu-id="71e53-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="71e53-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="71e53-104">El [Kit de desarrollo de Java se (JDK)](https://java.com/en/download/faq/develop.xml) y [Maven](https://maven.apache.org/) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="71e53-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="71e53-105">Si no tiene el JDK o el Maven, visite los vínculos anteriores para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="71e53-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span> <span data-ttu-id="71e53-106">(**Nota:** este tutorial se ha escrito con OpenJDK versión 12.0.1 y Maven 3.6.1.</span><span class="sxs-lookup"><span data-stu-id="71e53-106">(**Note:** This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="71e53-107">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="71e53-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="71e53-108">Una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="71e53-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="71e53-109">Si no tiene una cuenta de Microsoft, puede suscribirse al [programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="71e53-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="71e53-110">Registro de una aplicación web con el centro de administración de Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="71e53-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="71e53-111">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="71e53-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="71e53-112">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="71e53-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="71e53-113">Seleccione **Azure Active Directory** en el panel de navegación de la izquierda y, después, seleccione **registros de aplicaciones** en **administrar**.</span><span class="sxs-lookup"><span data-stu-id="71e53-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="71e53-114">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="71e53-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="71e53-115">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="71e53-115">Select **New registration**.</span></span> <span data-ttu-id="71e53-116">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="71e53-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="71e53-117">Establezca **Nombre** como `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="71e53-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="71e53-118">Establezca los **tipos de cuenta admitidos** **en las cuentas de cualquier directorio de la organización**.</span><span class="sxs-lookup"><span data-stu-id="71e53-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="71e53-119">Deje **URI de redireccionamiento** vacía.</span><span class="sxs-lookup"><span data-stu-id="71e53-119">Leave **Redirect URI** empty.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="71e53-121">Elija **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="71e53-121">Choose **Register**.</span></span> <span data-ttu-id="71e53-122">En la página **tutorial de Java Graph** , copie el valor del **identificador de la aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="71e53-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="71e53-124">Seleccione el vínculo **Agregar un URI de** redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="71e53-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="71e53-125">En la página **URI** de redireccionamiento, busque la sección **URI de redireccionamiento sugeridos para clientes públicos (móvil, escritorio)** .</span><span class="sxs-lookup"><span data-stu-id="71e53-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="71e53-126">Seleccione el `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="71e53-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Captura de pantalla de la página URI de redireccionamiento](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="71e53-128">Busque la sección **tipo de cliente predeterminado** y cambie la opción **tratar aplicación como un cliente público** cambie a **sí**y, a continuación, elija **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="71e53-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Captura de pantalla de la sección tipo de cliente predeterminado](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="71e53-130">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="71e53-130">Configure the sample</span></span>

1. <span data-ttu-id="71e53-131">Cambie el nombre `oAuth.properties.example` del archivo `oAuth.properties`a.</span><span class="sxs-lookup"><span data-stu-id="71e53-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="71e53-132">Edite `oAuth.properties` el archivo y realice los cambios siguientes.</span><span class="sxs-lookup"><span data-stu-id="71e53-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="71e53-133">Reemplace `YOUR_APP_ID_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="71e53-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="71e53-134">Compilar y ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="71e53-134">Build and run the sample</span></span>

<span data-ttu-id="71e53-135">En la interfaz de línea de comandos (CLI), navegue hasta el directorio del proyecto y ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="71e53-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```
