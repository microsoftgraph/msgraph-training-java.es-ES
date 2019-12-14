---
ms.openlocfilehash: 4e62f08217ab00427218cd1815d66d80fd2802de
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018862"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="52d50-101">En este ejercicio, creará una nueva aplicación de Azure AD con el centro de administración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="52d50-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="52d50-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com) e inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="52d50-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="52d50-103">Seleccione **Azure Active Directory** en el panel de navegación de la izquierda y, después, seleccione **registros de aplicaciones** en **administrar**.</span><span class="sxs-lookup"><span data-stu-id="52d50-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="52d50-104">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="52d50-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="52d50-105">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="52d50-105">Select **New registration**.</span></span> <span data-ttu-id="52d50-106">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="52d50-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="52d50-107">Establezca **Nombre** como `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="52d50-107">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="52d50-108">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="52d50-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="52d50-109">Deje **URI de redireccionamiento** vacía.</span><span class="sxs-lookup"><span data-stu-id="52d50-109">Leave **Redirect URI** empty.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](./images/aad-register-an-app.png)

1. <span data-ttu-id="52d50-111">Elija **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="52d50-111">Choose **Register**.</span></span> <span data-ttu-id="52d50-112">En la página **tutorial de Java Graph** , copie el valor del **identificador de la aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="52d50-112">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. <span data-ttu-id="52d50-114">Seleccione el vínculo **Agregar un URI de redireccionamiento** .</span><span class="sxs-lookup"><span data-stu-id="52d50-114">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="52d50-115">En la página **URI de redireccionamiento** , busque la sección **URI de redireccionamiento sugeridos para clientes públicos (móvil, escritorio)** .</span><span class="sxs-lookup"><span data-stu-id="52d50-115">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="52d50-116">Seleccione el `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="52d50-116">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Captura de pantalla de la página URI de redireccionamiento](./images/aad-redirect-uris.png)

1. <span data-ttu-id="52d50-118">Busque la sección **tipo de cliente predeterminado** y cambie la opción **tratar aplicación como un cliente público** cambie a **sí**y, a continuación, elija **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="52d50-118">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Captura de pantalla de la sección tipo de cliente predeterminado](./images/aad-default-client-type.png)
