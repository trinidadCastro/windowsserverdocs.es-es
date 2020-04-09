---
title: Get-AllServers
description: Temas de comandos de Windows para Get-AllServers, que recupera información sobre todos los servidores de servicios de implementación de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b400d5a2be69e8e89a05b233cc2e8f29bec848f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831218"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

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