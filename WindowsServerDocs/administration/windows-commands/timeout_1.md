---
title: timeout
description: Tema de referencia del tiempo de espera, que detiene el procesador de comandos durante el número de segundos especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed66342c4f0bbe22e9d2dc6440d291941c769cd7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721348"
---
# <a name="timeout"></a>timeout

Pausa el procesador de comandos durante el número de segundos especificado.



## <a name="syntax"></a>Sintaxis

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/t \<TimeoutInSeconds>|Especifica el número decimal de segundos (entre-1 y 99999) que hay que esperar antes de que el procesador de comandos continúe el procesamiento. El valor-1 hace que el equipo espere indefinidamente a una pulsación de tecla.|
|/nobreak|Especifica que se omitirán los trazos de clave de usuario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   El comando **timeout** se usa normalmente en archivos por lotes.
-   La pulsación de teclas de un usuario reanuda la ejecución del procesador de comandos inmediatamente, incluso si no ha expirado el período de tiempo de espera.
-   Cuando se usa junto con el comando **Sleep** , el **tiempo de espera** es similar al comando **pausar** .

## <a name="examples"></a>Ejemplos

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

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
