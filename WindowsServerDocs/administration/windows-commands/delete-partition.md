---
title: eliminar partición
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46a214f26e7c21f6ae08eb16d95fd898bd949b0f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378656"
---
# <a name="delete-partition"></a>eliminar partición



Elimina la partición que tiene el foco.

## <a name="syntax"></a>Sintaxis

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|estima|Permite que DiskPart elimine cualquier partición independientemente del tipo. Normalmente, DiskPart solo permite eliminar particiones de datos conocidas.|
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

> [!CAUTION]
> La eliminación de una partición en un disco dinámico puede eliminar todos los volúmenes dinámicos del disco, con lo que se destruirán los datos y el disco quedará en un estado dañado. Para eliminar un volumen dinámico, use siempre el comando **Eliminar volumen** en su lugar. Las particiones se pueden eliminar de los discos dinámicos, pero no se deben crear. Por ejemplo, es posible eliminar una partición de tabla de particiones GUID (GPT) no reconocida en un disco GPT dinámico. La eliminación de este tipo de partición no hace que el espacio libre resultante esté disponible. Este comando está pensado para que se reclame espacio en un disco dinámico sin conexión dañado en una situación de emergencia en la que no se puede usar el comando **Clean** en Diskpart.
> -   No se puede eliminar la partición del sistema, la partición de arranque o cualquier partición que contenga el archivo de paginación activo o la información de volcado de memoria.
> -   Se debe seleccionar una partición para que esta operación se realice correctamente. Use el comando **seleccionar partición** para seleccionar una partición y desplazar el foco a ella.

## <a name="BKMK_examples"></a>Example

Para eliminar la partición que tiene el foco, escriba:
```
delete partition
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

