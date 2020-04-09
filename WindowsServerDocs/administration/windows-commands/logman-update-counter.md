---
title: Logman (actualizar contador)
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d93ea91fb1b5d105923457aeb8d5515e1ac5b9c5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840578"
---
# <a name="logman-update-counter"></a>Logman (actualizar contador)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Actualice las propiedades de un recopilador de datos de contador existente.  

## <a name="syntax"></a>Sintaxis  
```  
logman update counter <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parámetros  

|                    Parámetro                     |                                                                               Descripción                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Muestra la ayuda contextual.                                                                     |
|                -s <computer name>                |                                                          Ejecute el comando en el equipo remoto especificado.                                                          |
|                 -config <value>                  |                                                         Especifica el archivo de configuración que contiene opciones de comando.                                                         |
|                   [-n] <name>                    |                                                                       Nombre del objeto de destino.                                                                        |
| -f < bin&#124;bincirc&#124;CSV&#124;TSV&#124;SQL > |                                                            Especifica el formato del registro del recopilador de datos.                                                             |
|             -[-] u < usuario [contraseña] >              | Especifica el usuario que se va a ejecutar como. Al escribir una \* para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |
|    -m < [Start] [STOP] [[Start] [STOP] [...]] >    |                                                cambie a Inicio o detención manual en lugar de a una hora de inicio o de finalización programada.                                                 |
|                -RF < [[HH:] mm:] SS >                |                                                        Ejecute el recopilador de datos durante el período de tiempo especificado.                                                         |
|        -b < M/d/YYYY h:mm: SS [AM&#124;PM] >         |                                                              Comienza a recopilar datos en el momento especificado.                                                               |
|        -e < M/d/YYYY h:mm: SS [AM&#124;PM] >         |                                                               Finaliza la recopilación de datos en el momento especificado.                                                                |
|                -Si < [[HH:] mm:] SS >                |                                                 Especifica el intervalo de ejemplo para los recopiladores de datos del contador de rendimiento.                                                  |
|              -o < path&#124;DSN! log >              |                                              Especifica el archivo de registro de salida o el DSN y el nombre del conjunto de registros en una base de datos SQL.                                               |
|                      -[-] r                       |                                                  Repetir el recopilador de datos diariamente en las horas de inicio y finalización especificadas.                                                  |
|                      -[-] a                       |                                                                     anexar a un archivo de registro existente.                                                                     |
|                      -[-] permitir                      |                                                                     Sobrescribir un archivo de registro existente.                                                                     |
|           -[-] v < nnnnnn&#124;mmddHHMM >           |                                                   Adjunte información de versión del archivo al final del nombre del archivo de registro.                                                   |
|                  -[-] RC <task>                   |                                                         Ejecute el comando especificado cada vez que se cierre el registro.                                                          |
|                 -[-] máx. <value>                  |                                                 Tamaño máximo del archivo de registro en MB o número máximo de registros para los registros de SQL.                                                  |
|              -[-] CNF < [[HH:] mm:] SS >              |     Cuando se especifica Time, cree un nuevo archivo cuando haya transcurrido el tiempo especificado. Si no se especifica Time, cree un archivo nuevo cuando se supere el tamaño máximo.     |
|                        -y                        |                                                             Responda sí a todas las preguntas sin preguntar.                                                              |
|                  -CF <filename>                  |                       Especifica el archivo que muestra los contadores de rendimiento que se van a recopilar. El archivo debe contener un nombre de contador de rendimiento por línea.                        |
|               -c < ruta [path []] >               |                                                              Especifica los contadores de rendimiento que se van a recopilar.                                                               |
|                   -SC <value>                    |                                      Especifica el número máximo de muestras que se van a recopilar con un recopilador de datos del contador de rendimiento.                                      |

## <a name="remarks"></a>Comentarios  
Donde [-] aparece en la lista, un archivo extra niega la opción.  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
El comando siguiente actualiza el perf_log del recopilador de datos, cambiando el intervalo de ejemplo a 10 y el formato del registro a CSV y agregando el control de versiones al nombre del archivo de registro con el formato mmddHHMM.  
```  
logman update perf_log -si 10 -f csv -v mmddhhmm  
```  
## <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
[Logman Create Counter](logman-create-counter.md)  
