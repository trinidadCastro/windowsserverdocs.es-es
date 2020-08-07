---
title: reg copy
description: Artículo de referencia para el comando reg Copy, que copia una entrada del registro en una ubicación especificada en el equipo local o remoto.
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc1141ddd8082ee6302886a5ce49b9805a19cede
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884182"
---
# <a name="reg-copy"></a>reg copy

Copia una entrada del registro en una ubicación especificada en el equipo local o remoto.

## <a name="syntax"></a>Sintaxis

```
reg copy <keyname1> <keyname2> [/s] [/f]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname1>` | Especifica la ruta de acceso completa de la subclave o entrada que se va a agregar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| `<keyname2>` | Especifica la ruta de acceso completa de la segunda subclave que se va a comparar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| /s | Copia todas las subclaves y entradas de la subclave especificada. |
| /f | Copia la subclave sin pedir confirmación. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Este comando no pide confirmación al copiar una subclave.

- Los valores devueltos para la operación de **comparación de reg** son:

    | Valor | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para copiar todas las subclaves y valores de la clave MyApp en la clave SaveMyApp, escriba:

```
reg copy HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```

Para copiar todos los valores de la clave MyCo del equipo denominado zodíaco en la clave MyCo1 del equipo actual, escriba:

```
reg copy \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
