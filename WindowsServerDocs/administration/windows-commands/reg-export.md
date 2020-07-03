---
title: reg export
description: Artículo de referencia para el comando reg Export, que copia las subclaves, entradas y valores especificados del equipo local en un archivo para su transferencia a otros servidores.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c0cad839569651823e1c1a2bcca3c17c5550c8a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934639"
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

#### <a name="remarks"></a>Comentarios

- Los valores devueltos para la operación de **exportación del registro** son:

    | Valor | Descripción |
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
