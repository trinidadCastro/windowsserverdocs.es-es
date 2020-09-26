---
title: set id
description: Artículo de referencia para el comando de ID. de conjunto de DiskPart, que cambia el campo de tipo de partición para la partición que tiene el foco.
ms.topic: reference
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0f25498a89cdb7347a40825af70621d5cd09440a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389089"
---
# <a name="set-id-diskpart"></a>identificador de conjunto (DiskPart)

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el campo de tipo de partición para la partición que tiene el foco. Este comando no funciona en discos dinámicos ni en particiones reservadas de Microsoft.

> [!IMPORTANT]
> Este comando está pensado para ser utilizado únicamente por los fabricantes de equipos originales (OEM). Cambiar los campos de tipo de partición con este parámetro puede hacer que el equipo no funcione o no pueda arrancar. A menos que sea un OEM o que tenga experiencia en discos GPT, no debe cambiar los campos de tipo de partición en discos GPT mediante este parámetro. En su lugar, use siempre el comando [Create Partition EFI](create-partition-efi.md) para crear particiones del sistema EFI, el comando [Create Partition MSR](create-partition-msr.md) para crear particiones reservadas de Microsoft y el comando [Create Partition Primary](create-partition-primary.md) sin el parámetro id para crear particiones primarias en discos GPT.

## <a name="syntax"></a>Sintaxis

```
set id={ <byte> | <GUID> } [override] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<byte>` | En el caso de los discos de registro de arranque maestro (MBR), especifica el nuevo valor para el campo de tipo, en formato hexadecimal, para la partición. Cualquier **byte** de tipo de partición se puede especificar con este parámetro, excepto en el tipo 0x42, que especifica una partición LDM. Tenga en cuenta que el 0x inicial se omite al especificar el tipo de partición hexadecimal. |
| `<GUID>` | En el caso de los discos de tabla de particiones GUID (GPT), especifica el nuevo valor de GUID para el campo de tipo de la partición. Los GUID reconocidos incluyen:<ul><li>**Partición del sistema EFI:** c12a7328-f81f-11D2-ba4b-00a0c93ec93b</li><li>**Partición de datos básica:** ebd0a0a2-b9e5-4433-87c0-68b6b72699c7</li></ul>Cualquier GUID de tipo de partición se puede especificar con este parámetro, excepto los siguientes:<ul><li>**Partición reservada de Microsoft:** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**Partición de metadatos LDM en un disco dinámico:** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**Partición de datos LDM en un disco dinámico:** af9b60a0-1431-4f62-bc68-3311714a69ad</li><li>**Partición de metadatos del clúster:** db97dba9-0840-4bae-97f0-ffb9a327c7e1</li></ul> |
| override | fuerza el desmontaje del sistema de archivos en el volumen antes de cambiar el tipo de partición. Al ejecutar el comando **set ID** , DiskPart intenta bloquear y desmontar el sistema de archivos en el volumen. Si no se especifica **override** y se produce un error en la llamada para bloquear el sistema de archivos (por ejemplo, porque hay un identificador abierto), se produce un error en la operación. Si se especifica **override** , DiskPart fuerza el desmontaje incluso si se produce un error en la llamada para bloquear el sistema de archivos, y los identificadores abiertos del volumen dejarán de ser válidos. |
| noerr | Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Observaciones

- Aparte de las limitaciones mencionadas anteriormente, DiskPart no comprueba la validez del valor que especifique (excepto para asegurarse de que es un byte en formato hexadecimal o GUID).

## <a name="examples"></a>Ejemplos

Para establecer el campo Type en *0x07* y forzar el desmontaje del sistema de archivos, escriba:

```
set id=0x07 override
```

Para establecer que el campo de tipo sea una partición de datos básica, escriba:

```
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
