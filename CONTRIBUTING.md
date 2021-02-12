---
ms.openlocfilehash: fbd3e506358aa4be60dfe3891b50085691f7443a
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661147"
---
# <a name="contributing-to-microsoft-graph-training-repositories"></a><span data-ttu-id="62f1b-101">Contribuir a repositorios de aprendizaje de Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="62f1b-101">Contributing to Microsoft Graph training repositories</span></span>

<span data-ttu-id="62f1b-102">Gracias por colaborar en este proyecto.</span><span class="sxs-lookup"><span data-stu-id="62f1b-102">Thank you for contributing to this project!</span></span> <span data-ttu-id="62f1b-103">Antes de enviar la solicitud de incorporación deble, asegúrese de tener en cuenta lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="62f1b-103">Before submitting your pull request, be sure to consider the following.</span></span>

## <a name="overview"></a><span data-ttu-id="62f1b-104">Información general</span><span class="sxs-lookup"><span data-stu-id="62f1b-104">Overview</span></span>

<span data-ttu-id="62f1b-105">El código de este repositorio sirve para tres propósitos:</span><span class="sxs-lookup"><span data-stu-id="62f1b-105">The code in this repository serves three purposes:</span></span>

- <span data-ttu-id="62f1b-106">Los archivos de Markdown en la carpeta [tutorial](/tutorial) se publican como un tutorial en la página de [tutoriales de Microsoft Graph](https://docs.microsoft.com/graph/tutorials) .</span><span class="sxs-lookup"><span data-stu-id="62f1b-106">The Markdown files in the [tutorial](/tutorial) folder are published as a tutorial on the [Microsoft Graph tutorials](https://docs.microsoft.com/graph/tutorials) page.</span></span>
- <span data-ttu-id="62f1b-107">El proyecto de ejemplo de la carpeta [Demo](/demo) es el origen de un [Inicio rápido de Microsoft Graph](https://developer.microsoft.com/graph/quick-start). \* *\** _</span><span class="sxs-lookup"><span data-stu-id="62f1b-107">The sample project in the [demo](/demo) folder is the source for a [Microsoft Graph quick start](https://developer.microsoft.com/graph/quick-start).\**\** _</span></span>
- <span data-ttu-id="62f1b-108">El proyecto de ejemplo de la carpeta demo también se puede descargar directamente desde GitHub y debe ejecutarse tal cual después de una configuración sencilla.</span><span class="sxs-lookup"><span data-stu-id="62f1b-108">The sample project in the demo folder is also downloadable directly from GitHub and should run as-is after some simple configuration.</span></span>

> <span data-ttu-id="62f1b-109">_*\**_ No todos los repositorios de formación están disponibles como inicios rápidos (todavía).</span><span class="sxs-lookup"><span data-stu-id="62f1b-109">_*\**_ Not all training repositories are available as quick starts (yet).</span></span>

<span data-ttu-id="62f1b-110">Es importante tener esto en cuenta, ya que los cambios en un solo punto _may \* requieren cambios en otro para mantener sincronizados los elementos. Whereever posible, los archivos de Markdown hacen referencia a los archivos de código fuente directamente (mediante una `:::code` Sintaxis personalizada), de modo que la actualización del código en el origen actualice automáticamente el código en Markdown.</span><span class="sxs-lookup"><span data-stu-id="62f1b-110">This is important to keep in mind, because changes in one place _may\* require changes in another, to keep things in sync. Whereever possible, the Markdown files refer to the source code files directly (using a custom `:::code` syntax), so that updating code in source will automatically update the code in Markdown.</span></span>

## <a name="updating-code"></a><span data-ttu-id="62f1b-111">Actualizar código</span><span class="sxs-lookup"><span data-stu-id="62f1b-111">Updating code</span></span>

<span data-ttu-id="62f1b-112">La `:::code` sintaxis usada en Markdown depende de comentarios específicos en el archivo de código fuente.</span><span class="sxs-lookup"><span data-stu-id="62f1b-112">The `:::code` syntax used in Markdown depends on specific comments in the source code file.</span></span> <span data-ttu-id="62f1b-113">Estos comentarios son similares a los siguientes:</span><span class="sxs-lookup"><span data-stu-id="62f1b-113">These comments look like the following:</span></span>

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

<span data-ttu-id="62f1b-114">Si actualiza el código entre estos comentarios de "marcador", los archivos de Markdown obtendrán automáticamente esos cambios cuando se publiquen en el sitio de documentación de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="62f1b-114">If you update code between these "marker" comments, the Markdown files will automatically get those changes when published to the Microsoft Graph documentation site.</span></span> <span data-ttu-id="62f1b-115">Si actualiza el código fuera de esos comentarios, es muy probable que necesite actualizar el Markdown correspondiente.</span><span class="sxs-lookup"><span data-stu-id="62f1b-115">If you update code outside of those comments, it's very likely that you'll need to update the corresponding Markdown.</span></span>

## <a name="adding-features"></a><span data-ttu-id="62f1b-116">Adición de características</span><span class="sxs-lookup"><span data-stu-id="62f1b-116">Adding features</span></span>

<span data-ttu-id="62f1b-117">Si bien se agradece el entusiasmo, no envíe solicitudes de incorporación de inserción para agregar nuevas características al ejemplo.</span><span class="sxs-lookup"><span data-stu-id="62f1b-117">While the enthusiasm is appreciated, please don't send pull requests to add new features to the sample.</span></span> <span data-ttu-id="62f1b-118">Como este repositorio es principalmente un tutorial de creación de la primera aplicación, el conjunto de características es limitado, por diseño.</span><span class="sxs-lookup"><span data-stu-id="62f1b-118">Because this repository is primarily a "build your first app" tutorial, the feature set is limited, by design.</span></span>

## <a name="submitting-pull-requests"></a><span data-ttu-id="62f1b-119">Enviar solicitudes de inserción</span><span class="sxs-lookup"><span data-stu-id="62f1b-119">Submitting pull requests</span></span>

<span data-ttu-id="62f1b-120">Envíe todas las solicitudes de incorporación de `master` conmutación a la sucursal.</span><span class="sxs-lookup"><span data-stu-id="62f1b-120">Please submit all pull requests to the `master` branch.</span></span>

## <a name="when-do-changes-get-published"></a><span data-ttu-id="62f1b-121">¿Cuándo se publican los cambios?</span><span class="sxs-lookup"><span data-stu-id="62f1b-121">When do changes get published?</span></span>

<span data-ttu-id="62f1b-122">La publicación de actualizaciones en el sitio de [tutoriales de Microsoft Graph](https://docs.microsoft.com/graph/tutorials) no es automática.</span><span class="sxs-lookup"><span data-stu-id="62f1b-122">Publishing of updates to the [Microsoft Graph tutorials](https://docs.microsoft.com/graph/tutorials) site is not automatic.</span></span> <span data-ttu-id="62f1b-123">En primer lugar, los cambios deben promoverse a la `live` rama y, a continuación, los administradores del sitio deben desencadenar una compilación.</span><span class="sxs-lookup"><span data-stu-id="62f1b-123">Changes must first be promoted to the `live` branch, then a build must be triggered by the site admins.</span></span> <span data-ttu-id="62f1b-124">Normalmente, esto se realiza en función de las necesidades.</span><span class="sxs-lookup"><span data-stu-id="62f1b-124">This is typically done on an "as-needed" basis.</span></span>

## <a name="code-of-conduct"></a><span data-ttu-id="62f1b-125">Código de conducta</span><span class="sxs-lookup"><span data-stu-id="62f1b-125">Code of conduct</span></span>

<span data-ttu-id="62f1b-p106">Este proyecto ha adoptado el [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Código de conducta de código abierto de Microsoft). Para obtener más información, consulte las [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) (Preguntas más frecuentes del código de conducta) o póngase en contacto con [opencode@microsoft.com](mailto:opencode@microsoft.com) con otras preguntas o comentarios.</span><span class="sxs-lookup"><span data-stu-id="62f1b-p106">This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.</span></span>