---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695789"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="bb9cd-101">En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="bb9cd-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="bb9cd-102">Abra **./graphtutorial/src/main/java/graphtutorial/Graph.java** y agregue la siguiente función a la **clase Graph.**</span><span class="sxs-lookup"><span data-stu-id="bb9cd-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="bb9cd-103">Abra **./graphtutorial/src/main/java/graphtutorial/App.java** y agregue la siguiente función a la **clase App.**</span><span class="sxs-lookup"><span data-stu-id="bb9cd-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="bb9cd-104">Esta función solicita al usuario asunto, asistentes, inicio, fin y cuerpo y, a continuación, usa esos valores para llamar a `Graph.createEvent` .</span><span class="sxs-lookup"><span data-stu-id="bb9cd-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="bb9cd-105">Agregue lo siguiente justo después del `// Create a new event` comentario en la `Main` función.</span><span class="sxs-lookup"><span data-stu-id="bb9cd-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="bb9cd-106">Guarda todos los cambios y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb9cd-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="bb9cd-107">Elija la **opción Agregar un evento.**</span><span class="sxs-lookup"><span data-stu-id="bb9cd-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="bb9cd-108">Responda a los mensajes para crear un nuevo evento en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="bb9cd-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
