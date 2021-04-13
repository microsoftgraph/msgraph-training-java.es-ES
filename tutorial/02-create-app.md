---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695803"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="09476-101">En esta sección, crearás una aplicación básica Java consola.</span><span class="sxs-lookup"><span data-stu-id="09476-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="09476-102">Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="09476-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="09476-103">Ejecute el siguiente comando para crear un nuevo proyecto gradle.</span><span class="sxs-lookup"><span data-stu-id="09476-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="09476-104">Una vez creado el proyecto, compruebe que funciona ejecutando el siguiente comando para ejecutar la aplicación en la CLI.</span><span class="sxs-lookup"><span data-stu-id="09476-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="09476-105">Si funciona, la aplicación debe generar `Hello World.` .</span><span class="sxs-lookup"><span data-stu-id="09476-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="09476-106">Instalar dependencias</span><span class="sxs-lookup"><span data-stu-id="09476-106">Install dependencies</span></span>

<span data-ttu-id="09476-107">Antes de seguir adelante, agrega algunas dependencias adicionales que usarás más adelante.</span><span class="sxs-lookup"><span data-stu-id="09476-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="09476-108">[Biblioteca de cliente de Azure Identity Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) para autenticar al usuario y adquirir tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="09476-108">[Azure Identity client library for Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="09476-109">[Sdk de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="09476-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="09476-110">Abra **./build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="09476-110">Open **./build.gradle**.</span></span> <span data-ttu-id="09476-111">Actualice la `dependencies` sección para agregar esas dependencias.</span><span class="sxs-lookup"><span data-stu-id="09476-111">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. <span data-ttu-id="09476-112">Agregue lo siguiente al final de **./build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="09476-112">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="09476-113">La próxima vez que cree el proyecto, Gradle descargará esas dependencias.</span><span class="sxs-lookup"><span data-stu-id="09476-113">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="09476-114">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="09476-114">Design the app</span></span>

1. <span data-ttu-id="09476-115">Abra el **archivo ./src/main/java/graphtutorial/App.java** y reemplace su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="09476-115">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="09476-116">Esto implementa un menú básico y lee la elección del usuario desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="09476-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
