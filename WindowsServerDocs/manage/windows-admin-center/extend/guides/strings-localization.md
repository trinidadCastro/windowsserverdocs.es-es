---
title: Cadenas y localización en el centro de administración de Windows
description: Obtener información sobre cómo preparar las cadenas para la localización en el SDK del centro de administración de Windows (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 61289ae175ca8b906386cff9e36f5023ea28d051
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385238"
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

Para la localización a otros idiomas, es necesario crear un archivo Strings. resjson para cada idioma. Estos archivos deben ser lugares en ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```. Los idiomas disponibles con las carpetas correspondientes son:

| Lenguaje      | Carpeta      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Deutsch | de-DE |
| Inglés | en-US |
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
> Si las necesidades de la estructura de archivos son diferentes dentro de LOC/output, tendrá que ajustar el localeOffset de la tarea de Gulp ' Generate-resjson-JSON-Localize ' que se encuentra en gulpfile. js. Este desplazamiento es la profundidad de la carpeta Loc en la que se debe iniciar la búsqueda de archivos Strings. resjson.

Se dará formato a cada archivo Strings. resjson de la misma manera que se mencionó anteriormente en la parte superior de esta guía. 

Por ejemplo, para incluir una localización para español, incluya esta entrada ```\loc\output\HelloWorld\es-ES\strings.resjson```en: 
```json
"HelloWorld_cim_title": "CIM Componente",
```
Siempre que agregue cadenas localizadas, se debe volver a ejecutar Gulp Generate para que aparezcan. Ejecute:
``` cmd
gulp generate 
```

Para confirmar que las cadenas se han generado, vaya ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```a. La entrada recién agregada aparecerá en este archivo.
Ahora, si cambia la opción de idioma en el centro de administración de Windows, podrá ver las cadenas localizadas en la extensión. 
