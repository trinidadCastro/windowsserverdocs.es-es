---
title: Opción set
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9b9174f219654e99eb9441abe3342c31b5089ef5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384050"
---
# <a name="set-option"></a>Opción set



Establece las opciones para crear la instantánea. Si se usa sin parámetros, la **opción set** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                                  Descripción                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [diferencial   |                                                                                                     complejos                                                                                                     |
|  transportables  |                       Especifica que la instantánea no se va a importar todavía. El archivo Metadata. cab se puede usar más adelante para importar la instantánea en el mismo equipo o en otro diferente.                       |
| [rollbackrecover] |                     Indica a los escritores que utilicen *Autorrecuperación* durante el evento **postsnapshot** . Esto resulta útil si se va a utilizar la instantánea para la reversión (por ejemplo, con minería de datos).                      |
|   [txfrecover]    |                                                               Solicita a VSS que haga que la instantánea sea transaccionalmente coherente durante la creación.                                                                |
|  [noautorecover]  | Detiene los escritores y el sistema de archivos para realizar cambios de recuperación en la instantánea a un estado coherente con las transacciones. **Noautorecover** no se puede usar con **txfrecover** o **rollbackrecover**. |

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)