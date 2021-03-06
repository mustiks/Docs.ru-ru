---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Отображение видео в веб-ASP.NET страниц узла (Razor) | Документация Майкрософт
author: Rick-Anderson
description: В этой главе описывается отображение видео в ASP.NET Web Pages с Razor синтаксис страницы.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 8f4b7186ae5c7b7b384ebcb23f7c9ad65caeb0bd
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021044"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Отображение видео на сайте ASP.NET Web Pages (Razor)
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В этой статье объясняется, как с помощью проигрывателя видео (media) в на веб-сайте ASP.NET Web Pages (Razor) позволяют пользователям просматривать видео, которые хранятся на сайте. Веб-страниц ASP.NET с синтаксисом Razor позволяет воспроизводить Flash (*.swf*), Media Player (*.wmv*) и Silverlight (*.xap*) видео.
> 
> Вы узнаете, как:
> 
> - Как выбрать видеопроигрывателя.
> - Как добавить видео на веб-страницу.
> - Как задать атрибуты видеопроигрывателя.
> 
> Это в коде ASP.NET Razor pages функций, появившихся в этой статье:
> 
> - `Video` Вспомогательный.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-страниц ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Этот учебник также работает с WebMatrix 3.


## <a name="introduction"></a>Вступление

Может потребоваться отображение видео на сайте. Один из способов сделать это — связь с сайтом, уже содержат видео, таких как YouTube. Если вы хотите внедрить видео с этих сайтов непосредственно на собственных страницах, можно обычно получить разметки HTML с сайта и скопируйте его на страницу. Например приведенный ниже показано, как внедрить YouTube видео:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Если вы хотите воспроизвести видео на свой веб-сайт (не на общедоступный сайт обмена), нельзя связать непосредственно к ней с помощью внедренных разметки следующим образом. Тем не менее, воспроизведение видео с веб-узла с помощью `Video` вспомогательного приложения, который отображает проигрыватель мультимедиа непосредственно на страницу.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Выбор видеопроигрывателя

Существует множество форматов видеофайлов, и каждый формат обычно требует разных игрока и другой способ настройки проигрывателя. В ASP.NET Razor pages, можно воспроизводить видео в веб-страницы с помощью `Video` вспомогательный. `Video` Вспомогательный упрощает процесс внедрения видео на веб-странице, так как он автоматически создает `object` и `embed` элементы HTML, которые обычно используются для добавления видео к странице.

`Video` Вспомогательный поддерживает следующие проигрыватели мультимедиа:

- Adobe Flash
- Проигрывателя Windows Media
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash` Исполнитель `Video` вспомогательный позволяют воспроизводить видео, флэш-памяти (*.swf* файлов) на веб-странице. Как минимум необходимо указать путь к видеофайлу. Если указать только путь, проигрыватель использует значения по умолчанию, установленные с текущей версией Flash. Ниже перечислены параметры по умолчанию.

- Видео отображается с использованием высоту и ширину по умолчанию и без использования цвета фона.
- Видео воспроизводится автоматически при загрузке страницы.
- Видео циклов непрерывно, пока не был явно остановлен.
- Видео масштабируется для отображения всех видео, а не обрезать видео в соответствии с определенным размером.
- В окне воспроизведения видео.

### <a name="the-mediaplayer-player"></a>Проигрыватель MediaPlayer

`MediaPlayer` Исполнитель `Video` вспомогательный позволяет воспроизводить видео Windows Media (*.wmv* файлы), Windows Media audio (*.wma* файлы) и MP3 (*.mp3* файлы) на веб-странице. Необходимо включить путь к файлу мультимедиа для воспроизведения; все остальные параметры являются необязательными. Если указать только путь, проигрыватель использует параметры по умолчанию, задать в текущей версии MediaPlayer, например:

- Видео отображается с использованием высоту и ширину по умолчанию.
- Видео воспроизводится автоматически при загрузке страницы.
- Однократное воспроизведение видео (он не цикла).
- Проигрыватель отображает полный набор элементов управления в пользовательском интерфейсе.
- В окне воспроизведения видео.

### <a name="the-silverlight-player"></a>Проигрыватель Silverlight

`Silverlight` Исполнитель `Video` вспомогательный позволяет воспроизводить Windows Media Video (*.wmv* файлы), Windows Media Audio (*.wma* файлы) и MP3 (*.mp3* файлы). Необходимо задать параметр path для указания пакета приложения Silverlight (*.xap* файла). Также необходимо задать параметры width и height. Все остальные параметры являются необязательными. При использовании проигрывателя Silverlight для видео, если выбрать только необходимые параметры проигрывателя Silverlight отображает видео без цвета фона.

> [!NOTE]
> В случае, если вы еще не знаете Silverlight: *.xap* файл — это сжатый файл, содержащий инструкции макета в *.xaml* файл, управляемый код в сборках и дополнительных ресурсов. Можно создать *.xap* файл в Visual Studio, как проект приложения Silverlight.


`Silverlight` Видеопроигрывателя использует как параметры, указываемые для проигрывателя, так и параметры, которые предоставляются в *.xap* файл.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Типы MIME
> 
> Когда браузер загружает файл, браузер гарантирует, что тип файла соответствует типу MIME, указанный для документа, который готовится к просмотру. Тип MIME — тип содержимого типа или носителя файла. `Video` Вспомогательная функция использует следующие типы MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Воспроизведение видео Flash (.swf).

Эта процедура показано, как воспроизвести видео с флэш-памяти с именем *sample.swf*. В процедуре предполагается, что у вас есть папка с именем *мультимедиа* на сайт и что *.swf* файл находится в этой папке.

1. Добавьте библиотеку ASP.NET на веб-сайт, как описано в [Установка вспомогательных функций на сайте веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если не было добавлено.
2. В веб-сайта, добавление страницы и назовите его *FlashVideo.cshtml*.
3. Добавьте следующую разметку страницы: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Откройте страницу в браузере. (Убедитесь, что выбран страницы **файлы** рабочей области, прежде чем запускать его.) Эта страница отображается, и автоматически воспроизводит видео. 

    ![[изображение]](10-working-with-video/_static/image1.jpg "ch08_video 1.jpg")

Можно задать `quality` параметр Flash видео для `low`, `autolow`, `autohigh`, `medium`, `high`, и `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Вы можете изменить видео в формате Flash для воспроизведения на определенного размера с помощью `scale` параметр, который можно задать следующее:

