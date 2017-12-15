---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: "Привязка данных управления \"ползунок\" (Visual Basic) | Документы Microsoft"
author: wenz
description: "Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши. Это позволяет привязать текущий иция..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6d106fda523356c9b7abd2d82b2d82537b50bd21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="f7e2a-104">Привязка данных управления "ползунок" (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="f7e2a-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="f7e2a-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f7e2a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f7e2a-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f7e2a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="f7e2a-107">Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="f7e2a-108">Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="f7e2a-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="f7e2a-109">Overview</span></span>

<span data-ttu-id="f7e2a-110">Ползунок в наборе элементов управления AJAX имеет графический ползунок, можно управлять с помощью мыши.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="f7e2a-111">Это позволяет привязать текущее положение ползунка на другой элемент управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="f7e2a-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="f7e2a-112">Steps</span></span>

<span data-ttu-id="f7e2a-113">Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):</span><span class="sxs-lookup"><span data-stu-id="f7e2a-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="f7e2a-114">Добавьте два `TextBox` элементов управления на страницу.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="f7e2a-115">Один будет преобразован в графический ползунок, а другой будет содержать положение движка.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="f7e2a-116">Следующий шаг уже является последним шагом.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-116">The next step is already the final step.</span></span> <span data-ttu-id="f7e2a-117">`SliderExtender` Управления из набора элементов управления ASP.NET AJAX делает ползунок из первого текстового поля и автоматически обновляет второе текстовое поле, когда изменения положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="f7e2a-118">Чтобы это работало `SliderExtender` `TargetControlID` атрибут необходимо задать идентификатор элемента в первом текстовом поле; `BoundControlID` атрибуту необходимо присвоить идентификатор второе текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="f7e2a-119">Как видно в браузере, привязка данных работает в обоих направлениях: Введите новое значение в текстовом поле обновляет положение ползунка.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="f7e2a-120">Если второе текстовое поле только для чтения, могут добавлять слабую защиту с текстовым полем, чтобы оно было сложнее для пользователя, чтобы вручную обновить значение в нем.</span><span class="sxs-lookup"><span data-stu-id="f7e2a-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="f7e2a-121">[![Ползунок и текстового поля находятся в синхронизированном состоянии](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f7e2a-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="f7e2a-122">Ползунок и текстового поля находятся в синхронизированном состоянии ([Просмотр полноразмерное изображение](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f7e2a-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f7e2a-123">Назад</span><span class="sxs-lookup"><span data-stu-id="f7e2a-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)