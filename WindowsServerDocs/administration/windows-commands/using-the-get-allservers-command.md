---
title: Mediante el comando get-AllServers
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbccb834f9058f2c3cca097cdf998455f2a6892e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440496"
---
# <a name="using-the-get-allservers-command"></a>Mediante el comando get-AllServers



Recupera información sobre todos los servidores de servicios de implementación de Windows.

> [!NOTE]
> Este comando puede tardar un período prolongado de tiempo en completarse si hay muchos servidores de servicios de implementación de Windows en su entorno o si la conexión de red los servidores de la vinculación es lenta.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                                 Descripción                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / Mostrar: {Config |                                                                                                                    Imágenes                                                                                                                    |
|  [/ Detallada]  | Cuando se usa junto con el **/Show:Images** o **/Show:All**, devuelve todos los metadatos de cada imagen de la imagen. Si el **/detallada** opción no se especifica, el comportamiento predeterminado es devolver el nombre de la imagen, descripción y nombre de archivo. |
| [/ Forest: {Sí |                                                                                                                     No}]                                                                                                                     |

## <a name="BKMK_examples"></a>Ejemplos

Para ver información acerca de todos los servidores, escriba:
```
WDSUTIL /Get-AllServers /Show:Config
```
Para ver información detallada sobre todos los servidores, escriba:
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)