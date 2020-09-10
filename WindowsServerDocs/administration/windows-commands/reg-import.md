---
title: reg import
description: Artículo de referencia para el comando reg Import, que copia el contenido de un archivo que contiene subclaves del registro, entradas y valores exportados en el registro del equipo local.
ms.topic: reference
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 49f83a20b4f9c9e1e64f6e33f0980757de60d9ec
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627037"
---
# <a name="reg-import"></a>reg import

Copia el contenido de un archivo que contiene las subclaves del registro exportadas, las entradas y los valores en el registro del equipo local.

## <a name="syntax"></a>Sintaxis

```
reg import <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<filename>` | Especifica el nombre y la ruta de acceso del archivo que tiene el contenido que se va a copiar en el registro del equipo local. Este archivo debe crearse de antemano mediante **reg Export**. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los valores devueltos para la operación de **importación de reg** son:

    | Value | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para importar las entradas del registro desde el archivo llamado CopiaAp. reg, escriba:

```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [REG Export (comando)](reg-export.md)
