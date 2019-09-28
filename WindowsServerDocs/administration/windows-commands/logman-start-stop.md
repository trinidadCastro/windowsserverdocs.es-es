---
title: ejecutar Logman | detener
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 395d325b31ee596e1394e7ed796a444f159d15fc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374410"
---
# <a name="logman-start--stop"></a>ejecutar Logman | detener

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicie un recopilador de datos y establezca la hora de inicio en manual, o bien detenga un conjunto de recopiladores de datos y establezca la hora de finalización en manual.  

## <a name="syntax"></a>Sintaxis  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  

|     Parámetro      |                                 Descripción                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Muestra la ayuda contextual.                       |
| -s <computer name> |            Ejecute el comando en el equipo remoto especificado.             |
|  -config <value>   |           Especifica el archivo de configuración que contiene opciones de comando.            |
|    [-n] <name>     |                          Nombre del objeto de destino.                          |
|        -ETS        | Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programar. |
|        -as         |               Realizar la operación solicitada de forma asincrónica.                |

## <a name="BKMK_examples"></a>Example  
El comando siguiente inicia el recopilador de datos perf_log en el equipo remoto server_1.  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