- `showall`. Это делает видео полностью видимыми, сохраняя исходные пропорции. Тем не менее вы может получиться границы каждой стороны.
- `noorder`. При этом размер видео, сохраняя исходные пропорции, но может обрезаться.
- `exactfit`. При этом всего видео отображаются без сохранения исходные пропорции, но наблюдается искажение.

Если вы не укажете `scale` параметра всего видео будут отображаться и исходные пропорции сохраняются без всякой обрезки. В следующем примере показано, как использовать `scale` параметр:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player поддерживает видео режим параметр `windowMode`. Вы можете задать `window`, `opaque`, и `transparent`. По умолчанию `windowMode` присваивается `window`, отображающий видео в отдельном окне веб-страницы. `opaque` Параметр скрывает все за видео на веб-странице. `transparent` Параметр позволяет фона веб-страницы, видны сквозь видео, при условии, что любой части видео является прозрачным.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Воспроизведение MediaPlayer (*.wmv*) видео

Ниже показано, как воспроизвести видео мультимедиа окно с именем *sample.wmv* , который находится в *мультимедиа* папки.

1. Добавьте библиотеку ASP.NET на веб-сайт, как описано в [Установка вспомогательных функций на сайте веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если у вас еще нет.
2. Создать новую страницу с именем *MediaPlayerVideo.cshtml*.
3. Добавьте следующую разметку страницы: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Откройте страницу в браузере. Видео, загружает и воспроизводится автоматически. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Можно задать `playCount` в целое число, указывающее, сколько раз для воспроизведения видео автоматически:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode` Позволяет указать, какие элементы управления отображаются в пользовательском интерфейсе. Можно задать `uiMode` для `invisible`, `none`, `mini`, или `full`. Если не указать `uiMode` , видео будет отображаться с окном состояния, поиска панели управления кнопки и регуляторов громкости в дополнение к видео окна. Эти элементы управления отображается также в том случае, если использовать проигрыватель для воспроизведения звукового файла. Ниже приведен пример использования `uiMode` параметр:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

По умолчанию аудио включен при воспроизведении видео. Отключение звука аудио, задав `mute` значение true для параметра:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Уровень звука видео MediaPlayer можно управлять, задав `volume` параметр в значение от 0 до 100. Значение по умолчанию — 50. Ниже приведен пример:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Воспроизведение видео Silverlight

Эта процедура показано, как воспроизвести видео, содержащихся в Silverlight *.xap* страницу, которая находится в папке с именем *мультимедиа*.

1. Добавьте библиотеку ASP.NET на веб-сайт, как описано в [Установка вспомогательных функций на сайте веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если у вас еще нет.
2. Создать новую страницу с именем *SilverlightVideo.cshtml*.
3. Добавьте следующую разметку страницы: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Откройте страницу в браузере. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы


[Обзор решения Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash атрибуты тега ОБЪЕКТА и ВНЕДРЕНИЯ](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM теги](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
