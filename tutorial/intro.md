---
ms.openlocfilehash: 3eec5a727d21e481525e2a2892e33a0a30a9bb25
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634783"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f5a74-101">Este tutorial le enseña a crear una aplicación de consola Java que use la API de Microsoft Graph para recuperar la información de calendario de un usuario.</span><span class="sxs-lookup"><span data-stu-id="f5a74-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="f5a74-102">Si prefiere descargar solo el tutorial completo, puede descargar o clonar el repositorio de [GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="f5a74-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5a74-103">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f5a74-103">Prerequisites</span></span>

<span data-ttu-id="f5a74-104">Antes de iniciar este tutorial, debe tener instalado el [Kit de desarrollo de Java se (JDK)](https://java.com/en/download/faq/develop.xml) y [Maven](https://maven.apache.org/) en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f5a74-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="f5a74-105">Si no tiene el JDK o el Maven, visite los vínculos anteriores para las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="f5a74-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="f5a74-106">Este tutorial se ha escrito con OpenJDK versión 12.0.1 y Maven 3.6.1.</span><span class="sxs-lookup"><span data-stu-id="f5a74-106">This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="f5a74-107">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="f5a74-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="f5a74-108">Comentarios</span><span class="sxs-lookup"><span data-stu-id="f5a74-108">Feedback</span></span>

<span data-ttu-id="f5a74-109">Envíe sus comentarios sobre este tutorial en el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="f5a74-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
