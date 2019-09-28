---
title: pause
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6501859eacf30dd6c1e64f34eee29ff81bd78ec9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372375"
---
# <a name="pause"></a>pause



Suspende el procesamiento de un programa por lotes y muestra el siguiente mensaje:
```
Press any key to continue . . .
```
Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
pause
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

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

## <a name="BKMK_examples"></a>Example

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

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)