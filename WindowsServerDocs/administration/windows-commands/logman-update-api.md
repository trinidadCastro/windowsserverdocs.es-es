---
title: Logman Update API
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6f322e52-0f9f-42b1-bd64-8b8f8fe086fc britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7739098343f7b98b0812a9b7199dea2da044786e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840598"
---
# <a name="logman-update-api"></a>Logman Update API

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Actualice las propiedades de un recopilador de datos de seguimiento de API existente.  

## <a name="syntax"></a>Sintaxis  
```  
logman update api <[-n] <name>> [options]  
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
|            -mods < ruta de acceso [ruta de acceso [...]] >             |                                                          Especifica la lista de módulos de los que se van a registrar las llamadas API.                                                           |
|     -inapis < Module! API [module! API [...]] >      |                                                         Especifica la lista de llamadas API que se van a incluir en el registro.                                                          |
|     -exapis < Module! API [module! API [...]] >      |                                                        Especifica la lista de llamadas API que se van a excluir del registro.                                                         |
|                     -[-] ano                      |                                                     Log (-ano) nombres de API solamente o no registran los nombres de API (-ano).                                                     |
|                  -[-] recursivo                   |                                          Log (-Recursive) o no registra (-recursivo) API de forma recursiva más allá de la primera capa.                                           |
|                   -exe <value>                   |                                                        Especifica la ruta de acceso completa a un archivo ejecutable para el seguimiento de la API.                                                        |

## <a name="remarks"></a>Comentarios  
Donde [-] aparece en la lista, un archivo extra niega la opción.  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
El siguiente comando actualiza el contador de seguimiento de API existente denominado trace_notepad del archivo ejecutable c:\WINDOWS\NOTEPAD.exe excluyendo la llamada API TlsGetValue generada por el módulo Kernel32. dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
## <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
[Logman Create API](logman-create-api.md)  
