---
title: mover
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efd0cd0716c564a9570647820056ab9c38e41274
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839378"
---
# <a name="move"></a>mover



Mueve uno o más archivos de un directorio a otro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
move [{/y | /-y}] [<Source>] [<Target>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/y|Suprime el mensaje para confirmar que desea sobrescribir un archivo de destino existente.|
|/-y|Hace que se pida confirmación de que desea sobrescribir un archivo de destino existente.|
|> de \<de origen|Especifica la ruta de acceso y el nombre del archivo o los archivos que se van a trasladar. Si desea trasladar o cambiar el nombre de un directorio, el *origen* debe ser la ruta de acceso y el nombre del directorio actual.|
|> de destino de \<|Especifica la ruta de acceso y el nombre al que se van a trasladar los archivos. Si desea trasladar o cambiar el nombre de un directorio, el *destino* debe ser la ruta de acceso y el nombre del directorio deseado.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   La opción de línea de comandos **/y** podría estar preestablecida en la variable de entorno COPYCMD. Puede invalidarlo con **/-y** en la línea de comandos. El valor predeterminado es preguntar antes de sobrescribir los archivos a menos que el comando de **copia** se ejecute desde un script de batch.
-   Si se mueven archivos cifrados a un volumen que no admite Sistema de cifrado de archivos (EFS), se produce un error. Descifre primero los archivos o mueva los archivos a un volumen que admita EFS.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para migrar todos los archivos con la extensión. xls del directorio \data al directorio \ Second_Q \Reports, escriba:
```
move \data\*.xls \second_q\reports\ 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)