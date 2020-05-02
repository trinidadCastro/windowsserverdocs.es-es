---
title: Opción SET
description: Tema de referencia de la opción set, que establece las opciones para la creación de instantáneas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4aba049e29cd74450467cf28057a2ff4e4a7094
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721901"
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