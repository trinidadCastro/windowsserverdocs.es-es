---
title: WDSUtil Get-allimages
description: Artículo de referencia para el comando WDSUtil Get-allimages, que recupera información acerca de todas las imágenes de un servidor.
ms.topic: reference
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2fe93036c5493baed122fede4b25b71349996536
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614937"
---
# <a name="wdsutil-get-allimages"></a>WDSUtil Get-allimages

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información acerca de todas las imágenes de un servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /get-allimages [/server:<servername>] /show:{boot | install | legacyris | all} [/detailed]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[/server:<servername>]` | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| `/show:{boot | install | legacyris | all}` | Donde **boot** solo devuelve imágenes de arranque, **install** devuelve las imágenes de instalación, así como información acerca de los grupos de imágenes que los contienen, **LegacyRis** solo devuelve imágenes de los servicios de instalación remota (RIS) y **All** devuelve información de la imagen de arranque, instala información de la imagen (incluida la información sobre los grupos de imágenes) e información de la imagen de RIS. |
| [/detailed] | Indica que se deben devolver todos los metadatos de imagen de cada imagen. Si no se usa esta opción, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo. |

## <a name="examples"></a>Ejemplos

Para ver información acerca de las imágenes, escriba:

```
wdsutil /get-allimages /show:install
```

```
wdsutil /verbose /get-allimages /server:MyWDSServer /show:all /detailed
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil-comando Add-image](wdsutil-add-image.md)

- [comando de copia de imagen de WDSUtil](wdsutil-copy-image.md)

- [comando WDSUtil Export-Image](wdsutil-export-image.md)

- [comando WDSUtil Remove-image](wdsutil-remove-image.md)

- [WDSUtil-comando Replace-Image](wdsutil-replace-image.md)

- [WDSUtil Set-Image (comando)](wdsutil-set-image.md)
