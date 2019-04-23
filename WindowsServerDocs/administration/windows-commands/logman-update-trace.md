---
title: logman update trace
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b7111f7f-4162-4d1a-8e53-d766db0ede1f britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30fe475fb6a8442558493dcd5a91ecb8fcee2de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826436"
---
# <a name="logman-update-trace"></a>logman update trace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Actualizar las propiedades de un recopilador de datos de seguimiento de eventos existente.  
  
## <a name="syntax"></a>Sintaxis  
```  
logman update trace <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|/?|Contextual muestra la Ayuda.|  
|-s <computer name>|Ejecutar el comando en el equipo remoto especificado.|  
|-config <value>|Especifica el archivo de configuración que contiene las opciones de comando.|  
|-ets|Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programación.|  
|[-n] <name>|Nombre del objeto de destino.|  
|-f < bin&#124;bincirc&#124;csv&#124;tsv&#124;sql >|Especifica el formato de registro del recopilador de datos.|  
|-[-]u <user [password]>|Especifica el usuario para la ejecución. Escribir un * para la contraseña genera una solicitud para la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña.|  
|-m <[start] [stop] [[start] [stop] [...]]>|Cambiar inicio manual o detener en lugar de un tiempo de inicio programado.|  
|-rf <[[hh:]mm:]ss>|Ejecute el recopilador de datos para el período de tiempo especificado.|  
|-b < M/d/aaaa ss [AM&#124;PM] >|Comenzar a recopilar datos a la hora especificada.|  
|-e < M/d/aaaa ss [AM&#124;PM] >|Finalizar la recopilación de datos a la hora especificada.|  
|-o < ruta de acceso&#124;dsn! registro >|Especifica que el archivo de registro de salida o el DSN y el registro de nombre del conjunto en una base de datos SQL.|  
|-[-]r|Repita el recopilador de datos diariamente a las de inicio especificada y horas de finalización.|  
|-[-]a|Anexar a un archivo de registro existente.|  
|-[-] ow|Sobrescribir un archivo de registro existente.|  
|-[-]v <nnnnnn&#124;mmddhhmm>|adjuntar información de control de versiones de archivo al final del nombre del archivo de registro.|  
|-[-] rc <task>|Ejecute el comando especificado cada vez que se cierra el registro.|  
|-max [-] <value>|Tamaño de archivo de registro máximo en MB o el número máximo de registros para los registros SQL.|  
|-[-]cnf <[[hh:]mm:]ss>|Cuando se especifica el tiempo, cree un nuevo archivo cuando ha transcurrido el tiempo especificado. Cuando no se especifica el tiempo, cree un nuevo archivo cuando se supera el tamaño máximo.|  
|-y|Responda Sí a todas las preguntas sin preguntar.|  
|-ct < perf&#124;sistema&#124;ciclo >|Especifica el tipo de reloj de la sesión de seguimiento de eventos.|  
|-ln <logger_name>|Especifica el nombre de registrador para sesiones de seguimiento de eventos.|  
|-ft < [[hh:] mm:] ss >|Especifica el temporizador de vaciado de sesión de seguimiento de eventos.|  
|-[-] p < proveedor [indicadores [nivel]] >|Especifica un único proveedor de seguimiento de eventos para habilitar.|  
|-pf <filename>|Especifica un archivo de varios proveedores de seguimiento de eventos para habilitar. El archivo debe ser un archivo de texto que contiene un proveedor por línea.|  
|-[-]rt|Ejecutar la sesión de seguimiento de eventos en el modo en tiempo real.|  
|-ul [-]|Ejecutar la sesión de seguimiento de eventos en modo de usuario.|  
|-bs <value>|Especifica el tamaño del búfer de sesión de seguimiento de eventos en kb.|  
|-nb <min max>|Especifica el número de búferes de sesión de seguimiento de eventos.|  
|-modo < globalsequence&#124;localsequence&#124;pagedmemory >|Especifica el modo de registrador de sesión de seguimiento de eventos.<br /><br />**Globalsequence** especifica que el seguimiento de eventos agrega un número de secuencia para todos los eventos que recibe con independencia de qué seguimiento de sesión ha recibido el evento.<br /><br />**Localsequence** especifica que el seguimiento de eventos agregar números de secuencia de eventos recibidos en una sesión de seguimiento específicos. Cuando el **localsequence** se usa la opción, los números de secuencia duplicados pueden existir en todas las sesiones, pero serán únicos dentro de cada sesión de seguimiento.<br /><br />**Pagedmemory** especifica que el seguimiento de eventos usan memoria paginada en lugar de con el grupo de memoria no paginada predeterminado para sus asignaciones de búfer interno.|  
## <a name="remarks"></a>Comentarios  
Donde verá [-], un - extra niega la opción.  
## <a name="BKMK_examples"></a>Ejemplos  
El comando siguiente actualiza el perf_log de recopilador de datos existente, cambiar el tamaño máximo del registro a 10 MB, actualizar el formato de archivo de registro a CSV y anexar el control de versiones de archivo en el formato de mmddhhmm.  
```  
logman update perf_log -max 10 -f csv -v mmddhhmm  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
[¡Logman create trace](logman-create-trace.md)  