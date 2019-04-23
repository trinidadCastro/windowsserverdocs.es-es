---
title: alerta de logman update
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ede94a76-931c-40ed-9fda-6766bed8ff72 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8de8db0ae02b5e0ae439f9adb742bfc316abb82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847786"
---
# <a name="logman-update-alert"></a>alerta de logman update

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Actualizar las propiedades de un recopilador de datos de alerta existente.  
  
## <a name="syntax"></a>Sintaxis  
```  
logman update alert <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|/?|Contextual muestra la Ayuda.|  
|-s <computer name>|Ejecutar el comando en el equipo remoto especificado.|  
|-config <value>|Especifica el archivo de configuración que contiene las opciones de comando.|  
|[-n] <name>|Nombre del objeto de destino.|  
|-[-]u <user [password]>|Especifica el usuario para la ejecución. Escribir un * para la contraseña genera una solicitud para la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña.|  
|-m <[start] [stop] [[start] [stop] [...]]>|Cambiar inicio manual o detener en lugar de un tiempo de inicio programado.|  
|-rf <[[hh:]mm:]ss>|Ejecute el recopilador de datos para el período de tiempo especificado.|  
|-b < M/d/aaaa ss [AM&#124;PM] >|Comenzar a recopilar datos a la hora especificada.|  
|-e < M/d/aaaa ss [AM&#124;PM] >|Finalizar la recopilación de datos a la hora especificada.|  
|-si <[[hh:]mm:]ss>|Especifica el intervalo de muestra para recopiladores de datos del contador de rendimiento.|  
|-o < ruta de acceso&#124;dsn! registro >|Especifica que el archivo de registro de salida o el DSN y el registro de nombre del conjunto en una base de datos SQL.|  
|-[-]r|Repita el recopilador de datos diariamente a las de inicio especificada y horas de finalización.|  
|-[-]a|Anexar a un archivo de registro existente.|  
|-[-] ow|Sobrescribir un archivo de registro existente.|  
|-[-]v <nnnnnn&#124;mmddhhmm>|adjuntar información de control de versiones de archivo al final del nombre del archivo de registro.|  
|-[-] rc <task>|Ejecute el comando especificado cada vez que se cierra el registro.|  
|-max [-] <value>|Tamaño de archivo de registro máximo en MB o el número máximo de registros para los registros SQL.|  
|-[-]cnf <[[hh:]mm:]ss>|Cuando se especifica el tiempo, cree un nuevo archivo cuando ha transcurrido el tiempo especificado. Cuando no se especifica el tiempo, cree un nuevo archivo cuando se supera el tamaño máximo.|  
|-y|Responda Sí a todas las preguntas sin preguntar.|  
|cf- <filename>|Especifica el archivo de lista de contadores de rendimiento para recopilar. El archivo debe contener un nombre de contador de rendimiento por línea.|  
|-[-]el|Habilita o deshabilita el registro de eventos de informes.|  
|-th <threshold [threshold [...]]>|Especificar contadores y sus valores de umbral para una alerta.|  
|-RDC [-] <name>|Especifica el conjunto de recopiladores de datos que se iniciará cuando se genera una alerta.|  
|-[-] tn <task>|Especifica la tarea se ejecute cuando se desencadene una alerta.|  
|-ex [-] <argument>|Especifica los argumentos de la tarea que se usará con la tarea especificada mediante -tn.|  
## <a name="remarks"></a>Comentarios  
Donde verá [-], un - extra niega la opción.  
## <a name="BKMK_examples"></a>Ejemplos  
El ejemplo siguiente actualiza el new_alert de recopilador de datos existente, establecer el valor de umbral para el contador % de tiempo en el grupo de contadores de procesador (_Total) al 40%.  
```  
logman update alert new_alert -th "\Processor(_Total)\% Processor time>40"  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
[logman crear alerta](logman-create-alert.md)  
