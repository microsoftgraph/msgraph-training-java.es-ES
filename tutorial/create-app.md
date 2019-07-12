---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634762"
---
<!-- markdownlint-disable MD002 MD041 -->

Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto. Ejecute el siguiente comando para crear un nuevo proyecto de Maven.

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> Puede escribir distintos valores para el identificador de grupo (`DgroupId` parámetro) y el identificador del`DartifactId` artefacto (parámetro) que los valores especificados anteriormente. El código de ejemplo de este tutorial presupone que se usó `com.contoso` el identificador de grupo. Si usa un valor diferente, asegúrese de reemplazar `com.contoso` en cualquier código de muestra por el identificador de grupo.

Cuando se le pida, confirme la configuración y, a continuación, espere a que se cree el proyecto. Una vez creado el proyecto, ejecute los comandos siguientes para empaquetar y ejecutar la aplicación en la CLI para comprobar que funciona.

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

Si funciona, la aplicación debe obtener resultados `Hello World!`. Antes de continuar, agregue más dependencias adicionales que usará más adelante.

- [Biblioteca de autenticación de Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) para autenticar al usuario y adquirir tokens de acceso.
- [SDK de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.
- [SLF4J el enlace NOP](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) para suprimir el registro de MSAL.

Abra **./graphtutorial/POM.XML**. Agregue lo siguiente dentro del `<dependencies>` elemento.

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-nop</artifactId>
  <version>1.8.0-beta4</version>
</dependency>

<dependency>
  <groupId>com.microsoft.graph</groupId>
  <artifactId>microsoft-graph</artifactId>
  <version>1.4.0</version>
</dependency>

<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>msal4j</artifactId>
  <version>0.4.0-preview</version>
</dependency>
```

La próxima vez que compile el proyecto, Maven descargará esas dependencias.

## <a name="design-the-app"></a>Diseñar la aplicación

Abra el archivo **./graphtutorial/src/Main/Java/com/contoso/app.Java** y reemplace el contenido por lo siguiente.

```java
package com.contoso;

import java.util.InputMismatchException;
import java.util.Scanner;

/**
 * Graph Tutorial
 *
 */
public class App {
    public static void main(String[] args) {
        System.out.println("Java Graph Tutorial");
        System.out.println();

        Scanner input = new Scanner(System.in);

        int choice = -1;

        while (choice != 0) {
            System.out.println("Please choose one of the following options:");
            System.out.println("0. Exit");
            System.out.println("1. Display access token");
            System.out.println("2. List calendar events");

            try {
                choice = input.nextInt();
            } catch (InputMismatchException ex) {
                // Skip over non-integer input
                input.nextLine();
            }

            // Process user choice
            switch(choice) {
                case 0:
                    // Exit the program
                    System.out.println("Goodbye...");
                    break;
                case 1:
                    // Display access token
                case 2:
                    // List the calendar
                    break;
                default:
                    System.out.println("Invalid choice");
            }
        }

        input.close();
    }
}
```

Esto implementa un menú básico y lee la elección del usuario de la línea de comandos.
