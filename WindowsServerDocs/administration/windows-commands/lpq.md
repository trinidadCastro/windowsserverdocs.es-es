---
title: lpq
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 051b1983fcc0fddd7b69e561c0a27a120f78d998
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840398"
---
# <a name="lpq"></a>lpq

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra el estado de una cola de impresión en un equipo que ejecuta line Printer daemon (LPD).  

## <a name="syntax"></a>Sintaxis  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
### <a name="parameters"></a>Parámetros  

|    Parámetro     |                                                                        Descripción                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Especifica (por nombre o dirección IP) el equipo o el dispositivo de uso compartido de impresoras que hospeda la cola de impresión de LPD con el estado que desea mostrar. Obligatorio. |
| -P <printerName> |                           Especifica (por nombre) la impresora para la cola de impresión con el estado que desea mostrar. Obligatorio.                           |
|        -l        |                                      Especifica que desea mostrar detalles sobre el estado de la cola de impresión.                                      |
|        /?        |                                                           Muestra la Ayuda en el símbolo del sistema.                                                            |

## <a name="remarks"></a>Comentarios  
Los parámetros **-S** y **-P** distinguen mayúsculas de minúsculas y deben escribirse en letras mayúsculas.  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
En este ejemplo se muestra cómo mostrar el estado de la cola de impresión Laserprinter1 en un host LPD en 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Referencia de comandos de impresión](print-command-reference.md)  
