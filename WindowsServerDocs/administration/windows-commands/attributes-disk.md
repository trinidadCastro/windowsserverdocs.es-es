---
title: disco de atributos
description: Comando de comandos de Windows para el **disco de atributos**, que muestra, establece o borra los atributos de un disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3c29a009a1efdfb7fed3d04d194cc8cfd2ea4eb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851248"
---
# <a name="attributes-disk"></a>disco de atributos

Muestra, establece o borra los atributos de un disco.

## <a name="syntax"></a>Sintaxis

```
attributes disk [{set | clear}] [readonly] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| set | Establece el atributo especificado del disco con el foco. |
| clear | Borra el atributo especificado del disco que tiene el foco. |
| readonly | Especifica que el disco es de solo lectura. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Comentarios

-   Cuando se utiliza el **disco de atributos** para mostrar los atributos actuales de un disco, el atributo disco de inicio denota el disco que se usa para iniciar el equipo. Para un reflejo dinámico, se muestra para el disco que contiene el Plex de arranque del volumen de arranque.

-   Se debe seleccionar un disco para que el comando de **disco de atributos** se ejecute correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver los atributos del disco seleccionado, escriba:

```
attributes disk
```

Para establecer el disco seleccionado como de solo lectura, escriba:

```
attributes disk set readonly
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)