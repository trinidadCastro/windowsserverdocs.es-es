---
title: ntcmdprompt
description: Artículo de referencia para el comando ntcmdprompt, que ejecuta el intérprete de comandos **Cmd.exe**, en lugar de **Command.com**, después de ejecutar un comando finalizar y permanecer residente (TSR) o después de iniciar el símbolo del sistema desde una aplicación de ms-dos.
ms.topic: reference
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 02aac71aea099359c1aa661086ed2702a89d276b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639160"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Ejecuta el intérprete de comandos **Cmd.exe**, en lugar de **Command.com**, después de ejecutar un comando finalizar y permanecer residente (TSR) o después de iniciar el símbolo del sistema desde una aplicación de ms-dos.

## <a name="syntax"></a>Sintaxis

```
ntcmdprompt
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Cuando **Command.com** se está ejecutando, algunas características de **Cmd.exe**, como la visualización de **doskey** del historial de comandos, no están disponibles. Si prefiere ejecutar el intérprete de comandos de **Cmd.exe** después de haber iniciado una terminación y permanecer residente (TSR) o de iniciar el símbolo del sistema desde una aplicación basada en MS-dos, puede usar el comando **ntcmdprompt** . Sin embargo, tenga en cuenta que es posible que el TSR no esté disponible para su uso cuando se ejecuta **Cmd.exe**. Puede incluir el comando **ntcmdprompt** en el archivo **config. NT** o el archivo de inicio personalizado equivalente en el archivo de información de programa (PIF) de una aplicación.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
