---
title: ntcmdprompt
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 583f56c294e66542a75efca09e97d57ae54a8cea
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436425"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se ejecuta el intérprete de comandos **Cmd.exe**, en lugar de **Command.com**, después de ejecutar una finalización y permanecer residente (programa residente en memoria) o después de iniciar el símbolo del sistema desde una aplicación de MS-DOS.
## <a name="syntax"></a>Sintaxis
```
ntcmdprompt
```
### <a name="parameters"></a>Parámetros

| Parámetro |             Descripción              |
|-----------|--------------------------------------|
|    /?     | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios
- Cuando **Command.com** se está ejecutando, algunas características de **Cmd.exe**, como el **doskey** la visualización de historial de comandos, no están disponibles. Si prefiere ejecutar el **Cmd.exe** intérprete de comandos después de haber iniciado una Terminate y permanecer residente (programa residente en memoria) o inicie el símbolo del sistema desde una aplicación basada en MS-DOS, puede usar el **ntcmdprompt**  comando. Sin embargo, tenga en cuenta que el programa residente en memoria no pueden estar disponibles para su uso cuando se ejecuta **Cmd.exe**. Puede incluir el **ntcmdprompt** en su **Config.nt** archivo o el archivo de inicio personalizado equivalente en el archivo de información de la aplicación (Pif).
  ## <a name="examples"></a>Ejemplos
  Para incluir **ntcmdprompt** en su **Config.nt** archivo o el archivo de configuración de inicio especificado en el Pif, tipo: **ntcmdprompt**
  ## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

