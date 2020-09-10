---
title: reg export
description: Artículo de referencia para el comando reg Export, que copia las subclaves, entradas y valores especificados del equipo local en un archivo para su transferencia a otros servidores.
ms.topic: reference
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a7bf8abe5dd97463202a024da90523a52020a986
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627050"
---
# <a name="reg-export"></a>reg export

Copia las subclaves, entradas y valores especificados del equipo local en un archivo para su transferencia a otros servidores.

## <a name="syntax"></a>Sintaxis

```
reg export <keyname> <filename> [/y]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname>` | Especifica la ruta de acceso completa de la subclave. La operación de exportación solo funciona con el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| `<filename>` | Especifica el nombre y la ruta de acceso del archivo que se va a crear durante la operación. El archivo debe tener una extensión. reg. |
| /y | Sobrescribe cualquier archivo existente con el nombre *filename* sin pedir confirmación. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los valores devueltos para la operación de **exportación del registro** son:

    | Value | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para exportar el contenido de todas las subclaves y valores de la clave MyApp al archivo CopiaAp. reg, escriba:

```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
