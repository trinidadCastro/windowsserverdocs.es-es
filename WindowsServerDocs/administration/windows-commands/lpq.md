---
title: lpq
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2d7a013ad9481780873cd57be4fa15732fc6196
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724257"
---
# <a name="lpq"></a>lpq

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra el estado de una cola de impresión en un equipo que ejecuta line Printer daemon (LPD).  

## <a name="syntax"></a>Sintaxis  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
### <a name="parameters"></a>Parámetros  

|    Parámetro     |                                                                        Descripción                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S<ServerName>  | Especifica (por nombre o dirección IP) el equipo o el dispositivo de uso compartido de impresoras que hospeda la cola de impresión de LPD con el estado que desea mostrar. Necesario. |
| -P<printerName> |                           Especifica (por nombre) la impresora para la cola de impresión con el estado que desea mostrar. Necesario.                           |
|        -l        |                                      Especifica que desea mostrar detalles sobre el estado de la cola de impresión.                                      |
|        /?        |                                                           Muestra la ayuda en el símbolo del sistema.                                                            |

## <a name="remarks"></a>Observaciones  
Los parámetros **-S** y **-P** distinguen mayúsculas de minúsculas y deben escribirse en letras mayúsculas.  
## <a name="examples"></a>Ejemplos  
En este ejemplo se muestra cómo mostrar el estado de la cola de impresión Laserprinter1 en un host LPD en 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Referencia de comandos de impresión](print-command-reference.md)  
