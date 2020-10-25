---
title: Get-AllServers
description: Artículo de referencia de Get-AllServers, que recupera información sobre todos los servidores de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 450c864bef3b3f17f3912a06aa72aa56ce6e529a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524450"
---
# <a name="get-allservers"></a>Get-AllServers

Recupera información sobre todos los servidores de servicios de implementación de Windows.

> [!NOTE]
> Este comando puede tardar mucho tiempo en completarse si hay muchos servidores de servicios de implementación de Windows en su entorno o si la conexión de red que vincula los servidores es lenta.

## <a name="syntax"></a>Sintaxis

```
wdsutil [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
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
wdsutil /Get-AllServers /Show:Config
```
Para ver información detallada acerca de todos los servidores, escriba:
```
wdsutil /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)