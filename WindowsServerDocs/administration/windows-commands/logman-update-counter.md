---
title: logman update contador
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6cefb171704d02580dc33a2753f84790bee630d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437563"
---
# <a name="logman-update-counter"></a>logman update contador

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Actualizar las propiedades de un contador recopilador de datos existente.  

## <a name="syntax"></a>Sintaxis  
```  
logman update counter <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  

|                    Parámetro                     |                                                                               Descripción                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Contextual muestra la Ayuda.                                                                     |
|                -s <computer name>                |                                                          Ejecutar el comando en el equipo remoto especificado.                                                          |
|                 -config <value>                  |                                                         Especifica el archivo de configuración que contiene las opciones de comando.                                                         |
|                   [-n] <name>                    |                                                                       Nombre del objeto de destino.                                                                        |
| -f < bin&#124;bincirc&#124;csv&#124;tsv&#124;sql > |                                                            Especifica el formato de registro del recopilador de datos.                                                             |
|             -[-]u <user [password]>              | Especifica el usuario para la ejecución. Escribir un \* para la contraseña genera una solicitud para la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |
|    -m <[start] [stop] [[start] [stop] [...]]>    |                                                Cambiar inicio manual o detener en lugar de un tiempo de inicio programado.                                                 |
|                -rf <[[hh:]mm:]ss>                |                                                        Ejecute el recopilador de datos para el período de tiempo especificado.                                                         |
|        -b < M/d/aaaa ss [AM&#124;PM] >         |                                                              Comenzar a recopilar datos a la hora especificada.                                                               |
|        -e < M/d/aaaa ss [AM&#124;PM] >         |                                                               Finalizar la recopilación de datos a la hora especificada.                                                                |
|                -si <[[hh:]mm:]ss>                |                                                 Especifica el intervalo de muestra para recopiladores de datos del contador de rendimiento.                                                  |
|              -o < ruta de acceso&#124;dsn! registro >              |                                              Especifica que el archivo de registro de salida o el DSN y el registro de nombre del conjunto en una base de datos SQL.                                               |
|                      -[-]r                       |                                                  Repita el recopilador de datos diariamente a las de inicio especificada y horas de finalización.                                                  |
|                      -[-]a                       |                                                                     Anexar a un archivo de registro existente.                                                                     |
|                      -[-] ow                      |                                                                     Sobrescribir un archivo de registro existente.                                                                     |
|           -[-]v <nnnnnn&#124;mmddhhmm>           |                                                   adjuntar información de control de versiones de archivo al final del nombre del archivo de registro.                                                   |
|                  -[-] rc <task>                   |                                                         Ejecute el comando especificado cada vez que se cierra el registro.                                                          |
|                 -max [-] <value>                  |                                                 Tamaño de archivo de registro máximo en MB o el número máximo de registros para los registros SQL.                                                  |
|              -[-]cnf <[[hh:]mm:]ss>              |     Cuando se especifica el tiempo, cree un nuevo archivo cuando ha transcurrido el tiempo especificado. Cuando no se especifica el tiempo, cree un nuevo archivo cuando se supera el tamaño máximo.     |
|                        -y                        |                                                             Responda Sí a todas las preguntas sin preguntar.                                                              |
|                  cf- <filename>                  |                       Especifica el archivo de lista de contadores de rendimiento para recopilar. El archivo debe contener un nombre de contador de rendimiento por línea.                        |
|               -c <path [path [ ]]>               |                                                              Especifica los contadores de rendimiento para recopilar.                                                               |
|                   -sc <value>                    |                                      Especifica el número máximo de muestras que se recopilarán con un recopilador de datos del contador de rendimiento.                                      |

## <a name="remarks"></a>Comentarios  
Donde verá [-], un - extra niega la opción.  
## <a name="BKMK_examples"></a>Ejemplos  
El comando siguiente actualiza el perf_log del recopilador de datos, cambiar el intervalo de muestra para 10 y el formato de registro a CSV y agregar control de versiones para el nombre de archivo de registro en el formato de mmddhhmm.  
```  
logman update perf_log -si 10 -f csv -v mmddhhmm  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
[logman crear contador](logman-create-counter.md)  
