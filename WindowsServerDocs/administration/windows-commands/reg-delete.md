---
title: reg delete
description: Artículo de referencia para el comando reg Delete, que elimina una subclave o entradas del registro.
ms.topic: reference
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e7c132be937fab973bee0f5bb81bf6377128e483
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627044"
---
# <a name="reg-delete"></a>reg delete

Elimina una subclave o entradas del registro.

## <a name="syntax"></a>Sintaxis

```
reg delete <keyname> [{/v Valuename | /ve | /va}] [/f]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname1>` | Especifica la ruta de acceso completa de la subclave o entrada que se va a agregar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| /v `<Valuename>` | Elimina una entrada específica bajo la subclave. Si no se especifica ninguna entrada, se eliminarán todas las entradas y subclaves de la subclave. |
| /ve | Especifica que solo se eliminarán las entradas que no tengan ningún valor. |
| /va | Elimina todas las entradas de la subclave especificada. Las subclaves de la subclave especificada no se eliminan. |
| /f | Elimina la subclave del registro existente o la entrada sin solicitar confirmación. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los valores devueltos para la operación de **eliminación de registro** son:

    | Value | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para eliminar el tiempo de espera de la clave del registro y sus subclaves y valores, escriba:

```
reg delete HKLM\Software\MyCo\MyApp\Timeout
```

Para eliminar la MTU del valor del registro en HKLM\Software\MyCo en el equipo denominado zodíaco, escriba:

```
reg delete \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
