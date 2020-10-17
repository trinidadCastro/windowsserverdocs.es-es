---
title: timeout
description: Artículo de referencia del comando timeout, que detiene el procesador de comandos durante el número de segundos especificado.
ms.topic: reference
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: edb4d14614099b0e501df175e619285928344c78
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156758"
---
# <a name="timeout"></a>timeout

Pausa el procesador de comandos durante el número de segundos especificado. Este comando se usa normalmente en archivos por lotes.

## <a name="syntax"></a>Sintaxis

```
timeout /t <timeoutinseconds> [/nobreak]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /t `<timeoutinseconds>` | Especifica el número decimal de segundos (entre-1 y 99999) que hay que esperar antes de que el procesador de comandos continúe el procesamiento. El valor **-1** hace que el equipo espere indefinidamente a una pulsación de tecla. |
| /nobreak | Especifica que se omitirán los trazos de clave de usuario. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- La pulsación de teclas de un usuario reanuda la ejecución del procesador de comandos inmediatamente, incluso si no ha expirado el período de tiempo de espera.

- Cuando se usa junto con la herramienta de **suspensión** del kit de recursos, el **tiempo de espera** es similar al comando **pausar** .

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
