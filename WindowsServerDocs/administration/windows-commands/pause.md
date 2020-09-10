---
title: pause
description: Artículo de referencia del comando PAUSE, que suspende el procesamiento de programas por lotes.
ms.topic: reference
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4611980b0f805843b1f93c20450163f01e362cf8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640654"
---
# <a name="pause"></a>pause

Suspende el procesamiento de un programa por lotes, mostrando el símbolo del sistema. `Press any key to continue . . .`

## <a name="syntax"></a>Sintaxis

```
pause
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Observaciones

- Si presiona CTRL + C para detener un programa por lotes, aparece el mensaje siguiente: `Terminate batch job (Y/N)?` . Si presiona **y** (para sí) en respuesta a este mensaje, el programa por lotes finaliza y el control vuelve al sistema operativo.

- Puede insertar el comando **pausar** antes de una sección del archivo por lotes que no desee procesar. Al **pausar** suspende el procesamiento del programa por lotes, puede presionar Ctrl + C y, a continuación, presionar **y** para detener el programa por lotes.

## <a name="examples"></a>Ejemplos

Para crear un programa por lotes que pida al usuario que cambie los discos en una de las unidades, escriba:

```
@echo off
:Begin
copy a:*.*
echo Put a new disk into Drive A
pause
goto begin
```

En este ejemplo, todos los archivos del disco de la unidad A se copian en el directorio actual. Después de que el mensaje le solicite colocar un nuevo disco en la unidad A, el comando **pausar** suspende el procesamiento para que pueda cambiar los discos y, a continuación, presionar cualquier tecla para reanudar el procesamiento. Este programa por lotes se ejecuta en un bucle sin fin; el comando **goto Begin** envía el intérprete de comandos a la etiqueta inicial del archivo por lotes.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)