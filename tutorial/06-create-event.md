---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695789"
---
<!-- markdownlint-disable MD002 MD041 -->

En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.

1. Abra **./graphtutorial/src/main/java/graphtutorial/Graph.java** y agregue la siguiente función a la **clase Graph.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Abra **./graphtutorial/src/main/java/graphtutorial/App.java** y agregue la siguiente función a la **clase App.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Esta función solicita al usuario asunto, asistentes, inicio, fin y cuerpo y, a continuación, usa esos valores para llamar a `Graph.createEvent` .

1. Agregue lo siguiente justo después del `// Create a new event` comentario en la `Main` función.

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. Guarda todos los cambios y ejecuta la aplicación. Elija la **opción Agregar un evento.** Responda a los mensajes para crear un nuevo evento en el calendario del usuario.
