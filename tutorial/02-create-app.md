---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695803"
---
<!-- markdownlint-disable MD002 MD041 -->

En esta sección, crearás una aplicación básica Java consola.

1. Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto. Ejecute el siguiente comando para crear un nuevo proyecto gradle.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. Una vez creado el proyecto, compruebe que funciona ejecutando el siguiente comando para ejecutar la aplicación en la CLI.

    ```Shell
    ./gradlew --console plain run
    ```

    Si funciona, la aplicación debe generar `Hello World.` .

## <a name="install-dependencies"></a>Instalar dependencias

Antes de seguir adelante, agrega algunas dependencias adicionales que usarás más adelante.

- [Biblioteca de cliente de Azure Identity Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) para autenticar al usuario y adquirir tokens de acceso.
- [Sdk de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) realizar llamadas a Microsoft Graph.

1. Abra **./build.gradle**. Actualice la `dependencies` sección para agregar esas dependencias.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. Agregue lo siguiente al final de **./build.gradle**.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

La próxima vez que cree el proyecto, Gradle descargará esas dependencias.

## <a name="design-the-app"></a>Diseñar la aplicación

1. Abra el **archivo ./src/main/java/graphtutorial/App.java** y reemplace su contenido por lo siguiente.

    ```java
    package graphtutorial;

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
                System.out.println("2. View this week's calendar");
                System.out.println("3. Add an event");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                }

                input.nextLine();

                // Process user choice
                switch(choice) {
                    case 0:
                        // Exit the program
                        System.out.println("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    case 3:
                        // Create a new event
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    Esto implementa un menú básico y lee la elección del usuario desde la línea de comandos.
