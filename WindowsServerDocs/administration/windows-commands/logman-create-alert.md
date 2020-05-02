---
title: Logman crear alerta
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93e6fc2b-5bf5-413b-84b4-be8b9dd3a57d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c56b8ee9ca9aa96cbff9af6f3726c62a21d7a17
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724431"
---
# <a name="logman-create-alert"></a>Logman crear alerta

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

crear un recopilador de datos de alertas.  

## <a name="syntax"></a>Sintaxis  
```  
logman create alert <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parámetros  

|                 Parámetro                  |                                                                               Descripción                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    Muestra la ayuda contextual.                                                                     |
|             -s<computer name>             |                                                          Ejecute el comando en el equipo remoto especificado.                                                          |
|              -config <value>               |                                                         Especifica el archivo de configuración que contiene opciones de comando.                                                         |
|                [-n]<name>                 |                                                                       Nombre del objeto de destino.                                                                        |
|          -[-] u <usuario [contraseña] >           | Especifica el usuario que se va a ejecutar como. Al escribir \* un para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |
| -m < [Start] [STOP] [[Start] [STOP] [...]] > |                                                cambie a Inicio o detención manual en lugar de a una hora de inicio o de finalización programada.                                                 |
|             -RF < [[HH:] mm:] SS>             |                                                        Ejecute el recopilador de datos durante el período de tiempo especificado.                                                         |
|     -b <M/d/YYYY h:mm: SS [AM&#124;PM] >      |                                                              Comienza a recopilar datos en el momento especificado.                                                               |
|     -e <M/d/YYYY h:mm: SS [AM&#124;PM] >      |                                                               Finaliza la recopilación de datos en el momento especificado.                                                                |
|             -Si < [[HH:] mm:] SS>             |                                                 Especifica el intervalo de ejemplo para los recopiladores de datos del contador de rendimiento.                                                  |
|           -o <ruta de acceso&#124;DSN>           |                                              Especifica el archivo de registro de salida o el DSN y el nombre del conjunto de registros en una base de datos SQL.                                               |
|                   -[-] r                    |                                                  Repetir el recopilador de datos diariamente en las horas de inicio y finalización especificadas.                                                  |
|                   -[-] a                    |                                                                     anexar a un archivo de registro existente.                                                                     |
|                   -[-] permitir                   |                                                                     Sobrescribir un archivo de registro existente.                                                                     |
|        -[-] v <nnnnnn&#124;mmddHHMM>        |                                                   Adjunte información de versión del archivo al final del nombre del archivo de registro.                                                   |
|               -[-] RC<task>                |                                                         Ejecute el comando especificado cada vez que se cierre el registro.                                                          |
|              -[-] máx. <value>               |                                                 Tamaño máximo del archivo de registro en MB o número máximo de registros para los registros de SQL.                                                  |
|           -[-] CNF < [[HH:] mm:] SS>           |     Cuando se especifica Time, cree un nuevo archivo cuando haya transcurrido el tiempo especificado. Si no se especifica Time, cree un archivo nuevo cuando se supere el tamaño máximo.     |
|                     -y                     |                                                             Responda sí a todas las preguntas sin preguntar.                                                              |
|               -CF<filename>               |                       Especifica el archivo que muestra los contadores de rendimiento que se van a recopilar. El archivo debe contener un nombre de contador de rendimiento por línea.                        |
|                   -[-] el                   |                                                                Habilita o deshabilita los informes del registro de eventos.                                                                 |
|     -TH <umbral [umbral [...]] >      |                                                        Especifique los contadores y sus valores de umbral para una alerta.                                                        |
|              -[-]rdcs<name>               |                                                     Especifica el conjunto de recopiladores de datos que se va a iniciar cuando se desencadene una alerta.                                                      |
|               -[-] TN<task>                |                                                             Especifica la tarea que se va a ejecutar cuando se desencadene una alerta.                                                              |
|            -[-] ex<argument>             |                                               Especifica los argumentos de tarea que se van a usar con la tarea especificada mediante-TN.                                                |

## <a name="remarks"></a>Observaciones  
Donde [-] aparece en la lista, un archivo extra niega la opción.  
## <a name="examples"></a>Ejemplos  
El siguiente comando crea una alerta denominada new_alert que se activa cuando el contador de rendimiento% de tiempo de procesador del grupo de contadores del procesador (_Total) supera el valor del contador de 50.  
```  
logman create alert new_alert -th \Processor(_Total)\% Processor time>50  
```  
> [!NOTE]
> El valor de umbral definido se basa en el valor recopilado por el contador, por lo que en este ejemplo, el valor de 50 equivale al 50% de tiempo de procesador.  
> ## <a name="additional-references"></a>Referencias adicionales  
> [logman](logman.md)  
