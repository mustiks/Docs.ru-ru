---
title: Начало работы с Razor Pages ASP.NET Core в Visual Studio Code
author: rick-anderson
description: Основные сведения о создании веб-приложении Razor Pages в ASP.NET Core с помощью Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244857"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Начало работы с Razor Pages ASP.NET Core в Visual Studio Code

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core. Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:razor-pages/index). Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

Из терминала выполните следующие команды.

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages. Откройте в браузере страницу http://localhost:5000 для просмотра приложения.

![Домашняя или индексная страница](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Открытие проекта

Нажмите клавиши CTRL+C, чтобы завершить работу приложения.

В Visual Studio Code (VS Code) откройте меню **Файл > Открыть папку** и выберите папку *RazorPagesMovie*.

- Нажмите кнопку **Да** в **предупреждении** "Required assets to build and debug are missing from 'TodoApi'.Add them?" (В TodoApi отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?).
- Щелкните **Восстановить** в **информационном** сообщении "There are unresolved dependencies" (Имеются неразрешенные зависимости).

### <a name="launch-the-app"></a>Запуск приложения

В меню **Отладка** выберите пункт **Запуск без отладки**. Кроме того, можно нажать сочетание клавиш, отображаемое рядом с пунктом меню. Это сочетание клавиш зависит от операционной системы.

В следующем учебнике мы добавим в проект модель. 

> [!div class="step-by-step"]
> [Далее: добавление модели](xref:tutorials/razor-pages-vsc/model)  
