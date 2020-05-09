---
title: delete partition
description: Tema de referencia del comando DELETE Partition, que elimina la partición que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13c79b826480171af578334942af8f73b77d796b
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993123"
---
# <a name="delete-partition"></a>delete partition

Elimina la partición que tiene el foco. Antes de comenzar, debe seleccionar una partición para que esta operación se realice correctamente. Use el comando [seleccionar partición](select-partition.md) para seleccionar una partición y desplazar el foco a ella.

> [!WARNING]
> La eliminación de una partición en un disco dinámico puede eliminar todos los volúmenes dinámicos del disco, destruyendo los datos y saliendo del disco en un estado dañado.
>
> No se puede eliminar la partición del sistema, la partición de arranque o cualquier partición que contenga el archivo de paginación activo o la información de volcado de memoria.

## <a name="syntax"></a>Sintaxis

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |
| override | Permite que DiskPart elimine una partición con independencia de su tipo. Normalmente, DiskPart solo permite eliminar particiones de datos conocidas. |

#### <a name="remarks"></a>Comentarios

- Para eliminar un volumen dinámico, use siempre el comando [Eliminar volumen](delete-volume.md) en su lugar.

- Las particiones se pueden eliminar de los discos dinámicos, pero no se deben crear. Por ejemplo, es posible eliminar una partición de tabla de particiones GUID (GPT) no reconocida en un disco GPT dinámico. La eliminación de este tipo de partición no hace que el espacio libre resultante esté disponible. En su lugar, este comando está diseñado para permitirle recuperar espacio en un disco dinámico sin conexión dañado en una situación de emergencia en la que no se puede usar el comando [Clean](clean.md) en Diskpart.

## <a name="examples"></a>Ejemplos

Para eliminar la partición que tiene el foco, escriba:

```
delete partition
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [select partition](select-partition.md)

- [eliminar comando](delete.md)

- [comando Eliminar volumen](delete-volume.md)

- [Clean (comando)](clean.md)
