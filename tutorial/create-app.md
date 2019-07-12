---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634762"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="dad39-101">Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="dad39-101">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="dad39-102">Ejecute el siguiente comando para crear un nuevo proyecto de Maven.</span><span class="sxs-lookup"><span data-stu-id="dad39-102">Run the following command to create a new Maven project.</span></span>

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> <span data-ttu-id="dad39-103">Puede escribir distintos valores para el identificador de grupo (`DgroupId` parámetro) y el identificador del`DartifactId` artefacto (parámetro) que los valores especificados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="dad39-103">You can enter different values for the group ID (`DgroupId` parameter) and artifact ID (`DartifactId` parameter) than the values specified above.</span></span> <span data-ttu-id="dad39-104">El código de ejemplo de este tutorial presupone que se usó `com.contoso` el identificador de grupo.</span><span class="sxs-lookup"><span data-stu-id="dad39-104">The sample code in this tutorial assumes that the group ID `com.contoso` was used.</span></span> <span data-ttu-id="dad39-105">Si usa un valor diferente, asegúrese de reemplazar `com.contoso` en cualquier código de muestra por el identificador de grupo.</span><span class="sxs-lookup"><span data-stu-id="dad39-105">If you use a different value, be sure to replace `com.contoso` in any sample code with your group ID.</span></span>

<span data-ttu-id="dad39-106">Cuando se le pida, confirme la configuración y, a continuación, espere a que se cree el proyecto.</span><span class="sxs-lookup"><span data-stu-id="dad39-106">When prompted, confirm the configuration, then wait for the project to be created.</span></span> <span data-ttu-id="dad39-107">Una vez creado el proyecto, ejecute los comandos siguientes para empaquetar y ejecutar la aplicación en la CLI para comprobar que funciona.</span><span class="sxs-lookup"><span data-stu-id="dad39-107">Once the project is created, verify that it works by running the following commands to package and run the app in your CLI.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

<span data-ttu-id="dad39-108">Si funciona, la aplicación debe obtener resultados `Hello World!`.</span><span class="sxs-lookup"><span data-stu-id="dad39-108">If it works, the app should output `Hello World!`.</span></span> <span data-ttu-id="dad39-109">Antes de continuar, agregue más dependencias adicionales que usará más adelante.</span><span class="sxs-lookup"><span data-stu-id="dad39-109">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="dad39-110">[Biblioteca de autenticación de Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) para autenticar al usuario y adquirir tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="dad39-110">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="dad39-111">[SDK de Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="dad39-111">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="dad39-112">[SLF4J el enlace NOP](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) para suprimir el registro de MSAL.</span><span class="sxs-lookup"><span data-stu-id="dad39-112">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

<span data-ttu-id="dad39-113">Abra **./graphtutorial/POM.XML**.</span><span class="sxs-lookup"><span data-stu-id="dad39-113">Open **./graphtutorial/pom.xml**.</span></span> <span data-ttu-id="dad39-114">Agregue lo siguiente dentro del `<dependencies>` elemento.</span><span class="sxs-lookup"><span data-stu-id="dad39-114">Add the following inside the `<dependencies>` element.</span></span>

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

<span data-ttu-id="dad39-115">La próxima vez que compile el proyecto, Maven descargará esas dependencias.</span><span class="sxs-lookup"><span data-stu-id="dad39-115">The next time you build the project, Maven will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="dad39-116">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="dad39-116">Design the app</span></span>

<span data-ttu-id="dad39-117">Abra el archivo **./graphtutorial/src/Main/Java/com/contoso/app.Java** y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="dad39-117">Open the **./graphtutorial/src/main/java/com/contoso/App.java** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="dad39-118">Esto implementa un menú básico y lee la elección del usuario de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="dad39-118">This implements a basic menu and reads the user's choice from the command line.</span></span>
