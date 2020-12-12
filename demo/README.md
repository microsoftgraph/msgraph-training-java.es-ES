---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661056"
---
# <a name="how-to-run-the-completed-project"></a>Cómo ejecutar el proyecto completado

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:

- El [Kit de desarrollo de Java se (JDK)](https://java.com/en/download/faq/develop.xml) y [Gradle](https://gradle.org/) se instalan en el equipo de desarrollo. Si no tiene el JDK o Gradle, visite los vínculos anteriores para las opciones de descarga. (**Nota:** este tutorial se ha escrito con OpenJDK versión 14.0.0.36 y Gradle 6.7.1. Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.
- Una cuenta profesional o educativa de Microsoft.

Si no tiene una cuenta de Microsoft, puede [suscribirse al programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registro de una aplicación web con el centro de administración de Azure Active Directory

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com). Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.

    ![Una captura de pantalla de los registros de la aplicación ](/tutorial/images/aad-portal-app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **Registrar una aplicación**, establezca los valores siguientes.

    - Establezca **Nombre** como `Java Graph Tutorial`.
    - Establezca los **tipos de cuenta admitidos** **en las cuentas de cualquier directorio de la organización**.
    - Deje **URI de redireccionamiento** vacía.

    ![Captura de pantalla de la página registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. Elija **Registrar**. En la página **tutorial de Java Graph** , copie el valor del **identificador de la aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. Seleccione el vínculo **Agregar un URI de redireccionamiento** . En la página **URI de redireccionamiento** , busque la sección **URI de redireccionamiento sugeridos para clientes públicos (móvil, escritorio)** . Seleccione el `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Captura de pantalla de la página URI de redireccionamiento](/tutorial/images/aad-redirect-uris.png)

1. Busque la sección **tipo de cliente predeterminado** y cambie la opción **tratar aplicación como un cliente público** cambie a **sí** y, a continuación, elija **Guardar**.

    ![Captura de pantalla de la sección tipo de cliente predeterminado](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Configuración del ejemplo

1. Cambie el nombre del `oAuth.properties.example` archivo a `oAuth.properties` .
1. Edite el `oAuth.properties` archivo y realice los cambios siguientes.
    1. Reemplace `YOUR_APP_ID_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.

## <a name="build-and-run-the-sample"></a>Compilar y ejecutar el ejemplo

En la interfaz de línea de comandos (CLI), navegue hasta el directorio del proyecto y ejecute los siguientes comandos.

```Shell
./gradlew --console plain run
```
