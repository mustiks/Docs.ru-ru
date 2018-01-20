---
uid: signalr/overview/security/introduction-to-security
title: "Общие сведения о безопасности SignalR | Документы Microsoft"
author: pfletcher
description: "Описывает вопросы безопасности, которые следует учитывать при разработке приложения SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: ffe71f8ea7105db4d5a0c156e2b4e76d6e40761d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-signalr-security"></a>Общие сведения о безопасности SignalR
====================
по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> В этой статье описаны вопросы безопасности, которые следует учитывать при разработке приложения SignalR. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR версии 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Предыдущие версии этого раздела
> 
> Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Обзор

Этот документ содержит следующие разделы.

- [Основные понятия безопасности SignalR](#concepts)

    - [Проверка подлинности и авторизация](#authentication)
    - [Маркер подключения](#connectiontoken)
    - [Если повторное подключение, повторное присоединение групп](#rejoingroup)
- [Как SignalR предотвращает межсайтовой подделки запроса](#csrf)
- [Рекомендации по обеспечению безопасности SignalR](#recommendations)

    - [Безопасный протокол слои сокетов (SSL)](#ssl)
    - [Не используйте группы в качестве механизма обеспечения безопасности](#groupsecurity)
    - [Безопасно обработки входные данные от клиентов](#input)
    - [Согласование изменений в состояние пользователей с активным соединением](#reconcile)
    - [Автоматически созданные файлы JavaScript прокси-сервера](#autogen)
    - [Исключения](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Основные понятия безопасности SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Аутентификация и авторизация

SignalR не предоставляет функций для проверки подлинности пользователей. Вместо этого функции SignalR интегрируются в существующую структуру проверки подлинности для приложения. Проверка подлинности пользователей будет обычным в приложении, а также работы с результатами проверки подлинности в вашей SignalR коду. Например может проверять подлинность пользователей с помощью проверки подлинности форм ASP.NET и на концентраторе, принудительно применить пользователей, которые или ролей авторизованы для вызова метода. На концентраторе можно также передать данные проверки подлинности, такие как имя пользователя или принадлежности пользователя к роли, для клиента.

SignalR обеспечивает [авторизовать](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) атрибут, чтобы указать, какие пользователи имеют доступ к концентратору или метода. К концентратору или отдельные методы в концентраторе применить атрибут Authorize. Без атрибута авторизовать все открытые методы концентратора доступны для клиента, который подключен к концентратору. Дополнительные сведения о концентраторах см. в разделе [проверки подлинности и авторизации для концентраторов SignalR](hub-authorization.md).

Можно применить `Authorize` атрибут концентраторы, но не постоянные подключения. Для применения правила авторизации при использовании `PersistentConnection` необходимо переопределить `AuthorizeRequest` метод. Дополнительные сведения о постоянные подключения см. в разделе [проверки подлинности и авторизации для постоянного подключения SignalR](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Маркер подключения

SignalR уменьшает риск выполнения вредоносных команд путем проверки подлинности источника. Для каждого запроса клиент и сервер передать маркер подключения, которая содержит идентификатор соединения и имя пользователя для прошедших проверку пользователей. Идентификатор соединения уникально идентифицирует каждый подключенный клиент. Идентификатор подключения случайным образом формирует сервер, если создается новое соединение и сохраняется этого идентификатора в течение всего соединения. Механизм проверки подлинности для веб-приложения предоставляет имя пользователя. Для защиты маркер подключения SignalR использует шифрования и цифровой подписи.

![](introduction-to-security/_static/image2.png)

Для каждого запроса сервер проверяет содержимое маркера, убедитесь что запрос поступает из указанного пользователя. Имя пользователя должно соответствовать идентификатор подключения. Проверив идентификатор подключения и имя пользователя, SignalR мешает злоумышленникам легко олицетворение другого пользователя. Если сервер не удается проверить маркер подключения, запрос завершается ошибкой.

![](introduction-to-security/_static/image4.png)

Так как идентификатор подключения является частью процесса проверки, не следует раскрывать один пользователь идентификатор подключения для других пользователей или сохраняет значение на клиенте, например, в файле cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Если повторное подключение, повторное присоединение групп

По умолчанию SignalR приложение будет автоматически повторно назначить пользователя в соответствующие группы при повторном подключении от временного прерывания, таких как после удалены и повторно установить подключение времени ожидания соединения. При повторном подключении, клиент передает токен группы, который включает идентификатор соединения и назначенной группы. Токен группы цифровой подписывается и шифруется. Клиент сохраняет тот же идентификатор соединения после повторного подключения; Таким образом идентификатор соединения, переданные от повторного подключения клиента должен соответствовать предыдущих идентификатор соединения, используемый клиентом. Такая проверка предотвращает злонамеренный пользователь передача запросы на присоединение несанкционированного групп при повторном подключении.

Однако важно отметить, маркер группы не истек. Если пользователь принадлежал к группе в прошлом, но был заблокированный из этой группы, этот пользователь может быть возможность имитировать группы маркер, который содержит запрещенные группы. Если необходимо безопасно управлять какие пользователи являются членами групп, которые необходимо сохранять данные на сервере, например, в базе данных. Затем добавьте логику для приложения, который проверяет на сервере, является ли пользователь принадлежит к группе. Пример проверка членства в группе см. в разделе [работа с группами](../guide-to-the-api/working-with-groups.md).

Автоматическое повторное присоединение групп применяется только после повторного подключения после временному перерыву в предоставлении. Если пользователь отключает по покидая приложение или повторного запуска приложения, приложения должны обрабатывать как добавить этого пользователя в требуемые группы. Дополнительные сведения см. в разделе [работа с группами](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Как SignalR предотвращает межсайтовой подделки запроса

Подделки межсайтовых запросов (CSRF) — это атака, при которой вредоносный сайт отправляет запрос уязвимым сайта, где пользователь вошел в данный момент в. SignalR предотвращает CSRF, делая крайне маловероятно, что для вредоносный сайт для создания запроса на допустимый для приложения SignalR.

### <a name="description-of-csrf-attack"></a>Описание атаки CSRF

Ниже приведен пример атаки CSRF:

1. Пользователь входит в www.example.com, с помощью форм проверки подлинности.
2. Сервер проверяет подлинность пользователя. Ответ от сервера включает файл cookie проверки подлинности.
3. Не выходя из пользователь посещает вредоносный веб-узел. Этот вредоносный сайт содержит следующие HTML-формы: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 Обратите внимание, что действие формы в блогах уязвимым сайте, не вредоносный сайт. Это часть CSRF «между сайтами».
4. При нажатии кнопки "Отправить". Браузер включает файл cookie проверки подлинности с запросом.
5. Запрос выполняется на сервере example.com с контекстом проверки подлинности пользователя и могут выполнять все, что разрешено делать прошедшего проверку подлинности пользователя.

Несмотря на то, что в этом примере требует от пользователя, нажмите кнопку «форма», страницу вредоносных удалось так же, как легко запускать сценарий, который отправляет запрос AJAX в приложение SignalR. Кроме того с помощью протокола SSL не запрещает атаки CSRF, так как вредоносный сайт можно отправить запрос «https://».

Как правило атаки CSRF для веб-сайтов, которые используют файлы cookie для проверки подлинности, так как браузеры отправляют все соответствующие файлы cookie в веб-узел назначения. Тем не менее атаки CSRF ограничены не использовать файлы cookie. Например Basic и дайджест-проверки подлинности также уязвимы. После входа пользователя в систему с обычная или краткая проверка подлинности браузер автоматически отправляет учетные данные до завершения сеанса.

### <a name="csrf-mitigations-taken-by-signalr"></a>Возможные способы устранения CSRF выполняемое SignalR

SignalR принимает следующие шаги, чтобы предотвратить создание допустимых запросов к приложению вредоносный сайт. SignalR принимает следующие действия по умолчанию, необходимо предпринимать никаких действий в коде.

- **Отключение запросов между доменами**  
 SignalR отключает междоменные запросы, чтобы запретить пользователям вызов конечной точки SignalR из внешнего домена. SignalR считает, что любой запрос из внешнего домена к недействительности и блокирует запрос. Мы рекомендуем оставить поведение по умолчанию; в противном случае вредоносный сайт может обманом заставить пользователей отправки команд на узел. Если необходимо использовать междоменные запросы, см. раздел [как для установления соединения между доменами](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Передать маркер подключения в строке запроса, а не файл cookie**  
 SignalR передает маркер подключения как значение строки запроса, а не как файл cookie. Хранение маркер подключения в файле cookie небезопасна, поскольку можно случайно перехода вперед в обозревателе маркер подключения при обнаружении вредоносных программ. Кроме того передавая маркер подключения в строке запроса запрещает маркер подключения сохранение за пределами текущего соединения. Таким образом пользователь-злоумышленник не может сделать запрос под учетными данными другого пользователя.
- **Проверьте маркер подключения**  
 Как описано в [маркер подключения](#connectiontoken) раздел сервер знает, какой идентификатор соединения связан с каждого проверенного пользователя. Сервер не обрабатывает все запросы от идентификатор соединения, который не соответствует имени пользователя. Маловероятно, пользователь-злоумышленник может узнать допустимый запрос, так как пользователь-злоумышленник нужно знать имя пользователя и идентификатор текущего подключения формируется случайным образом. Этот идентификатор соединения становится недействительным, сразу после завершения соединения. Анонимных пользователей нет доступа к конфиденциальным сведениям.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Рекомендации по обеспечению безопасности SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Безопасный протокол слои сокетов (SSL)

Протокол SSL использует шифрование для защиты передачи данных между клиентом и сервером. Если приложение SignalR передает конфиденциальные данные между клиентом и сервером, использование протокола SSL для транспорта. Дополнительные сведения о настройке SSL см. в разделе [настройке SSL в службах IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Не используйте группы в качестве механизма обеспечения безопасности

Группы представляют собой удобный способ сбора связанных пользователей, но они не являются безопасный механизм для ограничения доступа к конфиденциальной информации. Это особенно важно в тех случаях, когда пользователи могут автоматически повторно присоединиться к группам во время переподключения. Вместо этого рассмотрите возможность добавления привилегированных пользователей к роли и ограничение доступа к метода концентратора, чтобы только члены этой роли. Пример ограничения доступа на основе роли см. в разделе [проверки подлинности и авторизации для концентраторов SignalR](hub-authorization.md). Пример проверки доступа пользователей к группам, при повторном подключении см. в разделе [работа с группами](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Безопасно обработки входные данные от клиентов

Чтобы убедиться, что пользователь-злоумышленник не отправляет скрипт для других пользователей, необходимо закодировать все входные данные от клиентов, предназначенной для широковещательной рассылки для других клиентов. Так как приложение SignalR может иметь много различных типов клиентов следует кодировать сообщения на получающей клиентов, а не на сервере. Таким образом HTML-кодирование работает для веб-клиента, но не для других типов клиентов. Например, веб-метода клиента для отображения сообщения чата будет безопасно обрабатывать имя пользователя и сообщение путем вызова `html()` функции.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Согласование изменений в состояние пользователей с активным соединением

Если состояние проверки подлинности пользователя, изменяет время существования активного соединения, пользователь получит сообщение об ошибке, «идентификатор пользователя невозможно изменить во время активного подключения SignalR». В этом случае приложение должно повторного подключения к серверу, чтобы убедиться в том, что идентификатор соединения и имени пользователя будут соответствовать друг другу. Например если приложение позволяет пользователю выйти из системы, пока существует активное соединение, пользователя для соединения больше не будет соответствовать имени, переданный для следующего запроса. Будет остановить соединения, прежде чем пользователь выходит из системы и перезапустите ее.

Однако важно отметить, что большинство приложений не потребуется вручную остановить и запустить подключение. Если приложение перенаправляет пользователей на отдельную страницу после входа, такие как поведение по умолчанию в MVC-приложений или приложений веб-форм или обновляет текущую страницу после выхода, активное подключение будет автоматически отключен и не делает требуются какие-либо действия.

В следующем примере показано, как остановить и запустить подключение при изменении состояния пользователя.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Или состояние проверки подлинности пользователя может измениться, если веб-сайт использует скользящий срок действия с помощью форм и активности, не следует допустимый файл cookie проверки подлинности. В этом случае пользователь будет иметь вышел из системы и имя пользователя больше не будет соответствовать имени пользователя в маркер подключения. Эту неполадку можно устранить путем добавления определенный скрипт, который периодически запрашивает ресурс на веб-сервере, чтобы сохранить допустимый файл cookie проверки подлинности. В следующем примере показано, как для запроса ресурса каждые 30 минут.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Автоматически созданные файлы JavaScript прокси-сервера

Если вы не хотите включать все концентраторам и методам в файле JavaScript прокси-сервера для каждого пользователя, можно отключить автоматическое создание файла. Можно выбрать этот параметр, если имеется несколько концентраторов и методов, но не выполнять каждого пользователя, которые следует учитывать все методы. Отключить автоматическое создание, задав **EnableJavaScriptProxies** для **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Дополнительные сведения о файлах прокси JavaScript см. в разделе [созданный прокси и делает за вас](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Исключения

Следует избегать передача объектов исключений для клиентов, так как объекты могут передавать важные данные для клиентов. Вместо этого необходимо вызовите метод на стороне клиента, отображается соответствующее сообщение об ошибке.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]