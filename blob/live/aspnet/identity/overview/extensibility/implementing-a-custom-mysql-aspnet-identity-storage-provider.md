---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: "Реализация поставщика хранилища пользовательских MySQL ASP.NET Identity | Документы Microsoft"
author: raquelsa
description: "ASP.NET Identity является расширяемой системой, что дает возможность создать собственный поставщик хранилища и вставьте его в приложение без переработки приложения..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 3bfbccd91705755fc24bb8305fff171baa26f370
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a><span data-ttu-id="41afc-103">Реализация поставщика хранилища ASP.NET Identity пользовательских MySQL</span><span class="sxs-lookup"><span data-stu-id="41afc-103">Implementing a Custom MySQL ASP.NET Identity Storage Provider</span></span>
====================
<span data-ttu-id="41afc-104">по [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="41afc-104">by [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="41afc-105">ASP.NET Identity является расширяемой системой, что дает возможность создать собственный поставщик хранилища и вставьте его в приложение без переработки приложения.</span><span class="sxs-lookup"><span data-stu-id="41afc-105">ASP.NET Identity is an extensible system which enables you to create your own storage provider and plug it into your application without re-working the application.</span></span> <span data-ttu-id="41afc-106">В этом разделе описывается создание поставщика хранилища MySQL для ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="41afc-106">This topic describes how to create a MySQL storage provider for ASP.NET Identity.</span></span> <span data-ttu-id="41afc-107">Обзор создания поставщиков пользовательского хранилища см. в разделе [Обзор из пользовательских поставщиков хранилища для ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).</span><span class="sxs-lookup"><span data-stu-id="41afc-107">For an overview of creating custom storage providers, see [Overview of Custom Storage Providers for ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).</span></span>
> 
> <span data-ttu-id="41afc-108">Для работы с этим учебником требуются Visual Studio 2013 с обновлением 2.</span><span class="sxs-lookup"><span data-stu-id="41afc-108">To complete this tutorial, you must have Visual Studio 2013 with Update 2.</span></span>
> 
> <span data-ttu-id="41afc-109">Этот учебник выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="41afc-109">This tutorial will:</span></span>
> 
> - <span data-ttu-id="41afc-110">Показано, как создать экземпляр базы данных MySQL в Azure.</span><span class="sxs-lookup"><span data-stu-id="41afc-110">Show how to create a MySQL database instance on Azure.</span></span>
> - <span data-ttu-id="41afc-111">Показано, как использовать MySQL клиентского средства (MySQL Workbench) для создания таблиц и управления удаленной базы данных в Azure.</span><span class="sxs-lookup"><span data-stu-id="41afc-111">Show how to use a MySQL client tool (MySQL Workbench) to create tables and manage your remote database on Azure.</span></span>
> - <span data-ttu-id="41afc-112">Показано, как заменить значение по умолчанию реализация хранилища ASP.NET Identity реализацию пользовательского на проект MVC-приложения.</span><span class="sxs-lookup"><span data-stu-id="41afc-112">Show how to replace the default ASP.NET Identity storage implementation with our custom implementation on a MVC application project.</span></span>
> 
> <span data-ttu-id="41afc-113">Этот учебник изначально был записан с Raquel Soares De Almeida и Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="41afc-113">This tutorial was originally written by Raquel Soares De Almeida and Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).</span></span> <span data-ttu-id="41afc-114">Пример проекта был обновлен для идентификации 2.0 по Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="41afc-114">The sample project was updated for Identity 2.0 by Suhas Joshi.</span></span> <span data-ttu-id="41afc-115">Раздел был обновлен для идентификации 2.0, Tom FitzMacken.</span><span class="sxs-lookup"><span data-stu-id="41afc-115">The topic was updated for Identity 2.0 by Tom FitzMacken.</span></span>


## <a name="download-completed-project"></a><span data-ttu-id="41afc-116">Процент загрузки проекта</span><span class="sxs-lookup"><span data-stu-id="41afc-116">Download completed project</span></span>

<span data-ttu-id="41afc-117">В конце этого учебника вы получите проект MVC-приложения с ASP.NET Identity, работать с базой данных MySQL, размещенные в Azure.</span><span class="sxs-lookup"><span data-stu-id="41afc-117">At the end of this tutorial, you will have an MVC application project with ASP.NET Identity working with a MySQL database hosted on Azure.</span></span>

<span data-ttu-id="41afc-118">Можно загрузить поставщик хранилища завершенного MySQL в [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).</span><span class="sxs-lookup"><span data-stu-id="41afc-118">You can download the completed MySQL storage provider at [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).</span></span>

## <a name="the-steps-you-will-perform"></a><span data-ttu-id="41afc-119">Действия, которое будет выполнять</span><span class="sxs-lookup"><span data-stu-id="41afc-119">The steps you will perform</span></span>

<span data-ttu-id="41afc-120">В этом учебнике будет:</span><span class="sxs-lookup"><span data-stu-id="41afc-120">In this tutorial you will:</span></span>

1. <span data-ttu-id="41afc-121">Создание базы данных MySQL в Azure</span><span class="sxs-lookup"><span data-stu-id="41afc-121">Create a MySQL database on Azure</span></span>
2. <span data-ttu-id="41afc-122">Создание таблиц ASP.NET Identity в MySQL</span><span class="sxs-lookup"><span data-stu-id="41afc-122">Create the ASP.NET Identity tables in MySQL</span></span>
3. <span data-ttu-id="41afc-123">Создание приложения MVC и настройте его для использования поставщика MySQL</span><span class="sxs-lookup"><span data-stu-id="41afc-123">Create an MVC application and configure it to use the MySQL provider</span></span>
4. <span data-ttu-id="41afc-124">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="41afc-124">Run the app</span></span>

<span data-ttu-id="41afc-125">В этом разделе описывается архитектура ASP.NET Identity и решений, которые необходимо принять при реализации поставщика хранилища клиента.</span><span class="sxs-lookup"><span data-stu-id="41afc-125">This topic does not cover the architecture of ASP.NET Identity and the decisions you must make when implementing a customer storage provider.</span></span> <span data-ttu-id="41afc-126">Эти сведения в разделе [Обзор из пользовательских поставщиков хранилища для ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).</span><span class="sxs-lookup"><span data-stu-id="41afc-126">For that information, see [Overview of Custom Storage Providers for ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).</span></span>

## <a name="review-mysql-storage-provider-classes"></a><span data-ttu-id="41afc-127">Просмотрите классы поставщиков для хранения данных MySQL</span><span class="sxs-lookup"><span data-stu-id="41afc-127">Review MySQL storage provider classes</span></span>

<span data-ttu-id="41afc-128">Перед обращением к инструкции по созданию поставщика хранилища MySQL, давайте взглянем на классы, входящие в состав поставщика хранилища.</span><span class="sxs-lookup"><span data-stu-id="41afc-128">Before jumping into the steps to create the MySQL storage provider, let's look at the classes that make up the storage provider.</span></span> <span data-ttu-id="41afc-129">Необходимо будет классы, которые управляют операциями базы данных и классы, которые вызываются из приложения для управления пользователями и ролями.</span><span class="sxs-lookup"><span data-stu-id="41afc-129">You will need classes that manage the database operations and classes that are called from the application to manage users and roles.</span></span>

### <a name="storage-classes"></a><span data-ttu-id="41afc-130">Классы хранения</span><span class="sxs-lookup"><span data-stu-id="41afc-130">Storage classes</span></span>

- <span data-ttu-id="41afc-131">[IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -содержит свойства для пользователя.</span><span class="sxs-lookup"><span data-stu-id="41afc-131">[IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) - contains properties for the user.</span></span>
- <span data-ttu-id="41afc-132">[UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -содержит операции для добавления, обновления или получение пользователей.</span><span class="sxs-lookup"><span data-stu-id="41afc-132">[UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) - contains operations for adding, updating or retrieving users.</span></span>
- <span data-ttu-id="41afc-133">[IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -содержит свойства для ролей.</span><span class="sxs-lookup"><span data-stu-id="41afc-133">[IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) - contains properties for roles.</span></span>
- <span data-ttu-id="41afc-134">[RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -содержит операции для добавления, удаления, обновления и получения ролей.</span><span class="sxs-lookup"><span data-stu-id="41afc-134">[RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) - contains operations for adding, deleting, updating and retrieving roles.</span></span>

### <a name="data-access-layer-classes"></a><span data-ttu-id="41afc-135">Классы слой доступа к данным</span><span class="sxs-lookup"><span data-stu-id="41afc-135">Data access layer classes</span></span>

<span data-ttu-id="41afc-136">В этом примере классов слой доступа к данным содержит инструкции SQL для работы с таблицами; Однако в коде можно использовать объектно реляционного сопоставления (ORM), такие как Entity Framework и NHibernate.</span><span class="sxs-lookup"><span data-stu-id="41afc-136">For this example, the data access layer classes contain SQL statements for working with the tables; however, in your code you might want to use object-relational mapping (ORM) such as Entity Framework or NHibernate.</span></span> <span data-ttu-id="41afc-137">В частности приложение может наблюдаться низкая производительность без ORM, включающий отложенную загрузку и кэширование объекта.</span><span class="sxs-lookup"><span data-stu-id="41afc-137">In particular, your application may experience poor performance without an ORM that includes lazy loading and object caching.</span></span> <span data-ttu-id="41afc-138">Дополнительные сведения см. в разделе [ASP.NET 2.0 удостоверений без Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)</span><span class="sxs-lookup"><span data-stu-id="41afc-138">For more information, see [ASP.NET Identity 2.0 without Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)</span></span>

- <span data-ttu-id="41afc-139">[MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -содержит подключения к базе данных MySQL и методы для операций базы данных.</span><span class="sxs-lookup"><span data-stu-id="41afc-139">[MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) - contains the MySQL database connection and methods for performing database operations.</span></span> <span data-ttu-id="41afc-140">UserStore и RoleStore создаются с помощью экземпляра этого класса.</span><span class="sxs-lookup"><span data-stu-id="41afc-140">UserStore and RoleStore are both instantiated with an instance of this class.</span></span>
- <span data-ttu-id="41afc-141">[RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -содержит операции с базой данных для таблицы, которая хранит ролей.</span><span class="sxs-lookup"><span data-stu-id="41afc-141">[RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) - contains database operations for the table that stores roles.</span></span>
- <span data-ttu-id="41afc-142">[UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -содержит операции с базой данных для таблицы, которая хранит утверждения пользователей.</span><span class="sxs-lookup"><span data-stu-id="41afc-142">[UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) - contains database operations for the table that stores user claims.</span></span>
- <span data-ttu-id="41afc-143">[UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -содержит операции с базой данных для таблицы, в которой хранятся сведения об имени входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="41afc-143">[UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) - contains database operations for the table that stores user login information.</span></span>
- <span data-ttu-id="41afc-144">[UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -содержит операции с базой данных для таблицы, которая содержит пользователей, которым назначены роли, к которым.</span><span class="sxs-lookup"><span data-stu-id="41afc-144">[UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) - contains database operations for the table that stores which users are assigned to which roles.</span></span>
- <span data-ttu-id="41afc-145">[UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -содержит операции для таблицы, которая содержит пользователей с базой данных.</span><span class="sxs-lookup"><span data-stu-id="41afc-145">[UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) - contains database operations for the table that stores users.</span></span>

## <a name="create-a-mysql-database-instance-on-azure"></a><span data-ttu-id="41afc-146">Создайте экземпляр базы данных MySQL в Azure</span><span class="sxs-lookup"><span data-stu-id="41afc-146">Create a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="41afc-147">Войдите на [портал Azure](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="41afc-147">Log in to the [Azure Portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="41afc-148">Нажмите кнопку **+ создать** в нижней части страницы, а затем выберите **ХРАНИЛИЩЕ**.</span><span class="sxs-lookup"><span data-stu-id="41afc-148">Click **+NEW** at the bottom of the page, and then select **STORE**.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. <span data-ttu-id="41afc-149">В **Выбор и надстройки** мастера выберите **базы данных MySQL ClearDB** и щелкните стрелку в правом нижнем углу диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="41afc-149">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database** and click on the next arrow at the bottom right of the dialog.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. <span data-ttu-id="41afc-150">Оставьте значение по умолчанию **Free** планирования и изменить **имя** для **IdentityMySQLDatabase**.</span><span class="sxs-lookup"><span data-stu-id="41afc-150">Keep the default **Free** plan and change the **Name** to **IdentityMySQLDatabase**.</span></span> <span data-ttu-id="41afc-151">Выберите ближайший регион и щелкните стрелку «Далее».</span><span class="sxs-lookup"><span data-stu-id="41afc-151">Select the region nearest you and then click the next arrow.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. <span data-ttu-id="41afc-152">Установите флажок для завершения создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="41afc-152">Click the checkmark to complete the database creation.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. <span data-ttu-id="41afc-153">После создания базы данных можно было управлять из **надстройки** на портале управления.</span><span class="sxs-lookup"><span data-stu-id="41afc-153">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span>   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. <span data-ttu-id="41afc-154">Соединение с базой данных можно получить, щелкнув **сведений о СОЕДИНЕНИИ** в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="41afc-154">You can get the database connection information by clicking on **CONNECTION INFO** at the bottom of the page.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. <span data-ttu-id="41afc-155">Скопируйте строку подключения, нажав кнопку "Копировать" и сохраните его, чтобы можно было использовать позже в приложении MVC.</span><span class="sxs-lookup"><span data-stu-id="41afc-155">Copy the connection string by clicking on the copy button and save it so you can use later in your MVC application.</span></span>   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a><span data-ttu-id="41afc-156">Создание таблиц ASP.NET Identity в базе данных MySQL</span><span class="sxs-lookup"><span data-stu-id="41afc-156">Create the ASP.NET Identity tables in a MySQL database</span></span>

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a><span data-ttu-id="41afc-157">Установите средство MySQL Workbench для подключения и управления базой данных MySQL</span><span class="sxs-lookup"><span data-stu-id="41afc-157">Install MySQL Workbench tool to connect and manage MySQL database</span></span>

1. <span data-ttu-id="41afc-158">Установка **MySQL Workbench** средства из [MySQL файлов для загрузки](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="41afc-158">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="41afc-159">Запустите приложение и добавить, выберите на **MySQLConnections +** кнопку, чтобы добавить новое подключение.</span><span class="sxs-lookup"><span data-stu-id="41afc-159">Launch the app and add click on the **MySQLConnections +** button to add a new connection.</span></span> <span data-ttu-id="41afc-160">Используйте данные строки соединения, которые копируются из базы данных MySQL в Azure, созданный ранее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="41afc-160">Use the connection string data you copied from the Azure MySQL database you created earlier in this tutorial.</span></span>
3. <span data-ttu-id="41afc-161">После установления соединения, откройте новый **запроса** вкладка; команды из вставки [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) в окно запроса и выполните его для создания таблиц базы данных.</span><span class="sxs-lookup"><span data-stu-id="41afc-161">After establishing the connection, open a new **Query** tab; paste the commands from [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) into the query and execute it in order to create the database tables.</span></span>
4. <span data-ttu-id="41afc-162">Теперь у вас есть все ASP.NET Identity необходимые таблицы, созданные в базе данных MySQL, размещенные в Azure, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="41afc-162">You now have all the ASP.NET Identity necessary tables created on a MySQL database hosted on Azure as shown below.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a><span data-ttu-id="41afc-163">Создать проект MVC-приложения на основе шаблона и настроить его для использования поставщика MySQL</span><span class="sxs-lookup"><span data-stu-id="41afc-163">Create an MVC application project from template and configure it to use MySQL provider</span></span>

<span data-ttu-id="41afc-164">При необходимости установите [Visual Studio Express 2013 для Web](https://go.microsoft.com/fwlink/?LinkId=299058) или [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) с обновлением 2.</span><span class="sxs-lookup"><span data-stu-id="41afc-164">If needed, install either [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) with Update 2.</span></span>

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a><span data-ttu-id="41afc-165">Загрузите проект ASP.NET.Identity.MySQL из CodePlex</span><span class="sxs-lookup"><span data-stu-id="41afc-165">Download the ASP.NET.Identity.MySQL project from CodePlex</span></span>

1. <span data-ttu-id="41afc-166">Перейдите к URL-адрес репозитория в [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).</span><span class="sxs-lookup"><span data-stu-id="41afc-166">Browse to the repository URL at [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).</span></span>
2. <span data-ttu-id="41afc-167">Загрузка исходного кода.</span><span class="sxs-lookup"><span data-stu-id="41afc-167">Download the source code.</span></span>
3. <span data-ttu-id="41afc-168">Извлеките ZIP-файл в локальную папку.</span><span class="sxs-lookup"><span data-stu-id="41afc-168">Extract the .zip file into a local folder.</span></span>
4. <span data-ttu-id="41afc-169">Откройте решение AspNet.Identity.MySQL и постройте его.</span><span class="sxs-lookup"><span data-stu-id="41afc-169">Open the AspNet.Identity.MySQL solution and build it.</span></span>

### <a name="create-a-new-mvc-application-project-from-template"></a><span data-ttu-id="41afc-170">Создайте новый проект MVC-приложения на основе шаблона</span><span class="sxs-lookup"><span data-stu-id="41afc-170">Create a new MVC application project from template</span></span>

1. <span data-ttu-id="41afc-171">Щелкните правой кнопкой мыши **AspNet.Identity.MySQL** решения и **добавить**, **нового проекта**</span><span class="sxs-lookup"><span data-stu-id="41afc-171">Right click the **AspNet.Identity.MySQL** solution and **Add**, **New Project**</span></span>
2. <span data-ttu-id="41afc-172">В **Добавление нового проекта** окна выберите **Visual C#** слева, затем **Web** , а затем выберите **веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="41afc-172">In the **Add New Project** Dialog select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="41afc-173">Присвойте проекту имя **IdentityMySQLDemo**; и нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="41afc-173">Name your project **IdentityMySQLDemo**; and then click OK.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. <span data-ttu-id="41afc-174">В **новый проект ASP.NET** диалоговое окно, выберите шаблон MVC с параметрами по умолчанию (включающий **отдельных учетных записей пользователей** как метод проверки подлинности) и нажмите кнопку **ОК** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)</span><span class="sxs-lookup"><span data-stu-id="41afc-174">In the **New ASP.NET Project** dialog, select the MVC template with the default options (that includes **Individual User Accounts** as authentication method) and click **OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)</span></span>
4. <span data-ttu-id="41afc-175">В обозревателе решений щелкните правой кнопкой мыши проект IdentityMySQLDemo и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="41afc-175">In Solution Explorer, right-click your IdentityMySQLDemo project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="41afc-176">В диалоговом окне поле текст поиска, введите **Identity.EntityFramework**.</span><span class="sxs-lookup"><span data-stu-id="41afc-176">In the search text box dialog, type **Identity.EntityFramework**.</span></span> <span data-ttu-id="41afc-177">Выберите этот пакет в списке результатов и выберите **удаления**.</span><span class="sxs-lookup"><span data-stu-id="41afc-177">Select this package in the list of results and click **Uninstall**.</span></span> <span data-ttu-id="41afc-178">Вам будет предложено удаления зависимостей пакета EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="41afc-178">You will be prompted to uninstall the dependency package EntityFramework.</span></span> <span data-ttu-id="41afc-179">Щелкните Да, мы больше не будет этого пакета в этом приложении.</span><span class="sxs-lookup"><span data-stu-id="41afc-179">Click on Yes as we will no longer this package on this application.</span></span>
5. <span data-ttu-id="41afc-180">Щелкните правой кнопкой мыши проект IdentityMySQLDemo, выберите **добавить**, **ссылку, решения, проекты;** выберите AspNet.Identity.MySQL проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="41afc-180">Right click the IdentityMySQLDemo project, select **Add**, **Reference, Solution, Projects;** select the AspNet.Identity.MySQL project and click **OK**.</span></span>
6. <span data-ttu-id="41afc-181">В проекте IdentityMySQLDemo замените все вхождения</span><span class="sxs-lookup"><span data-stu-id="41afc-181">In the IdentityMySQLDemo project, replace all references to</span></span>  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
 <span data-ttu-id="41afc-182">на</span><span class="sxs-lookup"><span data-stu-id="41afc-182">with</span></span>  
     `using AspNet.Identity.MySQL;`
7. <span data-ttu-id="41afc-183">В IdentityModels.cs, задайте **ApplicationDbContext** для наследования от **MySqlDatabase** и включает конструктор, который принимает один параметр с именем подключения.</span><span class="sxs-lookup"><span data-stu-id="41afc-183">In IdentityModels.cs, set **ApplicationDbContext** to derive from **MySqlDatabase** and include a contructor that take a single parameter with the connection name.</span></span>  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. <span data-ttu-id="41afc-184">Откройте файл IdentityConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="41afc-184">Open the IdentityConfig.cs file.</span></span> <span data-ttu-id="41afc-185">В **ApplicationUserManager.Create** метод, replace, при создании экземпляра UserManager следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="41afc-185">In the **ApplicationUserManager.Create** method, replace instantiating UserManager with the following code:</span></span>  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. <span data-ttu-id="41afc-186">Откройте файл web.config и замените строку, DefaultConnection эту запись, заменив значения выделенная строка подключения базы данных MySQL, созданный на предыдущих шагах.</span><span class="sxs-lookup"><span data-stu-id="41afc-186">Open the web.config file and replace the DefaultConnection string with this entry replacing the highlighted values with the connection string of the MySQL database you created on previous steps:</span></span>  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a><span data-ttu-id="41afc-187">Запустите приложение и подключитесь к базе данных MySQL</span><span class="sxs-lookup"><span data-stu-id="41afc-187">Run the app and connect to the MySQL DB</span></span>

1. <span data-ttu-id="41afc-188">Щелкните правой кнопкой мыши **IdentityMySQLDemo** проект и выберите **Назначить запускаемым проектом**</span><span class="sxs-lookup"><span data-stu-id="41afc-188">Right click the **IdentityMySQLDemo** project and select **Set as Startup Project**</span></span>
2. <span data-ttu-id="41afc-189">Нажмите клавишу **сочетание клавиш Ctrl + F5** для построения и запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="41afc-189">Press **Ctrl + F5** to build and run the app.</span></span>
3. <span data-ttu-id="41afc-190">Щелкните **зарегистрировать** вкладки в верхней части страницы.</span><span class="sxs-lookup"><span data-stu-id="41afc-190">Click on **Register** tab on the top of the page.</span></span>
4. <span data-ttu-id="41afc-191">Введите новое имя пользователя и пароль и нажмите кнопку на **зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="41afc-191">Enter a new user name and password and then click on **Register**.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. <span data-ttu-id="41afc-192">Новый пользователь теперь зарегистрирован и вход.</span><span class="sxs-lookup"><span data-stu-id="41afc-192">The new user is now registered and logged in.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. <span data-ttu-id="41afc-193">Вернитесь в средство MySQL Workbench и проверять **IdentityMySQLDatabase** содержимое таблицы.</span><span class="sxs-lookup"><span data-stu-id="41afc-193">Go back to the MySQL Workbench tool and inspect the **IdentityMySQLDatabase** table's contents.</span></span> <span data-ttu-id="41afc-194">Проверьте таблицу пользователей для операций по мере регистрации новых пользователей.</span><span class="sxs-lookup"><span data-stu-id="41afc-194">Inspect the users table for the entries as you register new users.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a><span data-ttu-id="41afc-195">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="41afc-195">Next Steps</span></span>

<span data-ttu-id="41afc-196">Дополнительные сведения о включении других методов проверки подлинности на это приложение посвящены [создать приложение ASP.NET MVC 5 с Facebook и Google OAuth2 и входа OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="41afc-196">For more information on how to enable other authentication methods on this app, refer to [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<span data-ttu-id="41afc-197">Чтобы узнать, как интегрировать базы данных с помощью OAuth и настроить роли, чтобы ограничить доступ пользователей к приложению, см. [развернуть приложение для защиты ASP.NET MVC 5 с членством, OAuth и базы данных SQL на Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="41afc-197">To learn how to integrate your DB with OAuth and to set up roles to limit users access to your app, see [Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>