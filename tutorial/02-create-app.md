---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661084"
---
<!-- markdownlint-disable MD002 MD041 -->

En esta sección, creará una aplicación básica de consola Java.

1. Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto. Ejecute el siguiente comando para crear un nuevo proyecto Gradle.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. Una vez creado el proyecto, compruebe que funciona ejecutando el siguiente comando para ejecutar la aplicación en la CLI.

    ```Shell
    ./gradlew --console plain run
    ```

    Si funciona, la aplicación debe obtener resultados `Hello World.` .

## <a name="install-dependencies"></a>Instalar dependencias

Antes de continuar, agregue más dependencias adicionales que usará más adelante.

- [Biblioteca de autenticación de Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) para autenticar al usuario y adquirir tokens de acceso.
- [SDK de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.
- [SLF4J el enlace NOP](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) para suprimir el registro de MSAL.

1. Abra **./Build.Gradle**. Actualice la `dependencies` sección para agregar esas dependencias.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. Agregue lo siguiente al final de **./Build.Gradle**.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

La próxima vez que genere el proyecto, Gradle descargará esas dependencias.

## <a name="design-the-app"></a>Diseñar la aplicación

1. Abra el archivo **./src/Main/Java/graphtutorial/app.Java** y reemplace el contenido por lo siguiente.

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

    Esto implementa un menú básico y lee la elección del usuario de la línea de comandos.
