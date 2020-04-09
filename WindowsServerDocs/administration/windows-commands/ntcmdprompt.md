---
title: ntcmdprompt
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25afab00ced3cb14771c18aa38c7fd8c98aecc0c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838048"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ejecuta el intérprete de comandos **cmd. exe**, en lugar de **Command.com**, después de ejecutar un comando finalizar y permanecer residente (TSR) o después de iniciar el símbolo del sistema desde una aplicación de ms-dos.
## <a name="syntax"></a>Sintaxis
```
ntcmdprompt
```
#### <a name="parameters"></a>Parámetros

| Parámetro |             Descripción              |
|-----------|--------------------------------------|
|    /?     | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios
- Cuando **Command.com** se está ejecutando, algunas características de **cmd. exe**, como la visualización de **doskey** del historial de comandos, no están disponibles. Si prefiere ejecutar el intérprete de comandos **cmd. exe** después de haber iniciado una terminación y permanecer residente (TSR) o de iniciar el símbolo del sistema desde una aplicación basada en MS-dos, puede usar el comando **ntcmdprompt** . Sin embargo, tenga en cuenta que es posible que el TSR no esté disponible para su uso cuando ejecute **cmd. exe**. Puede incluir el comando **ntcmdprompt** en el archivo **config. NT** o el archivo de inicio personalizado equivalente en el archivo de información de programa (PIF) de una aplicación.
  ## <a name="examples"></a>Ejemplos
  Para incluir **ntcmdprompt** en el archivo **config. NT** o el archivo de inicio de configuración especificado en el PIF, escriba: **ntcmdprompt**
  ## <a name="additional-references"></a>Referencias adicionales
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

