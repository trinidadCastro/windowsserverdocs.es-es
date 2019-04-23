---
title: volumen en línea
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6bacba1e204f1eee2e3d4772ff9024aedbfc4fed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882006"
---
# <a name="online-volume"></a>volumen en línea



Pone los volúmenes que están actualmente sin conexión a un estado en línea

> [!IMPORTANT]
> Este comando no está disponible en cualquier edición de Windows Vista.

> [!IMPORTANT]
> Este comando generará un error si se usa en un volumen de solo lectura.

## <a name="syntax"></a>Sintaxis

```
online volume [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   Este comando funciona en los volúmenes que se han producido un error, se producen errores o se encuentran en estado de error de redundancia.
-   Debe seleccionarse un volumen para que este comando se ejecute correctamente. Use la **seleccione volumen** comando para seleccionar un volumen y desplace el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para que el volumen con el enfoque en línea, escriba:
```
online volume
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

