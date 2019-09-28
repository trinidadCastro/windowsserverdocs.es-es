---
title: lpq
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a3755c010c9bb4549deed08f26b5a0670fe7318
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374204"
---
# <a name="lpq"></a>lpq

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra el estado de una cola de impresión en un equipo que ejecuta line Printer daemon (LPD).  

## <a name="syntax"></a>Sintaxis  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Parámetros  

|    Parámetro     |                                                                        Descripción                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Especifica (por nombre o dirección IP) el equipo o el dispositivo de uso compartido de impresoras que hospeda la cola de impresión de LPD con el estado que desea mostrar. Obligatorio. |
| -P <printerName> |                           Especifica (por nombre) la impresora para la cola de impresión con el estado que desea mostrar. Obligatorio.                           |
|        -l        |                                      Especifica que desea mostrar detalles sobre el estado de la cola de impresión.                                      |
|        /?        |                                                           Muestra la ayuda en el símbolo del sistema.                                                            |

## <a name="remarks"></a>Comentarios  
Los parámetros **-S** y **-P** distinguen mayúsculas de minúsculas y deben escribirse en letras mayúsculas.  
## <a name="BKMK_examples"></a>Example  
En este ejemplo se muestra cómo mostrar el estado de la cola de impresión Laserprinter1 en un host LPD en 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Referencia de comandos de impresión](print-command-reference.md)  
