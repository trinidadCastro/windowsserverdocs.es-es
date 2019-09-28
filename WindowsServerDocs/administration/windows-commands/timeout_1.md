---
title: timeout
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 09f294eb78a8868b4e3962557a36199b69fae0c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385775"
---
# <a name="timeout"></a>timeout



Pausa el procesador de comandos durante el número de segundos especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/t \<TimeoutInSeconds >|Especifica el número decimal de segundos (entre-1 y 99999) que hay que esperar antes de que el procesador de comandos continúe el procesamiento. El valor-1 hace que el equipo espere indefinidamente a una pulsación de tecla.|
|/nobreak|Especifica que se omitirán los trazos de clave de usuario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El comando **timeout** se usa normalmente en archivos por lotes.
-   La pulsación de teclas de un usuario reanuda la ejecución del procesador de comandos inmediatamente, incluso si no ha expirado el período de tiempo de espera.
-   Cuando se usa junto con el comando **Sleep** , el **tiempo de espera** es similar al comando **pausar** .

## <a name="BKMK_examples"></a>Example

Para pausar el procesador de comandos durante diez segundos, escriba:
```
timeout /t 10
```
Para pausar el procesador de comandos durante 100 segundos y omitir cualquier pulsación de tecla, escriba:
```
timeout /t 100 /nobreak
```
Para pausar el procesador de comandos indefinidamente hasta que se presione una tecla, escriba:
```
timeout /t -1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
