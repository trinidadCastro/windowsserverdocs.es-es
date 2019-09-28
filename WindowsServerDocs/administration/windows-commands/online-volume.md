---
title: volumen en línea
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 06a3c81313180b2880c1e47c3b6c12236fda4245
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372522"
---
# <a name="online-volume"></a>volumen en línea



Traslada los volúmenes que están actualmente sin conexión a un estado en línea

> [!IMPORTANT]
> Este comando no está disponible en ninguna edición de Windows Vista.

> [!IMPORTANT]
> Este comando producirá un error si se usa en un volumen de solo lectura.

## <a name="syntax"></a>Sintaxis

```
online volume [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Este comando funciona en los volúmenes en los que se ha producido un error, están produciendo errores o tienen un estado de redundancia con errores.
-   Se debe seleccionar un volumen para que este comando se ejecute correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

## <a name="BKMK_examples"></a>Example

Para poner en línea el volumen que tiene el foco, escriba:
```
online volume
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

