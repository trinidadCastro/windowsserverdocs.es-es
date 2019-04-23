---
title: Las cadenas y localización en Windows Admin Center
description: Obtener las cadenas preparada para la localización en Windows Admin Center SDK (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fb328f86c98141a5a2a1c4fd05ec1d4c96a7bc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845406"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Las cadenas y localización en Windows Admin Center #

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Sigamos más detallada en el SDK de extensiones de Windows Admin Center y hablar sobre las cadenas y localización.

Para habilitar la localización de todas las cadenas que se representan en la capa de presentación, aproveche las ventajas del archivo strings.resjson en /src/resources/strings - lo ya está configurado. Cuando necesite agregar una nueva cadena a su extensión, agregue a este archivo resjson como una nueva entrada. La estructura existente sigue este formato:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

Puede usar cualquier formato que desee para las cadenas, pero tenga en cuenta que el proceso de generación (es decir, el proceso que toma el resjson y genera la clase TypeScript utilizable) convierte un carácter de subrayado (_) en puntos (.).

Por ejemplo, esta entrada:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Genera la siguiente estructura de descriptor de acceso:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```
