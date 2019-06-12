---
title: Opción SET
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4756627d19d296d02fa11ac67ef80080ddf318
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441360"
---
# <a name="set-option"></a>Opción SET



Establece las opciones para la creación de instantáneas. Si se utiliza sin parámetros, **establecer opción** muestra la Ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                                  Descripción                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [diferencial   |                                                                                                     plex]                                                                                                     |
|  [transportable]  |                       Especifica que la instantánea no se importarán todavía. El archivo .cab de metadatos más adelante puede usarse para importar la instantánea a la misma o en un equipo diferente.                       |
| [rollbackrecover] |                     Indica los autores de usar *Autorrecuperación* durante el **PostSnapshot** eventos. Esto es útil si la instantánea se usará para la reversión (por ejemplo, con la minería de datos).                      |
|   [txfrecover]    |                                                               Solicitudes de VSS para realizar la instantánea transaccionalmente coherente durante la creación.                                                                |
|  [noautorecover]  | Los escritores se detiene y el sistema de archivos, realice los cambios de recuperación en la instantánea a un estado transaccionalmente coherente. **Noautorecover** no se puede usar con **txfrecover** o **rollbackrecover**. |

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)