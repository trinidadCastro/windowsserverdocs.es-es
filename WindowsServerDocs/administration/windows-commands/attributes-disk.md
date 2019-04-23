---
title: Disco de atributos
description: Tema de los comandos de Windows para **disco atributos** -muestra, Establece o borra los atributos de un disco.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cacc2fb6b47d095f5e452ca470c89f228949594
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890356"
---
# <a name="attributes-disk"></a>Disco de atributos



Muestra, Establece o borra los atributos de un disco.

> [!IMPORTANT]
> Este parámetro no está disponible en ninguna edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
attributes disk [{set | clear}] [readonly] [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|set|Establece el atributo especificado del disco con el foco.|
|clear|Borra el atributo especificado del disco con el foco.|
|ReadOnly|Especifica que el disco es de solo lectura.|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   Cuando **disco atributos** es usa para mostrar los atributos actuales de un disco, el atributo de disco de inicio que denota el disco que se usa para iniciar el equipo. Para un reflejo dinámico, se muestra para el disco que contiene el complejo de arranque del volumen de arranque.
-   Debe seleccionarse un disco para el **disco atributos** comando se ejecute correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para ver los atributos del disco seleccionado, escriba:
```
attributes disk
```
Para establecer el disco seleccionado como de solo lectura, escriba:
```
attributes disk set readonly
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

