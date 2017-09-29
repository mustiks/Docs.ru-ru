---
title: "Общие сведения о размещении и развертывании в ASP.NET Core"
author: tdykstra
description: "Общие сведения о настройке сред размещения и развертывании приложений ASP.NET Core в этих средах."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: df3c1f0c2768b89c3ea5dc901782170c530a542e
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a><span data-ttu-id="d9479-104">Общие сведения о размещении и развертывании приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9479-104">Hosting and deployment overview for ASP.NET Core apps</span></span>

<span data-ttu-id="d9479-105">При развертывании приложения ASP.NET Core в среде внешнего размещения выполняются следующие действия.</span><span class="sxs-lookup"><span data-stu-id="d9479-105">Here are the main steps you perform to deploy an ASP.NET Core app to a hosting environment:</span></span>

* <span data-ttu-id="d9479-106">Публикация приложения в папку на сервере размещения.</span><span class="sxs-lookup"><span data-stu-id="d9479-106">Publish the app to a folder on the hosting server.</span></span>
* <span data-ttu-id="d9479-107">Настройка диспетчера процессов, который запускает приложение при поступлении запросов и перезапускает его после аварийного завершения или после перезагрузки сервера.</span><span class="sxs-lookup"><span data-stu-id="d9479-107">Set up a process manager that starts the app when requests come in and restarts it after it crashes or the server reboots.</span></span>
* <span data-ttu-id="d9479-108">Настройка обратного прокси-сервера, который перенаправляет запросы в приложение.</span><span class="sxs-lookup"><span data-stu-id="d9479-108">Set up a reverse proxy that forwards requests to the app.</span></span>

## <a name="publish-to-a-folder"></a><span data-ttu-id="d9479-109">Публикация в папку</span><span class="sxs-lookup"><span data-stu-id="d9479-109">Publish to a folder</span></span> 

<span data-ttu-id="d9479-110">Команда интерфейса командной строки [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) компилирует код приложения и копирует файлы, необходимые для его выполнения, в папку в *публикации*.</span><span class="sxs-lookup"><span data-stu-id="d9479-110">The [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) CLI command compiles application code and copies the files needed to run the application into a *publish* folder.</span></span> <span data-ttu-id="d9479-111">При развертывании из Visual Studio шаг `dotnet publish` выполняется автоматически перед копированием файлов место развертывания.</span><span class="sxs-lookup"><span data-stu-id="d9479-111">When you deploy from Visual Studio the `dotnet publish` step is done for you automatically before files are copied to the deployment destination.</span></span>

### <a name="folder-contents"></a><span data-ttu-id="d9479-112">Содержимое папки</span><span class="sxs-lookup"><span data-stu-id="d9479-112">Folder contents</span></span>

<span data-ttu-id="d9479-113">Папка *публикации* содержит *EXE*- и *DLL*-файлы и зависимости приложения, а также может включать среду выполнения .NET.</span><span class="sxs-lookup"><span data-stu-id="d9479-113">The *publish* folder contains *.exe* and *.dll* files for the application, its dependencies, and optionally the .NET runtime.</span></span>

