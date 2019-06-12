---
title: logman crear alerta
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93e6fc2b-5bf5-413b-84b4-be8b9dd3a57d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37a13ab5623a295f96bde2f734bcb17e1eca2be9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437843"
---
# <a name="logman-create-alert"></a>logman crear alerta

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crear un recopilador de datos de alertas.  

## <a name="syntax"></a>Sintaxis  
```  
logman create alert <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  

|                 Parámetro                  |                                                                               Descripción                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    Contextual muestra la Ayuda.                                                                     |
|             -s <computer name>             |                                                          Ejecutar el comando en el equipo remoto especificado.                                                          |
|              -config <value>               |                                                         Especifica el archivo de configuración que contiene las opciones de comando.                                                         |
|                [-n] <name>                 |                                                                       Nombre del objeto de destino.                                                                        |
|          -[-]u <user [password]>           | Especifica el usuario para la ejecución. Escribir un \* para la contraseña genera una solicitud para la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |
| -m <[start] [stop] [[start] [stop] [...]]> |                                                Cambiar inicio manual o detener en lugar de un tiempo de inicio programado.                                                 |
|             -rf <[[hh:]mm:]ss>             |                                                        Ejecute el recopilador de datos para el período de tiempo especificado.                                                         |
|     -b < M/d/aaaa ss [AM&#124;PM] >      |                                                              Comenzar a recopilar datos a la hora especificada.                                                               |
|     -e < M/d/aaaa ss [AM&#124;PM] >      |                                                               Finalizar la recopilación de datos a la hora especificada.                                                                |
|             -si <[[hh:]mm:]ss>             |                                                 Especifica el intervalo de muestra para recopiladores de datos del contador de rendimiento.                                                  |
|           -o < ruta de acceso&#124;dsn! registro >           |                                              Especifica que el archivo de registro de salida o el DSN y el registro de nombre del conjunto en una base de datos SQL.                                               |
|                   -[-]r                    |                                                  Repita el recopilador de datos diariamente a las de inicio especificada y horas de finalización.                                                  |
|                   -[-]a                    |                                                                     Anexar a un archivo de registro existente.                                                                     |
|                   -[-] ow                   |                                                                     Sobrescribir un archivo de registro existente.                                                                     |
|        -[-]v <nnnnnn&#124;mmddhhmm>        |                                                   adjuntar información de control de versiones de archivo al final del nombre del archivo de registro.                                                   |
|               -[-] rc <task>                |                                                         Ejecute el comando especificado cada vez que se cierra el registro.                                                          |
|              -max [-] <value>               |                                                 Tamaño de archivo de registro máximo en MB o el número máximo de registros para los registros SQL.                                                  |
|           -[-]cnf <[[hh:]mm:]ss>           |     Cuando se especifica el tiempo, cree un nuevo archivo cuando ha transcurrido el tiempo especificado. Cuando no se especifica el tiempo, cree un nuevo archivo cuando se supera el tamaño máximo.     |
|                     -y                     |                                                             Responda Sí a todas las preguntas sin preguntar.                                                              |
|               cf- <filename>               |                       Especifica el archivo de lista de contadores de rendimiento para recopilar. El archivo debe contener un nombre de contador de rendimiento por línea.                        |
|                   -[-]el                   |                                                                Habilita o deshabilita el registro de eventos de informes.                                                                 |
|     -th <threshold [threshold [...]]>      |                                                        Especificar contadores y sus valores de umbral para una alerta.                                                        |
|              -RDC [-] <name>               |                                                     Especifica el conjunto de recopiladores de datos que se iniciará cuando se genera una alerta.                                                      |
|               -[-] tn <task>                |                                                             Especifica la tarea se ejecute cuando se desencadene una alerta.                                                              |
|            -ex [-] <argument>             |                                               Especifica los argumentos de la tarea que se usará con la tarea especificada mediante -tn.                                                |

## <a name="remarks"></a>Comentarios  
Donde verá [-], un - extra niega la opción.  
## <a name="BKMK_examples"></a>Ejemplos  
El siguiente comando crea una alerta denominada new_alert que se desencadena cuando el tiempo de procesador de % de contador de rendimiento en el grupo de contadores de procesador (_Total) supera el valor del contador de 50.  
```  
logman create alert new_alert -th "\Processor(_Total)\% Processor time>50"  
```  
> [!NOTE]
> El valor de umbral definido se basa en el valor recopilado por el contador, por lo que en este ejemplo, el valor de 50 equivale al 50% de tiempo de procesador.  
> #### <a name="additional-references"></a>Referencias adicionales  
> [logman](logman.md)  
