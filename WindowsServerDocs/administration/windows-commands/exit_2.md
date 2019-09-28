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
ms.openlocfilehash: 105bf572c1ebeb37ea59ff8bc5c04121d2442341
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377351"
---
# <a name="exit"></a>exit

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

sale del programa cmd. exe (el intérprete de comandos) o del script por lotes actual.  
Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).  
## <a name="syntax"></a>Sintaxis  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parámetros  

| Parámetro  |                                                                                         Descripción                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     b     |                                      sale del script por lotes actual en lugar de salir de cmd. exe. Si se ejecuta desde fuera de un script por lotes, sale de cmd. exe.                                      |
| <exitCode> | Especifica un número numérico. Si se especifica **/b** , la variable de entorno ERRORLEVEL se establece en ese número. Si sale de **cmd. exe**, el código de salida del proceso se establece en ese número. |
|     /?     |                                                                             Muestra la ayuda en el símbolo del sistema.                                                                             |

## <a name="BKMK_examples"></a>Example  
Para cerrar el intérprete de comandos, cmd. exe, escriba:  
```  
exit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  

