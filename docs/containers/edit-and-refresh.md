---
title: Debugging apps in a local Docker container | Microsoft Docs
description: Learn how to modify an app that is running in a local Docker container, refresh the container via Edit and Refresh and set debugging breakpoints
author: ghogen
manager: jillfra
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.topic: conceptual
ms.workload: multiple
ms.date: 03/05/2019
ms.author: ghogen
ms.technology: vs-azure
---
# Debugging apps in a local Docker container

Visual Studio provides a consistent way to develop in a Docker container and validate your application locally.
You don't have to restart the container each time you make a code change.
This article illustrates how to use the "Edit and Refresh" feature to start an ASP.NET Core Web app in a local Docker container,
make any necessary changes, and then refresh the browser to see those changes.
This article also shows you how to set breakpoints for debugging.

## Prerequisites

The following tools must be installed.

::: moniker range="vs-2017"

* [Visual Studio 2017](https://visualstudio.microsoft.com/vs/older-downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=vs+2017+download) with the Web Development workload installed.

::: moniker-end

::: moniker range="vs-2019"

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the Web Development workload installed.

::: moniker-end

To run Docker containers locally, you'll need a local docker client.
You can use the [Docker Toolbox](https://www.docker.com/products/docker-toolbox), which requires Hyper-V to be disabled,
or you can use [Docker for Windows](https://www.docker.com/get-docker), which uses Hyper-V, and requires Windows 10.

If using Docker Toolbox, you'll need to configure the Docker client.

Docker containers are available for .NET Framework and .NET Core projects. Let's look at two example, first a .NET Core web app, and second, a .NET Framework console app.

## Create a web app

::: moniker range="vs-2017"
[!INCLUDE [create-aspnet5-app](../azure/includes/create-aspnet5-app.md)]
::: moniker-end
::: moniker range=">= vs-2019"
[!INCLUDE [create-aspnet5-app-2019](../azure/includes/vs-2019/create-aspnet5-app-2019.md)
::: moniker-end

### Edit your code and refresh

To quickly iterate changes, you can start your application within a container, and continue to make changes, viewing them as you would with IIS Express.

1. Set the Solution Configuration to `Debug` and press Ctrl+F5 to build your docker image and run it locally.

    Once the container image has been built and is running in a Docker container, Visual Studio launches the web app in your default browser.

2. Go to the Index page, which is where we're going to make our changes.
3. Return to Visual Studio and open `Index.cshtml`.
4. Add the following HTML content to the end of the file and save the changes.

    ```html
    <h1>Hello from a Docker Container!</h1>
    ```

5. Viewing the output window, when the .NET build is completed and you see these lines, switch back to your browser and refresh the page.

   ```output
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```

6. Your changes have been applied!

### Debug with breakpoints

Often, changes will need further inspection, leveraging the debugging features of Visual Studio.

1. Return to Visual Studio and open `Index.cshtml.cs`
2. Replace the contents of the `OnGet` method with the following:

   ```csharp
       ViewData["Message"] = "Your application description page from within a Container";
   ```

3. Set a breakpoint to the left of the code line.
4. Hit F5 to start debugging and hit the breakpoint.
5. Switch to Visual Studio to view the breakpoint, inspect values, and so on.

   ![Breakpoint](media/edit-and-refresh/breakpoint.png)

## Create a .NET Framework Console App

When using .NET Framework console app projects, the option to add Docker support without orchestration isn't supported. You can still use the following procedure, even if you are only using a single Docker project.

1. Create a new .NET Framework Console app project.
1. In **Solution Explorer**, right-click on the project node, and choose **Add** > **Container Orchestration Support**.  In the dialog that appears, choose **Docker Compose**. A Dockerfile is added to your project, and a Docker Compose project is added with associated support files.

### Debug with breakpoints

1. In **Solution Explorer**, open `Program.cs`.
2. Replace the contents of the `Main` method with the following:

   ```csharp
       System.Console.WriteLine("Hello, world!");
   ```

3. Set a breakpoint to the left of the code line.
4. Hit F5 to start debugging and hit the breakpoint.
5. Switch to Visual Studio to view the breakpoint, inspect values, and so on.

   ![Breakpoint](media/edit-and-refresh/breakpoint-console.png)

## Container reuse

During the development cycle, Visual Studio only rebuilds your container images and the container itself when you change the Dockerfile, but if not, it reuses the container from a previous run.

If you manually modified your container and want to restart from a clean container image, use the **Build** > **Clean** command in Visual Studio, and then build as normal.

## Summary

With Docker support in Visual Studio, you can get the productivity of working locally,
with the production realism of developing within a Docker container.

## Troubleshooting

[Troubleshooting Visual Studio Docker Development](troubleshooting-docker-errors.md)

## More about Docker with Visual Studio, Windows, and Azure

* [Container development with Visual Studio](/visualstudio/containers) - a container development landing page
* [Docker Integration for Azure Pipelines](https://aka.ms/dockertoolsforvsts) - Build and Deploy docker containers
* [Windows Container Information](https://aka.ms/containers)- Windows Server and Nano Server information
* [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) - [Azure Kubernetes Service Documentation](/azure/aks)
