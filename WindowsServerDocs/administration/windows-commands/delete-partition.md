---
title: Eliminar partición
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3b47338b74cf71a4754b7320d6b3842f342d324d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436144"
---
# <a name="delete-partition"></a>Eliminar partición



Elimina la partición tiene el foco.

## <a name="syntax"></a>Sintaxis

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|invalidar|Permite que DiskPart elimine una partición, independientemente del tipo. Normalmente, DiskPart sólo permite eliminar particiones de datos conocidas.|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

> [!CAUTION]
> Al eliminar una partición en un disco dinámico, pueden eliminar todos los volúmenes dinámicos del disco, por lo tanto, se destruirán los datos y dejar el disco en un estado dañado. Para eliminar un volumen dinámico, utilice siempre el **Eliminar volumen** comando en su lugar. Se pueden eliminar las particiones de los discos dinámicos, pero no se debe crear. Por ejemplo, es posible eliminar una partición de tabla de particiones GUID (GPT) no reconocida en un disco GPT dinámico. Eliminar dicha partición no hace que el espacio libre resultante esté disponible. Este comando está diseñado para permitirle que espacio reclame en un disco dinámico sin conexión dañado en una situación de emergencia donde el **limpia** no se puede usar comandos de DiskPart.
> -   No se puede eliminar la partición del sistema, partición de arranque o cualquier partición que contenga la active bloqueo o archivo de volcado de memoria información de paginación.
> -   Debe seleccionarse una partición para que esta operación se realice correctamente. Use la **seleccione partición** comando para seleccionar una partición y desplace el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para eliminar la partición tiene el foco, escriba:
```
delete partition
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

