---
title: opción set
description: Artículo de referencia para el comando set option, que establece las opciones para la creación de instantáneas.
ms.topic: reference
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a37e993700169f0268e68822be3a7fbaf6e58361
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389061"
---
# <a name="set-option"></a>opción set

Establece las opciones para crear la instantánea. Si se usa sin parámetros, la **opción set** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| copia | Especifica que se debe crear una instantánea de un momento dado de los volúmenes especificados. |
| complejos | Especifica que se cree una copia de clon de un momento dado de los datos en un volumen especificado. |
| transportables | Especifica que la instantánea no se va a importar todavía. El archivo Metadata. cab se puede usar más adelante para importar la instantánea en el mismo equipo o en otro diferente. |
| [rollbackrecover] | Indica a los escritores que utilicen *Autorrecuperación* durante el evento **postsnapshot** . Esto resulta útil si se va a utilizar la instantánea para la reversión (por ejemplo, con minería de datos). |
| [txfrecover] | Solicita a VSS que haga que la instantánea sea transaccionalmente coherente durante la creación. |
| [noautorecover] | Detiene los escritores y el sistema de archivos para realizar cambios de recuperación en la instantánea a un estado coherente con las transacciones. **Noautorecover** no se puede usar con **txfrecover** o **rollbackrecover**. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [establecer contexto (comando)](set-context.md)

- [comando establecer metadatos](set-metadata.md)

- [Set verbose (comando)](set-verbose.md)
