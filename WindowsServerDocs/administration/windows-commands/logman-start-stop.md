---
title: logman start y logman stop
description: Artículo de referencia de los comandos Logman Start y Logman STOP, que inicia un recopilador de datos y establece el tiempo de inicio en manual, o detiene un conjunto de recopiladores de datos y establece la hora de finalización en manual.
ms.topic: reference
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9a684eb010d52e5aba01fee609f878cc5485a7d5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639961"
---
# <a name="logman-start-and-logman-stop"></a>logman start y logman stop

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

El comando **Logman Start** inicia un recopilador de datos y establece la hora de inicio en manual. El comando **Logman STOP** detiene un conjunto de recopiladores de datos y establece la hora de finalización en manual.

## <a name="syntax"></a>Sintaxis

```
logman start <[-n] <name>> [options]
logman stop <[-n] <name>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s `<computer name>` | Ejecute el comando en el equipo remoto especificado. |
| -config `<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| [-n] `<name>` | Especifica el nombre del objeto de destino. |
| -ETS | Envía comandos a sesiones de seguimiento de eventos directamente, sin guardar ni programar. |
| -as | Realiza la operación solicitada de forma asincrónica. |
| -? | Muestra la ayuda contextual. |

### <a name="examples"></a>Ejemplos

Para iniciar el *perf_log*del recopilador de datos, en el *server_1*del equipo remoto, escriba:

```
logman start perf_log -s server_1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Logman (comando)](logman.md)
