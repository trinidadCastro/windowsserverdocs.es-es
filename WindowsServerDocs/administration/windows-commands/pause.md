---
title: pause
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e5805fcc14d6874d95ba90537d72b560229ba99b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436314"
---
# <a name="pause"></a>pause



Suspende la ejecución de un programa por lotes y muestra el mensaje siguiente:
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

- Al ejecutar el **pausar** comando, aparece el mensaje siguiente:  
  ```
  Press any key to continue . . .
  ```  
- Si presiona CTRL+C para detener un programa por lotes, aparece el mensaje siguiente:  
  ```
  Terminate batch job (Y/N)?
  ```  
  Si presiona S (Sí) en respuesta a este mensaje, el programa por lotes finaliza y el control se devuelve al sistema operativo.
- Puede insertar el **pausar** comando antes de una sección del archivo por lotes que no desee procesar. Cuando **pausar** suspende el procesamiento de un programa por lotes, puede presionar CTRL + C y, a continuación, presione s para detener el programa por lotes.

## <a name="BKMK_examples"></a>Ejemplos

Para crear un programa por lotes que pide al usuario que cambie los discos de una de las unidades, escriba:
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
En este ejemplo, todos los archivos en el disco en la unidad A se copian en el directorio actual. Después de que el mensaje le pregunta si desea insertar un nuevo disco en la unidad A, el **pausar** comando suspende el procesamiento para que pueda cambiar los discos y, a continuación, presione cualquier tecla para reanudar el procesamiento. Este programa se ejecutará en un bucle infinito, el **comenzar goto** comando envía el intérprete de comandos a la etiqueta de inicio del archivo por lotes. Para detener este programa por lotes, presione CTRL+C y, a continuación, presione S.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)