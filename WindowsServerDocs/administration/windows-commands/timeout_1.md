---
title: timeout
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3997399b732c494050797c83a0a52938574986bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830176"
---
# <a name="timeout"></a>timeout



Detiene el procesador de comandos durante el número de segundos especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/t \<TimeoutInSeconds>|Especifica el número decimal de segundos (entre -1 y 99999) que se debe esperar antes de que el procesador de comandos continúa el procesamiento. El valor -1 hace que el equipo que espere indefinidamente a una pulsación de tecla.|
|/nobreak|Especifica que debe omitirse pulsaciones de tecla de usuario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **tiempo de espera** comando se utiliza generalmente en archivos por lotes.
-   Una pulsación de tecla de usuario reanuda la ejecución de procesador de comandos inmediatamente, incluso si no ha expirado el período de tiempo de espera.
-   Cuando se usa junto con el **suspensión** comando, **tiempo de espera** es similar a la **pausar** comando.

## <a name="BKMK_examples"></a>Ejemplos

Para pausar el procesador de comandos durante diez segundos, escriba:
```
timeout /t 10
```
Para pausar el procesador de comandos durante 100 segundos y pasar por alto cualquier pulsación de tecla, escriba:
```
timeout /t 100 /nobreak
```
Para pausar el procesador de comandos indefinidamente hasta que se presiona una tecla, escriba:
```
timeout /t -1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
