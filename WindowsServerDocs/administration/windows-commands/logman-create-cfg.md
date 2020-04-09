---
title: Logman crear cfg
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bfc87093-3ff5-4e19-aa93-d185fb8e2239
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51ae4b64665577aa4795527371764401ce1fe9a1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840888"
---
# <a name="logman-create-cfg"></a>Logman crear cfg

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cree un recopilador de datos de configuración.  

## <a name="syntax"></a>Sintaxis  
```  
logman create cfg <[-n] <name>> [options]  
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
|                      -[-] ni                      |                                                         Habilitar (-ni) o deshabilitar la consulta de interfaz de red (-ni).                                                          |
|             -reg < ruta de acceso [ruta de acceso [...]] >             |                                                                 Especifica los valores del registro que se van a recopilar.                                                                 |
|            -ad< consulta [consulta [...]] >            |                                                      Especifica los objetos WMI que se van a recopilar mediante el lenguaje de consulta SQL.                                                       |
|             -FTC < ruta de acceso [ruta de acceso [...]] >             |                                                           Especifica la ruta de acceso completa a los archivos que se van a recopilar.                                                            |

## <a name="remarks"></a>Comentarios  
Donde [-] aparece en la lista, un archivo extra niega la opción.  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
El siguiente comando crea un recopilador de datos de configuración llamado cfg_log mediante la clave del registro HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows NT\Currentverion\\.  
```  
logman create cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\\  
```  
El siguiente comando crea un recopilador de datos de configuración denominado cfg_log que registra todos los objetos WMI de root\wmi en la columna de base de datos MSNdis_Vendordriverversion.  
```  
logman create cfg cfg_log -mgt root\wmi:select * FROM MSNdis_Vendordriverversion  
```  
## <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
