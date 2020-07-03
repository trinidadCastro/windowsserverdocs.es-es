---
title: Opción SET
description: Artículo de referencia de la opción set, que establece las opciones para la creación de instantáneas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7944cd51adffabdc85bc6d419b527b1b643d4a8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937086"
---
# <a name="set-option"></a>Opción SET

Establece las opciones para crear la instantánea. Si se usa sin parámetros, la **opción set** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                                  Descripción                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [diferencial   |                                                                                                     complejos                                                                                                     |
|  transportables  |                       Especifica que la instantánea no se va a importar todavía. El archivo Metadata. cab se puede usar más adelante para importar la instantánea en el mismo equipo o en otro diferente.                       |
| [rollbackrecover] |                     Indica a los escritores que utilicen *Autorrecuperación* durante el evento **postsnapshot** . Esto resulta útil si se va a utilizar la instantánea para la reversión (por ejemplo, con minería de datos).                      |
|   [txfrecover]    |                                                               Solicita a VSS que haga que la instantánea sea transaccionalmente coherente durante la creación.                                                                |
|  [noautorecover]  | Detiene los escritores y el sistema de archivos para realizar cambios de recuperación en la instantánea a un estado coherente con las transacciones. **Noautorecover** no se puede usar con **txfrecover** o **rollbackrecover**. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)