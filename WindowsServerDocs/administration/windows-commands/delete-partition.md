---
title: delete partition
description: Tema de referencia de DELETE Partition, que elimina la partición que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 259abdfc6e3ba8db22d5582bff08d7a4bc8b807b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716728"
---
# <a name="delete-partition"></a>delete partition

Elimina la partición que tiene el foco.

## <a name="syntax"></a>Sintaxis

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|override|Permite que DiskPart elimine una partición con independencia de su tipo. Normalmente, DiskPart solo permite eliminar particiones de datos conocidas.|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Observaciones

> [!CAUTION]
> La eliminación de una partición en un disco dinámico puede eliminar todos los volúmenes dinámicos del disco, con lo que se destruirán los datos y el disco quedará en un estado dañado. Para eliminar un volumen dinámico, use siempre el comando **Eliminar volumen** en su lugar. Las particiones se pueden eliminar de los discos dinámicos, pero no se deben crear. Por ejemplo, es posible eliminar una partición de tabla de particiones GUID (GPT) no reconocida en un disco GPT dinámico. La eliminación de este tipo de partición no hace que el espacio libre resultante esté disponible. Este comando está pensado para que se reclame espacio en un disco dinámico sin conexión dañado en una situación de emergencia en la que no se puede usar el comando **Clean** en Diskpart.
> -   No se puede eliminar la partición del sistema, la partición de arranque o cualquier partición que contenga el archivo de paginación activo o la información de volcado de memoria.
> -   Se debe seleccionar una partición para que esta operación se realice correctamente. Use el comando **seleccionar partición** para seleccionar una partición y desplazar el foco a ella.

## <a name="examples"></a>Ejemplos

Para eliminar la partición que tiene el foco, escriba:
```
delete partition
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

