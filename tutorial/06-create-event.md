---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661152"
---
<!-- markdownlint-disable MD002 MD041 -->

En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.

1. Abra **./graphtutorial/src/Main/Java/graphtutorial/Graph.Java** y agregue la siguiente función a la clase **Graph** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Abra **./graphtutorial/src/Main/Java/graphtutorial/app.Java** y agregue la siguiente función a la clase **App** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Esta función solicita al usuario el asunto, los asistentes, el inicio, el final y el cuerpo y, a continuación, usa esos valores para llamar `Graph.createEvent` .

1. Agregue lo siguiente justo después del `// Create a new event` Comentario en la `Main` función.

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. Guarde todos los cambios y ejecute la aplicación. Seleccione la opción **Agregar un evento** . Responda a los mensajes para crear un evento nuevo en el calendario del usuario.
