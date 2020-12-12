---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661152"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d479e-101">En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="d479e-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="d479e-102">Abra **./graphtutorial/src/Main/Java/graphtutorial/Graph.Java** y agregue la siguiente función a la clase **Graph** .</span><span class="sxs-lookup"><span data-stu-id="d479e-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="d479e-103">Abra **./graphtutorial/src/Main/Java/graphtutorial/app.Java** y agregue la siguiente función a la clase **App** .</span><span class="sxs-lookup"><span data-stu-id="d479e-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="d479e-104">Esta función solicita al usuario el asunto, los asistentes, el inicio, el final y el cuerpo y, a continuación, usa esos valores para llamar `Graph.createEvent` .</span><span class="sxs-lookup"><span data-stu-id="d479e-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="d479e-105">Agregue lo siguiente justo después del `// Create a new event` Comentario en la `Main` función.</span><span class="sxs-lookup"><span data-stu-id="d479e-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="d479e-106">Guarde todos los cambios y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d479e-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="d479e-107">Seleccione la opción **Agregar un evento** .</span><span class="sxs-lookup"><span data-stu-id="d479e-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="d479e-108">Responda a los mensajes para crear un evento nuevo en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="d479e-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
