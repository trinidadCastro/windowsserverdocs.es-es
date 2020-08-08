---
title: Cadenas y localización en el centro de administración de Windows
description: Obtener información sobre cómo preparar las cadenas para la localización en el SDK del centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 565e4da69466549538c380457269304c7f1cdd5a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944925"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Cadenas y localización en el centro de administración de Windows #

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Vamos a profundizar en el SDK de extensiones del centro de administración de Windows y hablar sobre las cadenas y la localización.

Para habilitar la localización de todas las cadenas que se representan en el nivel de presentación, aproveche el archivo Strings. resjson en/src/Resources/Strings (ya está configurado). Cuando necesite agregar una nueva cadena a la extensión, agréguela a este archivo resjson como una nueva entrada. La estructura existente sigue este formato:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

Puede usar cualquier formato que desee para las cadenas, pero tenga en cuenta que el proceso de generación (el proceso que toma resjson y genera la clase TypeScript utilizable) convierte el carácter de subrayado (_) en puntos (.).

Por ejemplo, esta entrada:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Genera la siguiente estructura de descriptores de acceso:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## <a name="add-other-languages-for-localization"></a>Agregar otros idiomas para la localización ##

Para la localización a otros idiomas, es necesario crear un archivo Strings. resjson para cada idioma. Estos archivos deben ser lugares en ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson``` . Los idiomas disponibles con las carpetas correspondientes son:

| Idioma      | Carpeta      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Deutsch | de-DE |
| Inglés | es-ES |
| Español | es-ES |
| Français | fr-FR |
| Magyar | hu-HU |
| Italiano | it-IT |
| 日本語 | ja-JP |
| 한국어 | ko-KR |
| Nederlands | nl-NL |
| Polski | pl-PL |
| Português (Brasil) | pt-BR |
| Português (Portugal) | pt-PT |
| Русский | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> Si las necesidades de la estructura de archivos son diferentes dentro de LOC/output, tendrá que ajustar el localeOffset de la tarea de Gulp ' Generate-resjson-JSON-Localize ' que se encuentra en el gulpfile.js. Este desplazamiento es la profundidad de la carpeta Loc en la que se debe iniciar la búsqueda de archivos Strings. resjson.

Se dará formato a cada archivo Strings. resjson de la misma manera que se mencionó anteriormente en la parte superior de esta guía.

Por ejemplo, para incluir una localización para español, incluya esta entrada en ```\loc\output\HelloWorld\es-ES\strings.resjson``` :
```json
"HelloWorld_cim_title": "CIM Componente",
```
Siempre que agregue cadenas localizadas, se debe volver a ejecutar Gulp Generate para que aparezcan. Ejecutar:
``` cmd
gulp generate
```

Para confirmar que las cadenas se han generado, vaya a ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson``` . La entrada recién agregada aparecerá en este archivo.
Ahora, si cambia la opción de idioma en el centro de administración de Windows, podrá ver las cadenas localizadas en la extensión.
