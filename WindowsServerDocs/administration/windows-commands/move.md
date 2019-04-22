---
title: move
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d651e586c31ff64664079bdd10ffde3701ec317d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824996"
---
# <a name="move"></a>move



Mueve uno o varios archivos de un directorio en otro directorio.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
move [{/y | /-y}] [<Source>] [<Target>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/y|Suprime el mensaje para confirmar que desea sobrescribir un archivo de destino existente.|
|/-y|Hace que se pida confirmación para confirmar que desea sobrescribir un archivo de destino existente.|
|\<origen >|Especifica la ruta de acceso y el nombre del archivo o archivos que desea mover. Si desea mover o cambiar el nombre de un directorio, *origen* debe ser el nombre y ruta del directorio actual.|
|\<Destino >|Especifica la ruta de acceso y nombre para mover archivos a. Si desea mover o cambiar el nombre de un directorio, *destino* debe ser el nombre y ruta de acceso del directorio que desee.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **/y** opción de línea de comandos podría estar preestablecido en apunta el vínculo. Puede invalidar esto con **/y** en la línea de comandos. El valor predeterminado es preguntar antes de sobrescribir los archivos a menos que el **copia** comando se ejecuta desde dentro de un script por lotes.
-   Mover archivos cifrados a un volumen que no es compatible con los resultados del sistema de archivos cifrados (EFS) en un error. Descifrar en primer lugar los archivos o mueva los archivos a un volumen que es compatible con EFS.

## <a name="BKMK_examples"></a>Ejemplos

Para mover todos los archivos con la extensión .xls desde el directorio \Data directorio \Second_Q\Reports, escriba:
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)