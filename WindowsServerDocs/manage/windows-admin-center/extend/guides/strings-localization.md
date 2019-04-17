---
title: Cadenas y localización en Windows Admin Center
description: Obtén información sobre cómo obtener la lista para la localización de las cadenas en el SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f0671160bd790d80e35f6d6c1586a07969939070
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296737"
---
# Cadenas y localización en Windows Admin Center #

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Vamos a profundizar más en el SDK de extensiones de Windows Admin Center y hablar sobre cadenas y localización.

Para habilitar la localización de todas las cadenas que se representan en la capa de presentación, aprovechar el archivo strings.resjson bajo /src/resources/strings - está ya configurado. Cuando se necesita agregar una nueva cadena a la extensión, agregarla a este archivo resjson como una entrada nueva. La estructura existente tiene este formato:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

Puedes usar cualquier formato que quieras para las cadenas, pero ten en cuenta que el proceso de generación (es decir, el proceso que toma la resjson y muestra la clase TypeScript utilizable) convierte el carácter de subrayado (_) a puntos (.).

Por ejemplo, esta entrada:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Genera la siguiente estructura de descriptor de acceso:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## Agregar otros idiomas para la localización ## 

Para la localización a otros idiomas, un archivo strings.resjson debe crearse para cada idioma. Estos archivos deben ser lugares en ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```. Los lenguajes disponibles con carpetas correspondientes son:

| Language      | Carpeta      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Alemán | de-DE |
| Inglés | en-US |
| Español | es-ES |
| Français | fr-FR | 
| Magiar | hu-HU | 
| Italiano | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| Países bajos | nl-NL |
| Polski | pl-PL |
| Português (Brasil) | pt-BR |
| Português (Portugal) | pt-PT |
| РУССКИЙ | ru-RU |
| Versión sueca | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> Si tus necesidades de la estructura de archivo son diferentes dentro de la ubicación y salida, tendrás que ajustar el localeOffset de la tarea de gulp 'generar resjson-json-localizado' que se encuentra en la gulpfile.js. Este desplazamiento es el punto de profundidad en la carpeta de ubicación debe iniciarse buscando strings.resjson archivos.

Cada archivo strings.resjson tendrán el formato de la misma manera, como se mencionó en la parte superior de esta guía. 

Por ejemplo, para incluir una localización para Español incluir esta entrada en ```\loc\output\HelloWorld\es-ES\strings.resjson```: 
```json
"HelloWorld_cim_title": "CIM Componente",
```
En cualquier momento que has agregado las cadenas localizadas, gulp generar debe ser ejecutado nuevo con el fin de hacer que aparezcan. Ejecutar:
``` cmd
gulp generate 
```

Para confirmar que las cadenas que se han generado navegar a ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```. La entrada recién agregada aparecerán en este archivo.
Ahora si se cambia la opción de idioma en Windows Admin Center, podrás ver las cadenas localizadas de la extensión. 
