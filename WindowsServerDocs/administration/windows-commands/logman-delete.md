---
title: logman delete
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e396d79dc936f56a69fac9469c020348640f1094
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437779"
---
# <a name="logman-delete"></a>logman delete

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

eliminar un recopilador de datos existente.  

## <a name="syntax"></a>Sintaxis  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  

|        Parámetro        |                                                                               Descripción                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Contextual muestra la Ayuda.                                                                     |
|   -s <computer name>    |                                                          Ejecutar el comando en el equipo remoto especificado.                                                          |
|     -config <value>     |                                                         Especifica el archivo de configuración que contiene las opciones de comando.                                                         |
|       [-n] <name>       |                                                                   Nombre del recopilador de datos de destino.                                                                    |
|          -ets           |                                              Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programación.                                               |
| -[-]u <user [password]> | Especifica el usuario para la ejecución. Escribir un \* para la contraseña genera una solicitud para la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |

## <a name="BKMK_examples"></a>Ejemplos  
El siguiente comando elimina el perf_log del recopilador de datos.  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