<span data-ttu-id="d9479-114">Приложения .NET Core могут публиковаться как *автономные* или *зависящие от платформы*.</span><span class="sxs-lookup"><span data-stu-id="d9479-114">A .NET Core app can be published as *self-contained* or *framework-dependent*.</span></span> <span data-ttu-id="d9479-115">Если приложение автономное, в папку *публикации* добавляются *DLL*-файлы, содержащие среду выполнения .NET.</span><span class="sxs-lookup"><span data-stu-id="d9479-115">If the app is self-contained, the *.dll* files that contain the .NET runtime are included in the *publish* folder.</span></span>  <span data-ttu-id="d9479-116">Если приложение зависит от платформы, файлы среды выполнения .NET не добавляются, так как приложение ссылается на версию .NET, установленную на компьютере.</span><span class="sxs-lookup"><span data-stu-id="d9479-116">If the app is framework-dependent, the .NET runtime files are not included because the app has a reference to a version of .NET that is installed on the computer.</span></span> <span data-ttu-id="d9479-117">По умолчанию используется модель развертывания с зависимостью от платформы.</span><span class="sxs-lookup"><span data-stu-id="d9479-117">The default deployment model is framework-dependent.</span></span> <span data-ttu-id="d9479-118">Дополнительные сведения см. в статье [Развертывание приложений .NET Core](https://docs.microsoft.com/dotnet/articles/core/deploying/index).</span><span class="sxs-lookup"><span data-stu-id="d9479-118">For more information, see [.NET Core application deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/index).</span></span>

<span data-ttu-id="d9479-119">В дополнение к *EXE*- и *DLL*-файлам папка *публикации* для приложения ASP.NET Core обычно содержит файлы конфигурации, статические ресурсы и представления MVC.</span><span class="sxs-lookup"><span data-stu-id="d9479-119">In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views.</span></span>  <span data-ttu-id="d9479-120">Дополнительные сведения см. в статье [Структура каталогов](xref:hosting/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="d9479-120">For more information, see [Directory structure](xref:hosting/directory-structure).</span></span>

## <a name="set-up-a-process-manager"></a><span data-ttu-id="d9479-121">Настройка диспетчер процессов</span><span class="sxs-lookup"><span data-stu-id="d9479-121">Set up a process manager</span></span>

<span data-ttu-id="d9479-122">Приложение ASP.NET Core — это консольное приложение, которое должно запускаться при загрузке сервера и перезапускаться после его аварийного завершения.</span><span class="sxs-lookup"><span data-stu-id="d9479-122">An ASP.NET Core app is a console app that has to be started when a server boots and restarted after crashes.</span></span> <span data-ttu-id="d9479-123">Для автоматического запуска и перезапуска требуется диспетчер процессов.</span><span class="sxs-lookup"><span data-stu-id="d9479-123">To automate starts and restarts you need a process manager.</span></span> <span data-ttu-id="d9479-124">Чаще всего для ASP.NET Core используются такие диспетчеры запросов, как [Nginx](xref:publishing/linuxproduction) и [Apache](xref:publishing/apache-proxy) в Linux, а также [IIS](xref:publishing/iis) и [Windows Service](xref:hosting/windows-service) в Windows.</span><span class="sxs-lookup"><span data-stu-id="d9479-124">The most common process managers for ASP.NET Core are [Nginx](xref:publishing/linuxproduction) and [Apache](xref:publishing/apache-proxy) on Linux, and [IIS](xref:publishing/iis) and [Windows Service](xref:hosting/windows-service) on Windows.</span></span>

## <a name="set-up-a-reverse-proxy"></a><span data-ttu-id="d9479-125">Настройка обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="d9479-125">Set up a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9479-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9479-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d9479-127">Если ваше приложение использует веб-сервер [Kestrel](xref:fundamentals/servers/kestrel), в качестве обратного прокси-сервера можно использовать [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) или [IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="d9479-127">If your app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server, you can use [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), or [IIS](xref:publishing/iis) as a reverse proxy server.</span></span> <span data-ttu-id="d9479-128">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="d9479-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="d9479-129">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d9479-129">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9479-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9479-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d9479-131">Если ваше приложение использует веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) и выходит в Интернет, в качестве обратного прокси-сервера необходимо использовать [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) или [IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="d9479-131">If your app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server and will be exposed to the Internet, you must use [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), or [IIS](xref:publishing/iis) as a reverse proxy server.</span></span> <span data-ttu-id="d9479-132">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="d9479-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="d9479-133">Основной причиной использования обратного прокси-сервера является безопасность.</span><span class="sxs-lookup"><span data-stu-id="d9479-133">The main reason for using a reverse proxy is security.</span></span> <span data-ttu-id="d9479-134">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d9479-134">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a><span data-ttu-id="d9479-135">Использование Visual Studio и MSBuild для автоматизации развертывания</span><span class="sxs-lookup"><span data-stu-id="d9479-135">Using Visual Studio and MSBuild to automate deployment</span></span>

<span data-ttu-id="d9479-136">Помимо копирования выходных данных `dotnet publish` на сервер в процессе развертывания часто требуется выполнение и других задач.</span><span class="sxs-lookup"><span data-stu-id="d9479-136">Deployment often requires additional tasks besides copying the output from `dotnet publish` to a server.</span></span> <span data-ttu-id="d9479-137">Это может быть, например, включение дополнительных файлов в папку *публикации* или исключение из нее каких-либо файлов.</span><span class="sxs-lookup"><span data-stu-id="d9479-137">For example, you might want to include extra files in the *publish* folder, or exclude files from it.</span></span> <span data-ttu-id="d9479-138">Visual Studio использует для веб-развертывания MSBuild и настраивает MSBuild для решения многих других задач в процессе развертывания.</span><span class="sxs-lookup"><span data-stu-id="d9479-138">Visual Studio uses MSBuild for web deployment, and you can customize MSBuild to do many other tasks during deployment.</span></span> <span data-ttu-id="d9479-139">Дополнительные сведения см. в статье [Профили публикации в Visual Studio](xref:publishing/web-publishing-vs) и в книге [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Использование MSBuild и сборки Team Foundation).</span><span class="sxs-lookup"><span data-stu-id="d9479-139">For more information, see [Publish profiles in Visual Studio](xref:publishing/web-publishing-vs) and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.</span></span>

<span data-ttu-id="d9479-140">Развертывание можно выполнять напрямую из Visual Studio в службу приложений Azure, используя [компонент веб-публикации](xref:tutorials/publish-to-azure-webapp-using-vs) или [встроенную поддержку Git](xref:publishing/azure-continuous-deployment).</span><span class="sxs-lookup"><span data-stu-id="d9479-140">You can deploy directly from Visual Studio to Azure App Service by using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or by using [built-in Git support](xref:publishing/azure-continuous-deployment).</span></span> <span data-ttu-id="d9479-141">Visual Studio Team Services поддерживает [непрерывное развертывание в службе приложений Azure](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).</span><span class="sxs-lookup"><span data-stu-id="d9479-141">Visual Studio Team Services supports [continuous deployment to Azure App Service](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9479-142">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d9479-142">Additional resources</span></span>

<span data-ttu-id="d9479-143">Сведения об использовании Docker в качестве среды размещения см. в статье [Размещение приложений ASP.NET Core в Docker](xref:publishing/docker).</span><span class="sxs-lookup"><span data-stu-id="d9479-143">For information about using Docker as a hosting environment, see [Host ASP.NET Core apps in Docker](xref:publishing/docker).</span></span>