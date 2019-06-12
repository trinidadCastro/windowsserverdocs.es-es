---
title: exit
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4e599f84389b23e527e3718a620d5fdfefe24edb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439463"
---
# <a name="exit"></a>exit

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

se cierra el programa Cmd.exe (el intérprete de comandos) o el script por lotes actual.  
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).  
## <a name="syntax"></a>Sintaxis  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parámetros  

| Parámetro  |                                                                                         Descripción                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      se cierra la secuencia de comandos por lotes actual en lugar de salir de Cmd.exe. Si se ejecuta desde fuera de un script por lotes, se cierra en Cmd.exe.                                      |
| <exitCode> | Especifica un valor numérico. Si **/b** se especifica, se establece la variable de entorno ERRORLEVEL a dicho número. Si se abandona **Cmd.exe**, el código de salida se establece en ese número. |
|     /?     |                                                                             Muestra la ayuda en el símbolo del sistema.                                                                             |

## <a name="BKMK_examples"></a>Ejemplos  
Para cerrar el intérprete de comandos Cmd.exe, escriba:  
```  
exit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  

