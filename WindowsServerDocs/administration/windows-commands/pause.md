---
title: pause
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89e76c4f45f59c32ef879fb518a1a92c973f5cdf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723353"
---
# <a name="pause"></a>pause



Suspende el procesamiento de un programa por lotes y muestra el siguiente mensaje:
```
Press any key to continue . . .
```


## <a name="syntax"></a>Sintaxis

```
pause
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

- Al ejecutar el comando **PAUSE** , aparece el siguiente mensaje:  
  ```
  Press any key to continue . . .
  ```  
- Si presiona CTRL + C para detener un programa por lotes, aparece el siguiente mensaje:  
  ```
  Terminate batch job (Y/N)?
  ```  
  Si presiona Y (para sí) en respuesta a este mensaje, el programa por lotes finaliza y el control vuelve al sistema operativo.
- Puede insertar el comando **pausar** antes de una sección del archivo por lotes que no desee procesar. Al **pausar** suspende el procesamiento del programa por lotes, puede presionar Ctrl + C y, a continuación, presionar y para detener el programa por lotes.

## <a name="examples"></a>Ejemplos

Para crear un programa por lotes que pida al usuario que cambie los discos en una de las unidades, escriba:
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
En este ejemplo, todos los archivos del disco de la unidad A se copian en el directorio actual. Después de que el mensaje le solicite colocar un nuevo disco en la unidad A, el comando **pausar** suspende el procesamiento para que pueda cambiar los discos y, a continuación, presionar cualquier tecla para reanudar el procesamiento. Este programa por lotes se ejecuta en un bucle sin fin; el comando **goto Begin** envía el intérprete de comandos a la etiqueta inicial del archivo por lotes. Para detener este programa por lotes, presione CTRL + C y, a continuación, presione Y.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)