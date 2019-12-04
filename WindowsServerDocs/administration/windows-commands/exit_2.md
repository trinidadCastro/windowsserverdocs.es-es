---
title: exit
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6d3a4d0bfc74644f6fda43abe57e0e4e7c1264a
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791230"
---
# <a name="exit"></a>exit

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sale del programa cmd. exe (el intérprete de comandos) o del script por lotes actual.  
Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).  
## <a name="syntax"></a>Sintaxis  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parámetros  

| Parámetro  |                                                                                         Descripción                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     b     |                                      Sale del script por lotes actual en lugar de salir de cmd. exe. Si se ejecuta desde fuera de un script por lotes, sale de cmd. exe.                                      |
| `<exitCode>` | Especifica un número numérico. Si se especifica **/b** , la variable de entorno ERRORLEVEL se establece en ese número. Si sale de **cmd. exe**, el código de salida del proceso se establece en ese número. |
|     /?     |                                                                             Muestra la ayuda en el símbolo del sistema.                                                                             |

## <a name="BKMK_examples"></a>Example  
Para cerrar el intérprete de comandos, cmd. exe, escriba:  
```  
exit  
```  
## <a name="additional-references"></a>referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  

