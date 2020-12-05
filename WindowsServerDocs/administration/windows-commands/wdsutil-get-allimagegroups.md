---
title: WDSUtil Get-allimagegroups
description: Artículo de referencia del comando WDSUtil Get-allimagegroups, que recupera información sobre todos los grupos de imágenes de un servidor y todas las imágenes de esos grupos de imágenes.
ms.topic: reference
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7209b0f806b0fd86167e8fbcf36f717a0d069195
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614867"
---
# <a name="wdsutil-get-allimagegroups"></a>WDSUtil Get-allimagegroups

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información sobre todos los grupos de imágenes de un servidor y todas las imágenes de esos grupos de imágenes.

## <a name="syntax"></a>Sintaxis

```
wdsutil [options] /get-allimagegroups [/server:<servername>] [/detailed]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[/server:<servername>]` | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| [/detailed] | Devuelve los metadatos de la imagen de cada imagen. Si no se usa este parámetro, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo de cada imagen. |

## <a name="examples"></a>Ejemplos

Para ver información acerca de los grupos de imágenes, escriba:

```
wdsutil /get-allimagegroups
```

```
wdsutil /verbose /get-allimagegroups /server:MyWDSServer /detailed
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Add-imagegroup (comando)](wdsutil-add-imagegroup.md)

- [WDSUtil Remove-imagegroup (comando)](wdsutil-remove-imagegroup.md)

- [WDSUtil Set-imagegroup (comando)](wdsutil-set-imagegroup.md)
