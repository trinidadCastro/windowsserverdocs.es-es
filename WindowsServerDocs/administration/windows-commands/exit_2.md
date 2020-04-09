---
title: exit
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13cf7a7658394e59ce6cc7e66c3083cd3d359574
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844888"
---
# <a name="exit"></a>exit

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sale del programa cmd. exe (el intérprete de comandos) o del script por lotes actual.  
Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).  
## <a name="syntax"></a>Sintaxis  
```  
exit [/b] [<exitCode>]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro  |                                                                                         Descripción                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      Sale del script por lotes actual en lugar de salir de cmd. exe. Si se ejecuta desde fuera de un script por lotes, sale de cmd. exe.                                      |
| `<exitCode>` | Especifica un número numérico. Si se especifica **/b** , la variable de entorno ERRORLEVEL se establece en ese número. Si sale de **cmd. exe**, el código de salida del proceso se establece en ese número. |
|     /?     |                                                                             Muestra la Ayuda en el símbolo del sistema.                                                                             |

## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para cerrar el intérprete de comandos, cmd. exe, escriba:  
```  
exit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  

