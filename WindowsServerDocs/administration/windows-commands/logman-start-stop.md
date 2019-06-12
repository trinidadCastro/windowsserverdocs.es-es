---
title: logman start | detener
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0c6027c4c9a99e45bb1c2e95cdfd4a7687a5c43b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437681"
---
# <a name="logman-start--stop"></a>logman start | detener

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

iniciar un recopilador de datos y establecer la hora de inicio a manual o detener un recopilador de datos establece y la hora de finalización en manual.  

## <a name="syntax"></a>Sintaxis  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  

|     Parámetro      |                                 Descripción                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Contextual muestra la Ayuda.                       |
| -s <computer name> |            Ejecutar el comando en el equipo remoto especificado.             |
|  -config <value>   |           Especifica el archivo de configuración que contiene las opciones de comando.            |
|    [-n] <name>     |                          Nombre del objeto de destino.                          |
|        -ets        | Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programación. |
|        -as         |               Realizar la operación solicitada de forma asincrónica.                |

## <a name="BKMK_examples"></a>Ejemplos  
El comando siguiente inicia el perf_log del recopilador de datos en el servidor_1 del equipo remoto.  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
