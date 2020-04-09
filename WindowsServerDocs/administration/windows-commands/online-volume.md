---
title: volumen en línea
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 476dd893e7899a2bd58336546a7881934f415f92
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837848"
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

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Este comando funciona en los volúmenes en los que se ha producido un error, están produciendo errores o tienen un estado de redundancia con errores.
-   Se debe seleccionar un volumen para que este comando se ejecute correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para poner en línea el volumen que tiene el foco, escriba:
```
online volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

