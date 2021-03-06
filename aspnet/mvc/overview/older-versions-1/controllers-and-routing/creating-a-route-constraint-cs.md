---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Создание ограничения маршрута (C#) | Документация Майкрософт
author: StephenWalther
description: В этом руководстве Стивен Вальтер демонстрирует, как можно контролировать, как браузер запрашивает маршруты соответствия путем создания ограничения маршрута с помощью регулярных выражений.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51d76248a59968e1d6befd5d5404f7bf6c2878e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836014"
---
<a name="creating-a-route-constraint-c"></a>Создание ограничения маршрута (C#)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

> В этом руководстве Стивен Вальтер демонстрирует, как можно контролировать, как браузер запрашивает маршруты соответствия путем создания ограничения маршрута с помощью регулярных выражений.


Использовать ограничения маршрута для ограничения браузер запросы, которые соответствуют определенным маршрутом. Регулярное выражение можно использовать для указания ограничения маршрута.

Например представьте, что вы определили маршрута в листинге 1 в файле Global.asax.

**В листинге 1 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

В листинге 1 содержит маршрут с именем продукта. Маршрут продукта можно использовать для сопоставления запросов браузера ProductController, содержащихся в листинге 2.

**В листинге 2 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Обратите внимание на то, что Details() действию, предоставляемым продукта контроллер принимает один параметр с именем productId. Этот параметр является параметром целое число.

Маршрут, определенный в листинге 1 будет соответствовать любой из следующих URL-адресов:

- / Продукта/23
- / Продукта/7

К сожалению маршрут будет соответствовать следующие URL-адреса:

- / Продукта/сведения
- / Продукта/apple

Поскольку действие Details() ожидает целочисленный параметр, выполняющего запрос, содержащий отличные от целочисленное значение приведет к ошибке. Например при вводе /Product/apple URL-адрес в адресную строку браузера на страницу ошибки будет получить на рис. 1.


[![В диалоговом окне нового проекта](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Рис 01**: развернуть страница отображается ([Просмотр полноразмерного изображения](creating-a-route-constraint-cs/_static/image2.png))


Что Вы действительно хотите сделать — соответствовать только URL-адреса, содержащие productId правильное целое число. Ограничение можно использовать при определении маршрута для ограничения URL-адреса, которые соответствуют маршруту. Измененный маршрут продукта в листинге 3 содержит ограничение регулярное выражение, которое соответствует только целые числа.

**Листинг 3 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

Регулярное выражение \d+ соответствует один или несколько целых чисел. Этим ограничением обусловлены сопоставления следующие URL-адреса маршрута продукта:

- / Продукта/3
- / Продукта/8999

Но не следующие URL-адреса:

- / Продукта/apple
- / Продукта

- Эти запросы браузера будут обрабатываться другой маршрут или, если отсутствуют соответствующие маршруты *не удалось найти ресурс* будет возвращена ошибка.

> [!div class="step-by-step"]
> [Назад](creating-custom-routes-cs.md)
> [Вперед](creating-a-custom-route-constraint-cs.md)
