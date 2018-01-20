---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: "ASP.NET веб-развертывания с помощью Visual Studio: преобразования файлов Web.config | Документы Microsoft"
author: tdykstra
description: "Этот учебник ряд показано развертывание ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, Пол..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a88d8f35c770b362b74f787fee2c60a7577bccb2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>ASP.NET веб-развертывания с помощью Visual Studio: файл преобразования Web.config
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Этот учебник ряд показано развертывание ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010. Сведения о последовательности см. в разделе [в первом учебнике ряда](introduction.md).


## <a name="overview"></a>Обзор

Этот учебник показывает, как автоматизировать процесс изменения *Web.config* файла при его развертывании в другой целевой среды. Большинство приложений имеют параметры *Web.config* файл, должны быть разными, при развертывании приложения. Автоматизация процесса внесения этих изменений позволяет из того, чтобы сделать их вручную каждый раз при развертывании, который может оказаться трудоемкой и ошибкам.

Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Преобразования Web.config и параметры веб-развертывания

Существует два способа для автоматизации процесса изменения *Web.config* параметры: [преобразования Web.config](https://msdn.microsoft.com/en-us/library/dd465326.aspx) и [параметры веб-развертывания](https://msdn.microsoft.com/en-us/library/ff398068.aspx). Объект *Web.config* преобразования файл содержит XML-разметку, которая определяет способ изменения *Web.config* файла при его развертывании. Можно указать различные изменения для конкретной конфигурации построения и для конкретных профилей публикации. По умолчанию конфигурации построения, отладки и выпуска, и можно создать пользовательские конфигурации построения. Профиль публикации обычно соответствует в целевой среде. (Вы узнаете о дополнительных сведений о публикации профилей в [развертывание в IIS в тестовой среде](deploying-to-iis.md) учебник.)

Параметры веб-развертывания можно использовать для указания различных параметров, которые должны быть настроены во время развертывания, включая параметры, которые находятся в *Web.config* файлов. Используемый для задания *Web.config* изменения в файле, для настройки параметров веб-развертывание более сложны, но они полезны, если вы не знаете значение для установки, пока не будет развернут. Например, в среде предприятия можно создать *пакета развертывания* и присвойте ему человека в ИТ-отдел для установки в рабочей среде и у другого пользователя есть возможность введите строки подключения или пароли, которые не знаете.

Для сценария, в котором рассматриваются этого учебника ряда, известно заранее все, что необходимо сделать для *Web.config* файл, поэтому необходимо использовать параметры веб-развертывания. Следует настроить некоторые преобразования, различаются в зависимости от конфигурации построения, который используется, и некоторые, различаются в зависимости от того, используется профиль публикации.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Задание настроек Web.config в Azure

Если *Web.config* файла настройки, необходимо изменить находятся в `<connectionStrings>` или `<appSettings>` элемент, и при развертывании веб-приложений в службе приложений Azure имеется другой параметр для автоматического изменения во время развертывание. Можно ввести параметры, которые вы хотите вступают в силу в Azure в **Настройка** страницы портала управления для веб-приложения (Прокрутите вниз до **параметры приложения** и **строки подключения**  разделы). При развертывании проекта Azure автоматически применяет изменения. Дополнительные сведения см. в разделе [веб-сайтов Windows Azure: как строки приложения и строк соединения работает](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>По умолчанию файлы преобразования

В **обозревателе решений**, разверните *Web.config* для просмотра *Web.Debug.config* и *Web.Release.config* преобразования файлов создаются по умолчанию для конфигурации построения два заданных по умолчанию.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Можно создать файлы преобразования для конфигураций настраиваемой сборки, щелкнув правой кнопкой мыши файл Web.config и выбрав **добавить преобразования конфигурации** в контекстном меню. В этом учебнике не нужно делать, и команды меню отключено, так как вы еще не создали все конфигурации настраиваемого построения.

Позже вы создадите три дополнительные преобразования файла, по одной для тестирования, промежуточного хранения и рабочей профилей публикации. Типичным примером параметра, который будет обрабатываться в файл преобразования профиль публикации, так как он зависит от целевой среды является конечной точки WCF, отличается для теста и производства. Вы создадите публикации файлов преобразования профиля на следующих занятиях рассматривается после создания профили публикации, которые переходят с.

## <a name="disable-debug-mode"></a>Отключите режим отладки

Пример установки, который зависит от конфигурации построения, а не целевой среде `debug` атрибута. Для сборки выпуска обычно требуется отключенной отладкой независимо от того, какая среда выполняется развертывание. Таким образом, создание шаблонов проектов Visual Studio по умолчанию *Web.Release.config* преобразования файлов с кодом, который удаляет `debug` атрибута из `compilation` элемента. Значение по умолчанию здесь *Web.Release.config*: помимо образец кода преобразования, помещается в комментарий, он включает в себя код в `compilation` элемент, который удаляет `debug` атрибут:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"` Атрибут указывает, что `debug` атрибут, который необходимо удалить из `system.web/compilation` элемент в развернутой *Web.config* файла. Это будет выполняться каждый раз при развертывании сборки выпуска.

## <a name="limit-error-log-access-to-administrators"></a>Ограничение доступа к журналу ошибок для администраторов

Если возникает ошибка во время выполнения приложения, приложение отображает страницу универсальное сообщение об ошибке вместо ошибки, сформированные системой и использует [пакет Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) для регистрации ошибок и отчетности. `customErrors` Элемента в приложении *Web.config* файла указывает на страницу ошибки:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Чтобы открыть страницу ошибки, временно изменить `mode` атрибут `customErrors` элемента «RemoteOnly» для «On» и запустите приложение из Visual Studio. Вызывает ошибку, запрашивая недопустимый URL-адрес, например *Studentsxxx.aspx*. Вместо ошибки созданный IIS «не удается найти ресурс» страница отображается *GenericErrorPage.aspx* страницы.

![Страницы ошибок](web-config-transformations/_static/image2.png)

Чтобы просмотреть журнал ошибок, замените все содержимое в URL-адрес после номера порта с *elmah.axd* (например, `http://localhost:51130/elmah.axd`) и нажмите клавишу ВВОД:

![ELMAH страницы](web-config-transformations/_static/image3.png)

Не забудьте задать `customErrors` элемент обратно в режим «RemoteOnly» после завершения.

На компьютере разработчика удобно Разрешить свободный доступ к странице журнала ошибки, но в производственной среде, может представлять угрозу безопасности. Для сайта рабочей среде, необходимо добавить правило авторизации, которое ограничивает доступ к журналу ошибок для администраторов и убедитесь, что ограничение работает вы хотите его в тесте, а также промежуточной. Поэтому это другое изменение, необходимо реализовать каждый раз при развертывании сборки выпуска, и поэтому она принадлежит к *Web.Release.config* файла.

Откройте *Web.Release.config* и добавьте новую `location` элемент непосредственно перед закрывающим тегом `configuration` тег, как показано ниже.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` Значение атрибута «Insert» вызывает это `location` добавлен как одноуровневый для любой существующий элемент `location` элементов в *Web.config* файла. (Уже имеется один `location` элемент, который задает авторизации правил для **обновление кредиты** страницы.)

Теперь можно просматривать преобразование, убедитесь, что закодирован его правильно.

В **обозревателе решений**, щелкните правой кнопкой мыши *Web.Release.config* и нажмите кнопку **преобразования предварительной версии**.

![Преобразование меню предварительного просмотра](web-config-transformations/_static/image4.png)

Открывается страница, которые показывает разработки *Web.config* файла слева, а также то, что развертывание *Web.config* файла будет выглядеть как в правой части с изменения выделены.

![Предварительный просмотр преобразования отладки](web-config-transformations/_static/image5.png)

![Предварительный просмотр расположение преобразования](web-config-transformations/_static/image6.png)

(В области предварительного просмотра можно заметить некоторые дополнительные изменения чужим преобразующий: обычно они включают Удаление пробелов, не влияет на функциональные возможности.)

При тестировании сайта после развертывания, вы также сможете протестировать для убедитесь, что правило авторизации является эффективным.

> [!NOTE] 
> 
> **Примечание по безопасности** никогда не отображать сведения об ошибке для просмотра в реальном приложении, или хранения этих сведений в общедоступном расположении. Злоумышленники могут использовать сведения об ошибках для обнаружения уязвимостей в узле. Если вы используете ELMAH в приложении, настройте ELMAH, чтобы свести к минимуму угрозы безопасности. В примере ELMAH в этом учебнике не следует считать Рекомендуемая конфигурация. Это пример, в котором был выбран для демонстрации обработки необходимо, приложение может создавать файлы в папку. Дополнительные сведения см. в разделе [защиты конечной точки ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Параметр, который будет обрабатывать в публикации профиль преобразования файлов

Распространенным сценарием является возможность *Web.config* параметры, которые должны быть разными в каждой среде, которое развертывается в файл. Например приложение, которое вызывает службу WCF может потребоваться другой конечной точке в тестовой и рабочей сред. Приложение Contoso университета также включает параметр этого типа. Этот параметр определяет, отображается индикатор на страницах сайта о том, какая среда используется, например разработки, тестирования или производства. Значение параметра определяет приложения, которые будут добавлены «(Dev)» или «(Test)» для основного заголовка в *Site.Master* главной страницы:

![Индикатор среды](web-config-transformations/_static/image7.png)

Индикатор среды учитывается, если приложение выполняется в промежуточной или рабочей среде.

Веб-страницы университета Contoso чтения значение, которое задано в `appSettings` в *Web.config* файл, чтобы определить, какой приложение выполняется в политике:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Значение должно быть «Test» в тестовой среде и «Вход» для промежуточных и производственных.

Следующий код в файл преобразования будет реализовать это преобразование:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform` «SetAttributes» указывает, что это преобразование предназначено для изменения значений атрибутов существующего элемента в значении атрибута *Web.config* файла. `xdt:Locator` Значение «Match(key)» указывает, что элемента для изменения одного атрибута которого `key` атрибут соответствует `key` атрибут, указанный здесь. Только атрибут из `add` элемент `value`, и что будут изменены в развернутой *Web.config* файла. Код, показанный здесь причины `value` атрибут `Environment` `appSettings` элемента требуется задать значение «Test» *Web.config* файл, который развертывается.

Данное преобразование входит в файлах преобразования профиль публикации, которые еще не созданы. Предстоит создать и изменить файлы преобразования, для реализации этого изменения при создании профилей публикации для тестирования, промежуточного хранения и рабочей сред. Вы сможете сделать это в [развертывание в IIS](deploying-to-iis.md) и [развертывания в рабочей среде](deploying-to-production.md) учебники.

> [!NOTE]
> Поскольку этот параметр находится в `<appSettings>` элемент, у вас есть другой способ для указания преобразование при развертывании веб-приложений в разделе службы приложения Azure [параметры файла Web.config, указав в Azure](#watransforms) ранее в в этом разделе.


## <a name="setting-connection-strings"></a>Задание строк подключения

Несмотря на то, что файл преобразования по умолчанию содержит пример, в котором показано, как обновить строку подключения, в большинстве случаев вы не обязательно для настройки преобразования строки подключения, так как строки подключения можно указать в профиле публикации. Вы сможете сделать это в [развертывание в IIS](deploying-to-iis.md) и [развертывания в рабочей среде](deploying-to-production.md) учебники.

## <a name="summary"></a>Сводка

Вы теперь выполняются так же, как можно *Web.config* преобразования, прежде чем создавать профили публикации, и вы знаете, какие будут в развернутом файле Web.config для предварительного просмотра.

![Предварительный просмотр расположение преобразования](web-config-transformations/_static/image8.png)

В этом руководстве вы будете аккуратно задач настройки развертывания, которые требуется установить для свойства проекта.

## <a name="more-information"></a>Дополнительные сведения

Дополнительные сведения о подразделах, рассматриваемых в этом учебнике см. в разделе [преобразования с помощью Web.config, чтобы изменить параметры в файле app.config или в файл Web.config во время развертывания](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) в Web Карта содержимого развертывания для Visual Studio и ASP.NET.

>[!div class="step-by-step"]
[Назад](preparing-databases.md)
[Вперед](project-properties.md)