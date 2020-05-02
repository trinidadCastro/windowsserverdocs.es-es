---
title: mover
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1df37753239fea5d5ba9ba22256706a47d4c6a2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723911"
---
# <a name="move"></a>mover



Mueve uno o más archivos de un directorio a otro.



## <a name="syntax"></a>Sintaxis

```
move [{/y | /-y}] [<Source>] [<Target>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/y|Suprime el mensaje para confirmar que desea sobrescribir un archivo de destino existente.|
|/-y|Hace que se pida confirmación de que desea sobrescribir un archivo de destino existente.|
|\<> de origen|Especifica la ruta de acceso y el nombre del archivo o los archivos que se van a trasladar. Si desea trasladar o cambiar el nombre de un directorio, el *origen* debe ser la ruta de acceso y el nombre del directorio actual.|
|\<Target>|Especifica la ruta de acceso y el nombre al que se van a trasladar los archivos. Si desea trasladar o cambiar el nombre de un directorio, el *destino* debe ser la ruta de acceso y el nombre del directorio deseado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   La opción de línea de comandos **/y** podría estar preestablecida en la variable de entorno COPYCMD. Puede invalidarlo con **/-y** en la línea de comandos. El valor predeterminado es preguntar antes de sobrescribir los archivos a menos que el comando de **copia** se ejecute desde un script de batch.
-   Si se mueven archivos cifrados a un volumen que no admite Sistema de cifrado de archivos (EFS), se produce un error. Descifre primero los archivos o mueva los archivos a un volumen que admita EFS.

## <a name="examples"></a>Ejemplos

Para migrar todos los archivos con la extensión. xls del directorio \data al directorio \ Second_Q \Reports, escriba:
```
move \data\*.xls \second_q\reports\ 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)