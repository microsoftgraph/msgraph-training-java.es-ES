---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661084"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="34fa7-101">En esta sección, creará una aplicación básica de consola Java.</span><span class="sxs-lookup"><span data-stu-id="34fa7-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="34fa7-102">Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="34fa7-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="34fa7-103">Ejecute el siguiente comando para crear un nuevo proyecto Gradle.</span><span class="sxs-lookup"><span data-stu-id="34fa7-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="34fa7-104">Una vez creado el proyecto, compruebe que funciona ejecutando el siguiente comando para ejecutar la aplicación en la CLI.</span><span class="sxs-lookup"><span data-stu-id="34fa7-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="34fa7-105">Si funciona, la aplicación debe obtener resultados `Hello World.` .</span><span class="sxs-lookup"><span data-stu-id="34fa7-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="34fa7-106">Instalar dependencias</span><span class="sxs-lookup"><span data-stu-id="34fa7-106">Install dependencies</span></span>

<span data-ttu-id="34fa7-107">Antes de continuar, agregue más dependencias adicionales que usará más adelante.</span><span class="sxs-lookup"><span data-stu-id="34fa7-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="34fa7-108">[Biblioteca de autenticación de Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) para autenticar al usuario y adquirir tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="34fa7-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="34fa7-109">[SDK de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="34fa7-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="34fa7-110">[SLF4J el enlace NOP](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) para suprimir el registro de MSAL.</span><span class="sxs-lookup"><span data-stu-id="34fa7-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="34fa7-111">Abra **./Build.Gradle**.</span><span class="sxs-lookup"><span data-stu-id="34fa7-111">Open **./build.gradle**.</span></span> <span data-ttu-id="34fa7-112">Actualice la `dependencies` sección para agregar esas dependencias.</span><span class="sxs-lookup"><span data-stu-id="34fa7-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="34fa7-113">Agregue lo siguiente al final de **./Build.Gradle**.</span><span class="sxs-lookup"><span data-stu-id="34fa7-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="34fa7-114">La próxima vez que genere el proyecto, Gradle descargará esas dependencias.</span><span class="sxs-lookup"><span data-stu-id="34fa7-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="34fa7-115">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="34fa7-115">Design the app</span></span>

1. <span data-ttu-id="34fa7-116">Abra el archivo **./src/Main/Java/graphtutorial/app.Java** y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="34fa7-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="34fa7-117">Esto implementa un menú básico y lee la elección del usuario de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="34fa7-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
