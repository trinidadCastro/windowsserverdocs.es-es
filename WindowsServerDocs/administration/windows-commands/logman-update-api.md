---
title: logman update api
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6f322e52-0f9f-42b1-bd64-8b8f8fe086fc britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 285e4b527cf02061380ab2d9b5525e5b297a43cc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437666"
---
# <a name="logman-update-api"></a>logman update api

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Actualizar las propiedades de un recopilador de datos de seguimiento de API existente.  

## <a name="syntax"></a>Sintaxis  
```  
logman update api <[-n] <name>> [options]  
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
|            -mods <path [path [...]]>             |                                                          Especifica la lista de módulos para registrar las llamadas de API de.                                                           |
|     -inapis <module!api [module!api [...]]>      |                                                         Especifica la lista de llamadas de API para incluir en el registro.                                                          |
|     -exapis <module!api [module!api [...]]>      |                                                        Especifica la lista de llamadas API que excluir del registro.                                                         |
|                     -[-] ano                      |                                                     Registro (--ano) solo, los nombres de API o no solo registro (--ano) los nombres de API.                                                     |
|                  -recursivas [-]                   |                                          Registro (-recursivo) o no iniciar sesión (-recursivo) las API recursivamente más allá de la primera capa.                                           |
|                   -exe <value>                   |                                                        Especifica la ruta de acceso completa a un archivo ejecutable para el seguimiento de API.                                                        |

## <a name="remarks"></a>Comentarios  
Donde verá [-], un - extra niega la opción.  
## <a name="BKMK_examples"></a>Ejemplos  
El siguiente contador de seguimiento de las actualizaciones de la API existente de comando llamado trace_notepad para el archivo ejecutable de c:\windows\notepad.exe excluyendo la llamada API TlsGetValue generado por el módulo kernel32.dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
[logman crear api](logman-create-api.md)  