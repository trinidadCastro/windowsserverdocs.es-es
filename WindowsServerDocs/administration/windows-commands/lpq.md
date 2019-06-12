---
title: lpq
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 18ff1ff3ecbc2df0a437ec8a465dec9a12123ede
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437512"
---
# <a name="lpq"></a>lpq

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra el estado de una cola de impresión en un equipo que ejecuta Line printer Daemon (LPD).  

## <a name="syntax"></a>Sintaxis  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Parámetros  

|    Parámetro     |                                                                        Descripción                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Especifica (por nombre o dirección IP), el equipo o dispositivo que hospeda la cola de impresión de LPD con un estado que desea mostrar de compartir impresoras. Obligatorio. |
| -P <printerName> |                           Especifica (por nombre) la impresora para la cola de impresión con un estado que desea mostrar. Obligatorio.                           |
|        -l        |                                      Especifica que desea mostrar los detalles sobre el estado de la cola de impresión.                                      |
|        /?        |                                                           Muestra la ayuda en el símbolo del sistema.                                                            |

## <a name="remarks"></a>Comentarios  
El **-S** y **-P** los parámetros distinguen entre mayúsculas y minúsculas y debe escribirse en letras mayúsculas.  
## <a name="BKMK_examples"></a>Ejemplos  
En este ejemplo se muestra cómo mostrar el estado de la cola de impresión Laserprinter1 en un host LPD en 10.0.0.45:  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Referencia de comandos de impresión](print-command-reference.md)  
