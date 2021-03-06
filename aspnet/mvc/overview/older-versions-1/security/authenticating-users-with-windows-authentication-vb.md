---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Проверка подлинности пользователей с проверкой подлинности Windows (Visual Basic) | Документация Майкрософт
author: microsoft
description: Узнайте, как использовать проверку подлинности Windows в контексте приложения MVC. Вы узнаете, как включить проверку подлинности Windows в рамках приложения web co...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: e37508dedd4243dd1a1638e68760f6f4310e61a8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836995"
---
<a name="authenticating-users-with-windows-authentication-vb"></a>Проверка подлинности пользователей с проверкой подлинности Windows (VB)
====================
по [Microsoft](https://github.com/microsoft)

> Узнайте, как использовать проверку подлинности Windows в контексте приложения MVC. Вы узнаете, как включить проверку подлинности Windows в файл веб-конфигурации приложения и настройка проверки подлинности со службами IIS. Наконец вы узнаете, как использовать атрибут [Authorize] для ограничения доступа к действиям контроллера определенным пользователям Windows или группы.


Целью данного руководства является объяснить, как можно эффективно использовать безопасности функции, встроенные в Internet Information Services, чтобы пароль защиты представления в приложениях MVC. Вы узнаете, как разрешить действий контроллера, который должен вызываться только определенным пользователям Windows или пользователей, которые являются членами определенных групп Windows.

С помощью проверки подлинности Windows имеет смысл, если вы создаете на внутренний корпоративный веб-сайт (сайт интрасети), и вы хотите, чтобы пользователи могли использовать свои стандартные имена пользователей Windows и пароли при доступе к веб-сайта. Если вы создаете к краям, с которыми сталкиваются веб-сайта (веб-сайта Internet) рекомендуется использовать проверку подлинности форм.

#### <a name="enabling-windows-authentication"></a>Включение проверки подлинности Windows

При создании нового приложения ASP.NET MVC по умолчанию не включена проверка подлинности Windows. Проверка подлинности форм — тип проверки подлинности по умолчанию включена для приложения MVC. Путем изменения файла конфигурации (web.config) для своего приложения MVC web, необходимо включить проверку подлинности Windows. Найти &lt;проверки подлинности&gt; разделе и изменить его для использования Windows вместо проверки подлинности форм, следующим образом:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

При включении проверки подлинности Windows, веб-сервер отвечает для проверки подлинности пользователей. Как правило существует два типа веб-серверов, которые можно использовать при создании и развертывании приложения ASP.NET MVC.

Во-первых при разработке приложения MVC, использовать веб-сервера разработки ASP.NET, включенные в Visual Studio. По умолчанию веб-сервера разработки ASP.NET выполняет все страницы в контексте текущей учетной записи Windows (учетная запись любой вы использовали для входа в Windows).

Веб-сервера разработки ASP.NET также поддерживает проверку подлинности NTLM. Вы можете включить проверку подлинности NTLM, щелкнув правой кнопкой мыши имя проекта в окне обозревателя решений и выбрав пункт Свойства. Затем выберите вкладку "веб" и установите флажок "NTLM" (см. рис. 1).

**Рис. 1 – Включение проверки подлинности NTLM для веб-сервера разработки ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Для веб-приложения в рабочей среде в наличии, использовании IIS в качестве веб-сервера. IIS поддерживает несколько типов проверки подлинности, в том числе:

- Обычная проверка подлинности — определяются как часть протокола HTTP 1.0. Передает имена пользователей и пароли в виде открытого текста (в кодировке Base64) через Интернет. -Дайджест-проверка подлинности — отправляет хэш пароля, а не пароль, через Интернет. -Интегрированные Windows (NTLM) проверка подлинности — наилучший тип проверки подлинности для использования в интрасети с помощью windows. -Сертификатов проверки подлинности — включает аутентификацию с использованием клиентского сертификата. Сертификат сопоставляется учетной записи пользователя Windows.

> [!NOTE] 
> 
> Более подробный обзор различных типов проверки подлинности, см. в разделе [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Можно использовать диспетчер служб IIS для включения определенного типа проверки подлинности. Имейте в виду, что все типы проверки подлинности, недоступны в случае каждой операционной системы. Кроме того Если вы используете IIS 7.0 в Windows Vista, необходимо будет включить различные типы проверки подлинности Windows, прежде чем они появятся в диспетчер служб IIS. Откройте **панели управления, программы, программы и компоненты Windows, включение или отключение компонентов**и разверните узел Internet Information Services (см. рис. 2).

**Рис. 2 – функции Enabling Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

С помощью служб IIS, можно включить или отключить различные типы проверки подлинности. Например на рис. 3 показана отключить анонимную проверку подлинности и включение проверки подлинности встроенная Windows (NTLM), при использовании IIS 7.0.

**Рис. 3 – Включение проверки подлинности встроенная Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Авторизация Windows пользователей и групп

После включения проверки подлинности Windows, можно использовать &lt;Authorize&gt; атрибут для управления доступом к контроллерам или действиям контроллера. Этот атрибут может применяться для всего контроллера MVC или определенного действия контроллера.

Например контроллер Home в листинге 1 предоставляет три действия, с именем Index() CompanySecrets() и StephenSecrets(). Любой пользователь может вызвать действие Index(). Однако действие CompanySecrets() можно вызвать только члены локальной группы диспетчеров Windows. Наконец действие StephenSecrets() можно вызвать только Windows пользователя домена с именем Стивен (в домене Redmond).

**В листинге 1 — Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Из-за Windows контроля учетных записей (UAC), при работе с Windows Vista или Windows Server 2008, в локальную группу администраторов будет вести себя иначе, чем другие группы. &lt;Authorize&gt; атрибута не определит правильно является членом локальной группы администраторов Если вы не измените параметры контроля учетных Записей компьютера.


Точно что произойдет при попытке вызвать действие контроллера, не являясь нужных разрешений зависит от типа проверки подлинности включена. По умолчанию, при использовании ASP.NET Development Server вы просто получите пустую страницу. Странице, доставленной с **401 Нет прав** состояние ответа HTTP.

Если, с другой стороны, при использовании IIS с отключена анонимная проверка подлинности и обычную проверку подлинности включена, то появляется диалоговое окно Вход в систему каждый раз, когда запрашивается защищенную страницу (см. рис. 4).

**Рис. 4 – диалоговое окно входа обычной проверки подлинности**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Сводка

Этом руководстве описано, как можно использовать проверку подлинности Windows в контексте приложения ASP.NET MVC. Вы узнали, как включить проверку подлинности Windows в файл веб-конфигурации приложения и настройка проверки подлинности со службами IIS. Наконец, вы узнали, как использовать &lt;Authorize&gt; атрибута для ограничения доступа к действиям контроллера определенным пользователям Windows или группы.

> [!div class="step-by-step"]
> [Назад](authenticating-users-with-forms-authentication-vb.md)
> [Вперед](preventing-javascript-injection-attacks-vb.md)
