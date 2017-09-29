---
title: "Создание веб-API с помощью ASP.NET Core и VS Code"
description: "Создание веб-API на платформах macOS, Linux или Windows с помощью ASP.NET Core MVC и Visual Studio Code"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, веб-API, REST, Mac, Linux, HTTP, служба, служба HTTP, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: e09943b2f810d04456a65589976aa07065a9f010
ms.sourcegitcommit: e6bcd56a4b11e20ff55df004971f9ed384937342
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="02c21-104">Создание веб-API с помощью ASP.NET Core MVC и Visual Studio Code на платформах macOS, Windows и Linux</span><span class="sxs-lookup"><span data-stu-id="02c21-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="02c21-105">Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Майк Уоссон (Mike Wasson)](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="02c21-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="02c21-106">В этом учебнике вы создадите веб-API для управления списком дел.</span><span class="sxs-lookup"><span data-stu-id="02c21-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="02c21-107">Вы не будете создавать пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="02c21-107">You won’t build a UI.</span></span>

<span data-ttu-id="02c21-108">Существует 3 версии этого учебника:</span><span class="sxs-lookup"><span data-stu-id="02c21-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="02c21-109">macOS, Linux, Windows: создание веб-API с помощью Visual Studio Code (этот учебник)</span><span class="sxs-lookup"><span data-stu-id="02c21-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="02c21-110">macOS: [создание веб-API с помощью Visual Studio для Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="02c21-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="02c21-111">Windows: [создание веб-API с помощью Visual Studio для Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="02c21-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="02c21-112">Настройка среды разработки</span><span class="sxs-lookup"><span data-stu-id="02c21-112">Set up your development environment</span></span>

<span data-ttu-id="02c21-113">Скачайте и установите следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="02c21-113">Download and install:</span></span>
- [<span data-ttu-id="02c21-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="02c21-114">.NET Core</span></span>](https://www.microsoft.com/net/core)
- [<span data-ttu-id="02c21-115">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="02c21-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="02c21-116">[Расширение C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="02c21-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="02c21-117">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="02c21-117">Create the project</span></span>

<span data-ttu-id="02c21-118">Из консоли выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="02c21-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="02c21-119">Откройте папку *TodoApi* в Visual Studio Code (VS Code) и выберите файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="02c21-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="02c21-120">В **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?) нажмите кнопку **Да**</span><span class="sxs-lookup"><span data-stu-id="02c21-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="02c21-121">.</span><span class="sxs-lookup"><span data-stu-id="02c21-121">Add them?"</span></span>
- <span data-ttu-id="02c21-122">В **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости) щелкните **Восстановить**.</span><span class="sxs-lookup"><span data-stu-id="02c21-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code с предупреждением "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="02c21-126">Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее.</span><span class="sxs-lookup"><span data-stu-id="02c21-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="02c21-127">В браузере перейдите по адресу http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="02c21-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="02c21-128">Отобразится следующее:</span><span class="sxs-lookup"><span data-stu-id="02c21-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="02c21-129">Советы по использованию VS Code см. в [справке по Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="02c21-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="02c21-130">Добавление поддержки для Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="02c21-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="02c21-131">Измените файл *TodoApi.csproj*, чтобы установить поставщик базы данных [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="02c21-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="02c21-132">Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных.</span><span class="sxs-lookup"><span data-stu-id="02c21-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

<span data-ttu-id="02c21-133">Запустите `dotnet restore`, чтобы скачать и установить поставщик базы данных EF Core InMemory DB.</span><span class="sxs-lookup"><span data-stu-id="02c21-133">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="02c21-134">Можно запустить `dotnet restore` из терминала либо ввести `⌘⇧P` (в macOS) или `Ctrl+Shift+P` (в Linux) в VS Code, а затем ввести **.NET**.</span><span class="sxs-lookup"><span data-stu-id="02c21-134">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="02c21-135">Выберите **.NET: восстановить пакеты**.</span><span class="sxs-lookup"><span data-stu-id="02c21-135">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="02c21-136">Добавление класса модели</span><span class="sxs-lookup"><span data-stu-id="02c21-136">Add a model class</span></span>

<span data-ttu-id="02c21-137">Модель — это объект, представляющий данные в приложении.</span><span class="sxs-lookup"><span data-stu-id="02c21-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="02c21-138">В этом случае единственной моделью является задача.</span><span class="sxs-lookup"><span data-stu-id="02c21-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="02c21-139">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="02c21-139">Add a folder named *Models*.</span></span> <span data-ttu-id="02c21-140">Классы моделей можно размещать в любом месте в проекте, но по соглашению используется папка *Models*.</span><span class="sxs-lookup"><span data-stu-id="02c21-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="02c21-141">Добавьте класс `TodoItem` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="02c21-141">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="02c21-142">После создания `TodoItem` база данных сформирует `Id`.</span><span class="sxs-lookup"><span data-stu-id="02c21-142">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="02c21-143">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="02c21-143">Create the database context</span></span>

<span data-ttu-id="02c21-144">*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="02c21-144">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="02c21-145">Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="02c21-145">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="02c21-146">Добавьте класс `TodoContext` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="02c21-146">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="02c21-147">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="02c21-147">Add a controller</span></span>

<span data-ttu-id="02c21-148">В папке *Controllers* создайте класс с именем `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="02c21-148">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="02c21-149">Добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="02c21-149">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="02c21-150">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="02c21-150">Launch the app</span></span>

<span data-ttu-id="02c21-151">В VS Code нажмите клавишу F5, чтобы запустить приложение.</span><span class="sxs-lookup"><span data-stu-id="02c21-151">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="02c21-152">Перейдите по адресу http://localhost:5000/api/todo (только что созданный контроллер `Todo`).</span><span class="sxs-lookup"><span data-stu-id="02c21-152">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="02c21-153">Справка по Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="02c21-153">Visual Studio Code help</span></span>

- [<span data-ttu-id="02c21-154">Начало работы</span><span class="sxs-lookup"><span data-stu-id="02c21-154">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="02c21-155">Отладка</span><span class="sxs-lookup"><span data-stu-id="02c21-155">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="02c21-156">Интегрированный терминал</span><span class="sxs-lookup"><span data-stu-id="02c21-156">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="02c21-157">Сочетания клавиш</span><span class="sxs-lookup"><span data-stu-id="02c21-157">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="02c21-158">Сочетания клавиш для Mac</span><span class="sxs-lookup"><span data-stu-id="02c21-158">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="02c21-159">Сочетания клавиш для Linux</span><span class="sxs-lookup"><span data-stu-id="02c21-159">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="02c21-160">Сочетания клавиш для Windows</span><span class="sxs-lookup"><span data-stu-id="02c21-160">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]

