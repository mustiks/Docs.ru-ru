---
title: Добавление модели в приложение MVC ASP.NET Core
author: rick-anderson
description: Добавление модели в простое приложение ASP.NET Core.
ms.author: riande
ms.date: 09/18/2017
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 82b8338f10cb4d58ae06bdb70583e1563c2e6b64
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961429"
---
[!INCLUDE [adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Добавьте класс в папку *Models* с именем *Movie.cs*.
* Добавьте в файл *ToUpperPlugin.cs* следующий код:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Поле `ID` является обязательным для первичного ключа базы данных. 

Соберите приложение, чтобы убедиться, что оно не содержит ошибок. Теперь вы добавили **M**одель в свое приложение **M**VC.

## <a name="prepare-the-project-for-scaffolding"></a>Подготовка проекта для формирования шаблонов

- Добавьте в файл *MvcMovie.csproj* следующие выделенные пакеты NuGet:
             
   [!code-csharp[](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Сохраните файл и нажмите кнопку **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).
- Создайте файл *Models/MvcMovieContext.cs* и добавьте следующий класс `MvcMovieContext`:

   [!code-csharp[](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Откройте файл *Startup.cs* и добавьте два использования:

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Добавьте контекст базы данных в файл *Startup.cs*:

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  За счет этого Entity Framework будет знать, какие классы модели входят в модель данных. Вы определяете один *набор сущностей* объектов Movie, который будут представлен в базу данных как таблица Movie.

- Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.

## <a name="scaffold-the-moviecontroller"></a>Формирование шаблонов для MovieController

Откройте окно терминала в папке проекта и выполните следующие команды.

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Ядро формирование шаблонов создает следующее.

* Контроллер фильмов (*Controllers/MoviesController.cs*)
* Файлы представления Razor для страниц Create, Delete, Edit и Index (*Views/Movies/\*.cshtml*)

Автоматическое создание методов и представлений действия [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*. Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

Теперь у вас есть база данных и страницы для отображения, редактирования, обновления и удаления данных. В следующем руководстве мы будем работать с базой данных.

### <a name="additional-resources"></a>Дополнительные ресурсы

* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro)
* [Глобализация и локализация](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Назад: Добавление представления](adding-view.md)
> [Далее: Работа с SQLite](working-with-sql.md)
