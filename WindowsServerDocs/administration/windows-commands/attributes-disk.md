---
title: disco de atributos
description: 'Temas de comandos de Windows para el **disco de atributos** : muestra, establece o borra los atributos de un disco.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 415125208b13d82adeed736107f59fda9489a953
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382571"
---
# <a name="attributes-disk"></a>disco de atributos



Muestra, establece o borra los atributos de un disco.

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
|clear|Borra el atributo especificado del disco que tiene el foco.|
|ReadOnly|Especifica que el disco es de solo lectura.|
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Cuando se utiliza el **disco de atributos** para mostrar los atributos actuales de un disco, el atributo disco de inicio denota el disco que se usa para iniciar el equipo. Para un reflejo dinámico, se muestra para el disco que contiene el Plex de arranque del volumen de arranque.
-   Se debe seleccionar un disco para que el comando de **disco de atributos** se ejecute correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="BKMK_examples"></a>Example

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

