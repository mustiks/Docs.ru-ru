---
title: "Добавление модели в приложение Razor Pages в ASP.NET Core"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core"
keywords: ASP.NET Core,Razor Pages,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/modelz
ms.openlocfilehash: 8e370decfd81e62022478b0ab695ff876e5e0a10
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="c4bb3-104">Добавление модели в приложение Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c4bb3-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="c4bb3-105">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="c4bb3-105">Add a data model</span></span>

<span data-ttu-id="c4bb3-106">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="c4bb3-107">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-107">Name the folder *Models*.</span></span>

<span data-ttu-id="c4bb3-108">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-108">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="c4bb3-109">Добавьте класс **Movie** и следующие свойства.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="c4bb3-110">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="c4bb3-110">Add a database connection string</span></span>

<span data-ttu-id="c4bb3-111">Добавьте строку подключения в файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="c4bb3-112">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="c4bb3-112">Register the database context</span></span>

<span data-ttu-id="c4bb3-113">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

<span data-ttu-id="c4bb3-114">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="c4bb3-115">Добавление средств формирования шаблонов и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="c4bb3-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="c4bb3-116">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="c4bb3-117">Добавление веб-пакета создания кода для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="c4bb3-118">Этот пакет необходим для работы ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="c4bb3-119">Добавление первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-119">Add an initial migration.</span></span>
* <span data-ttu-id="c4bb3-120">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="c4bb3-121">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-121">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="c4bb3-123">В PMC введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="c4bb3-124">Команда `Install-Package` устанавливает средства, необходимые для запуска ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="c4bb3-125">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="c4bb3-126">Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="c4bb3-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="c4bb3-127">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="c4bb3-128">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="c4bb3-129">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="c4bb3-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="c4bb3-130">Команда `Update-Database` выполняет `Up` метод в файле *Migrations/\<отметка-времени>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

<span data-ttu-id="c4bb3-131">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c4bb3-131">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c4bb3-132">[Назад: Начало работы](xref:tutorials/razor-pages/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="c4bb3-132">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    