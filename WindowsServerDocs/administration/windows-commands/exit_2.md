---
title: exit
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47dfa42f2bacb3fe9f12d1da9163bcf828e9488
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725705"
---
# <a name="exit"></a>exit

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Sale del programa cmd. exe (el intérprete de comandos) o del script por lotes actual.  
  
## <a name="syntax"></a>Sintaxis  
```  
exit [/b] [<exitCode>]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro  |                                                                                         Descripción                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      Sale del script por lotes actual en lugar de salir de cmd. exe. Si se ejecuta desde fuera de un script por lotes, sale de cmd. exe.                                      |
| `<exitCode>` | Especifica un número numérico. Si se especifica **/b** , la variable de entorno ERRORLEVEL se establece en ese número. Si sale de **cmd. exe**, el código de salida del proceso se establece en ese número. |
|     /?     |                                                                             Muestra la ayuda en el símbolo del sistema.                                                                             |

## <a name="examples"></a>Ejemplos  
Para cerrar el intérprete de comandos, cmd. exe, escriba:  
```  
exit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  

