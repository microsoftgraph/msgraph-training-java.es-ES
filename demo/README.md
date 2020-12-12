---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661056"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="21247-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="21247-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21247-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="21247-102">Prerequisites</span></span>

<span data-ttu-id="21247-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="21247-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="21247-104">El [Kit de desarrollo de Java se (JDK)](https://java.com/en/download/faq/develop.xml) y [Gradle](https://gradle.org/) se instalan en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="21247-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="21247-105">Si no tiene el JDK o Gradle, visite los vínculos anteriores para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="21247-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span> <span data-ttu-id="21247-106">(**Nota:** este tutorial se ha escrito con OpenJDK versión 14.0.0.36 y Gradle 6.7.1.</span><span class="sxs-lookup"><span data-stu-id="21247-106">(**Note:** This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="21247-107">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="21247-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="21247-108">Una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="21247-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="21247-109">Si no tiene una cuenta de Microsoft, puede [suscribirse al programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="21247-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="21247-110">Registro de una aplicación web con el centro de administración de Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="21247-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="21247-111">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="21247-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="21247-112">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="21247-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="21247-113">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="21247-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="21247-114">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="21247-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="21247-115">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="21247-115">Select **New registration**.</span></span> <span data-ttu-id="21247-116">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="21247-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="21247-117">Establezca **Nombre** como `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="21247-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="21247-118">Establezca los **tipos de cuenta admitidos** **en las cuentas de cualquier directorio de la organización**.</span><span class="sxs-lookup"><span data-stu-id="21247-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="21247-119">Deje **URI de redireccionamiento** vacía.</span><span class="sxs-lookup"><span data-stu-id="21247-119">Leave **Redirect URI** empty.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="21247-121">Elija **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="21247-121">Choose **Register**.</span></span> <span data-ttu-id="21247-122">En la página **tutorial de Java Graph** , copie el valor del **identificador de la aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="21247-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="21247-124">Seleccione el vínculo **Agregar un URI de redireccionamiento** .</span><span class="sxs-lookup"><span data-stu-id="21247-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="21247-125">En la página **URI de redireccionamiento** , busque la sección **URI de redireccionamiento sugeridos para clientes públicos (móvil, escritorio)** .</span><span class="sxs-lookup"><span data-stu-id="21247-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="21247-126">Seleccione el `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="21247-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Captura de pantalla de la página URI de redireccionamiento](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="21247-128">Busque la sección **tipo de cliente predeterminado** y cambie la opción **tratar aplicación como un cliente público** cambie a **sí** y, a continuación, elija **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="21247-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Captura de pantalla de la sección tipo de cliente predeterminado](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="21247-130">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="21247-130">Configure the sample</span></span>

1. <span data-ttu-id="21247-131">Cambie el nombre del `oAuth.properties.example` archivo a `oAuth.properties` .</span><span class="sxs-lookup"><span data-stu-id="21247-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="21247-132">Edite el `oAuth.properties` archivo y realice los cambios siguientes.</span><span class="sxs-lookup"><span data-stu-id="21247-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="21247-133">Reemplace `YOUR_APP_ID_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="21247-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="21247-134">Compilar y ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="21247-134">Build and run the sample</span></span>

<span data-ttu-id="21247-135">En la interfaz de línea de comandos (CLI), navegue hasta el directorio del proyecto y ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="21247-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
./gradlew --console plain run
```
