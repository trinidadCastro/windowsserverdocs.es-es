---
title: Get-AllServers
description: Artículo de referencia de Get-AllServers, que recupera información sobre todos los servidores de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b60fb7710699c4fff6656a0e2a34684a538b116d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626358"
---
# <a name="get-allservers"></a>Get-AllServers

Recupera información sobre todos los servidores de servicios de implementación de Windows.

> [!NOTE]
> Este comando puede tardar mucho tiempo en completarse si hay muchos servidores de servicios de implementación de Windows en su entorno o si la conexión de red que vincula los servidores es lenta.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

### <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                                 Descripción                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show: {config |                                                                                                                    Imágenes                                                                                                                    |
|  [/Detailed]  | Cuando se usa junto con **/Show: images** o **/Show: ALL**, devuelve todos los metadatos de imagen de cada imagen. Si no se especifica la opción **/Detailed** , el comportamiento predeterminado es devolver el nombre de la imagen, la descripción y el nombre de archivo. |
| [/Forest: {Yes |                                                                                                                     No}]                                                                                                                     |

## <a name="examples"></a>Ejemplos

Para ver información acerca de todos los servidores, escriba:
```
WDSUTIL /Get-AllServers /Show:Config
```
Para ver información detallada acerca de todos los servidores, escriba:
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)