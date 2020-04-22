---
ms.openlocfilehash: f77f0518aa22d015cf5e915886f8f24a487e7bab
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612078"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e44d7-101">Este tutorial le enseña a crear una aplicación de consola Java que use la API de Microsoft Graph para recuperar la información de calendario de un usuario.</span><span class="sxs-lookup"><span data-stu-id="e44d7-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="e44d7-102">Si prefiere descargar solo el tutorial completo, puede descargar o clonar el repositorio de [GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="e44d7-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e44d7-103">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e44d7-103">Prerequisites</span></span>

<span data-ttu-id="e44d7-104">Antes de iniciar este tutorial, debe tener instalado el [Kit de desarrollo de Java se (JDK)](https://java.com/en/download/faq/develop.xml) y [Gradle](https://gradle.org/) en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="e44d7-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="e44d7-105">Si no tiene el JDK o el Maven, visite los vínculos anteriores para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="e44d7-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

<span data-ttu-id="e44d7-106">También debe tener una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e44d7-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="e44d7-107">Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="e44d7-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="e44d7-108">Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="e44d7-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="e44d7-109">Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="e44d7-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="e44d7-110">Este tutorial se ha escrito con OpenJDK versión 14.0.0.36 y Gradle 6,3.</span><span class="sxs-lookup"><span data-stu-id="e44d7-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.3.</span></span> <span data-ttu-id="e44d7-111">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="e44d7-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="e44d7-112">Comentarios</span><span class="sxs-lookup"><span data-stu-id="e44d7-112">Feedback</span></span>

<span data-ttu-id="e44d7-113">Envíe sus comentarios sobre este tutorial en el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="e44d7-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
