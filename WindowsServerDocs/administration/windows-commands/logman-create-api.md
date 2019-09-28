---
title: Logman Create API
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ecc0a75-2613-464a-8616-c5dc404bb736
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 512602213fcfd95770af0e27b721a589ed489771
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374600"
---
# <a name="logman-create-api"></a>Logman Create API

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cree un recopilador de datos de seguimiento de API.  

## <a name="syntax"></a>Sintaxis  
```  
logman create api <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  

|                    Parámetro                     |                                                                               Descripción                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Muestra la ayuda contextual.                                                                     |
|                -s <computer name>                |                                                          Ejecute el comando en el equipo remoto especificado.                                                          |
|                 -config <value>                  |                                                         Especifica el archivo de configuración que contiene opciones de comando.                                                         |
|                   [-n] <name>                    |                                                                       Nombre del objeto de destino.                                                                        |
| -f < bin&#124;bincirc&#124;CSV&#124;TSV&#124;SQL > |                                                            Especifica el formato del registro del recopilador de datos.                                                             |
|             -[-] u < usuario [contraseña] >              | Especifica el usuario que se va a ejecutar como. Al escribir un \* para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |
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
|            -mods < ruta de acceso [ruta de acceso [...]] >             |                                                          Especifica la lista de módulos de los que se van a registrar las llamadas API.                                                           |
|     -inapis < Module! API [module! API [...]] >      |                                                         Especifica la lista de llamadas API que se van a incluir en el registro.                                                          |
|     -exapis < Module! API [module! API [...]] >      |                                                        Especifica la lista de llamadas API que se van a excluir del registro.                                                         |
|                     -[-] ano                      |                                                     Log (-ano) nombres de API solamente o no registran los nombres de API (-ano).                                                     |
|                  -[-] recursivo                   |                                          Log (-Recursive) o no registra (-recursivo) API de forma recursiva más allá de la primera capa.                                           |
|                   -exe <value>                   |                                                        Especifica la ruta de acceso completa a un archivo ejecutable para el seguimiento de la API.                                                        |

## <a name="remarks"></a>Comentarios  
Donde [-] aparece en la lista, un archivo extra niega la opción.  
## <a name="BKMK_examples"></a>Example  
El siguiente comando crea un contador de seguimiento de API denominado trace_notepad para el archivo ejecutable c:\WINDOWS\NOTEPAD.exe y envía los resultados al archivo c:\notepad.ETL.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -o c:\notepad.etl  
```  
El siguiente comando crea un contador de seguimiento de API denominado trace_notepad para el archivo ejecutable c:\WINDOWS\NOTEPAD.exe recopilar los valores generados por el módulo c:\windows\system32\advapi32.dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -mods c:\windows\system32\advapi32.dll  
```  
El siguiente comando crea un contador de seguimiento de API denominado trace_notepad para el archivo ejecutable c:\WINDOWS\NOTEPAD.exe sin incluir la llamada de API TlsGetValue generada por el módulo Kernel32. dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
