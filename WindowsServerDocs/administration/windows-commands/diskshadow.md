---
title: diskshadow
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c5648235a1c856c6aef09621e2381e74d08d70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869186"
---
# <a name="diskshadow"></a>diskshadow

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DiskShadow.exe es una herramienta que expone la funcionalidad proporcionada por las instantáneas de volumen servicio \(VSS\). De forma predeterminada, diskshadow usa un intérprete de comandos interactivo parecido de diskraid o DiskPart. Además, incorpora un modo que permite ejecutar scripts.  
  
> [!NOTE]  
> Pertenencia al grupo Administradores local, o equivalente, es lo mínimo necesario para ejecutar diskshadow.  
  
Para obtener ejemplos de cómo utilizar los comandos de diskshadow, consulte [ejemplos](#BKMK_examples).  
  
## <a name="syntax"></a>Sintaxis  
para el modo interactivo, escriba lo siguiente en el símbolo del sistema para iniciar el intérprete de comandos de diskshadow:  
  
```  
diskshadow  
```  
  
para el modo de secuencia de comandos, escriba lo siguiente, donde *script.txt* es un archivo de script que contiene los comandos de diskshadow:  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>comandos de DiskShadow  
Puede ejecutar los comandos siguientes en el intérprete de comandos de diskshadow o a través de un archivo de script:  
  
|Parámetro|Descripción|  
|-------|--------|  
|[set_2](set_2.md)|Establece el contexto, opciones, el modo detallado y archivo de metadatos para la creación de instantáneas.|  
|[Simular la restauración](simulate-restore.md)|Comprueba la participación del sistema de escritura en las sesiones de restauración en el equipo sin emitir **PreRestore** o **PostRestore** eventos a los escritores.|  
|[Cargar metadatos](load-metadata.md)|Carga un archivo .cab de metadatos antes de importar una copia de instantáneas transportables o carga los metadatos del escritor en el caso de una restauración.|  
|[writer](writer.md)|Comprueba que un sistema de escritura o un componente se incluye o excluye un componente o sistema de escritura de los procedimientos de copia de seguridad y restauración.|  
|[add_1](add_1.md)|los volúmenes se agrega al conjunto de volúmenes de instantáneas, o agrega alias para el entorno de alias.|  
|[create_1](create_1.md)|inicia el proceso de creación de la copia de instantáneas, usando la configuración actual de contexto y la opción.|  
|[exec](exec.md)|ejecuta un archivo en el equipo local.|  
|[Comenzar la copia de seguridad](begin-backup.md)|Inicia una sesión de copia de seguridad completa.|  
|[Copia de seguridad final](end-backup.md)|Finaliza una sesión de copia de seguridad completa y problemas de un **Backupcomplete** eventos con el estado de sistema de escritura adecuados, si es necesario.|  
|[Comenzar la restauración](begin-restore.md)|Inicia una sesión de restauración y problemas de un **PreRestore** eventos a escritores implicados.|  
|[Restauración de finalización](end-restore.md)|Finaliza una sesión de restauración y problemas de un **PostRestore** eventos a escritores implicados.|  
|[reset](reset.md)|diskshadow se restablece al estado predeterminado.|  
|[list](list.md)|Los escritores de listas, las instantáneas o proveedores de instantáneas actualmente registrados que se encuentran en el sistema.|  
|[eliminar las sombras](delete-shadows.md)|eliminaciones de instantáneas.|  
|[import](import.md)|Importa una copia de instantáneas transportables de un archivo de metadatos cargados en el sistema.|  
|[mask](mask.md)|Quita las instantáneas de hardware que se importaron mediante el uso de la **importar** comando.|  
|[expose](expose.md)|expone una instantánea persistentes como una letra de unidad, un recurso compartido o un punto de montaje.|  
|[unexpose](unexpose.md)|Unexposes una instantánea que se expone mediante el **exponer** comando.|  
|[break_2](break_2.md)|Desasocia un volumen de instantáneas de VSS.|  
|[revert](revert.md)|Revierte un volumen en una copia de instantáneas especificada.|  
|[exit_1](exit_1.md)|diskshadow se cierra.|  
  
## <a name="remarks"></a>Comentarios  
  
-   como mínimo, sólo **agregar** y **crear** son necesarios para crear una instantánea. Sin embargo, este perderá todo el contexto y la opción de configuración, será una copia de seguridad y solo creará una instantánea con ninguna secuencia de comandos de ejecución de copia de seguridad.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Esto es un ejemplo de secuencia de comandos que va a crear una instantánea de copia de seguridad. Se puede guardar en archivo como script.dsh y ejecutar con diskshadow \/script.dsh s  
  
Se supone lo siguiente:  
  
-   Tiene un directorio existente llamado c:\\diskshadowdata.  
  
-   El volumen del sistema es C: y el volumen de datos es D:.  
  
-   Tiene un archivo backupscript.cmd en c:\\diskshadowdata.  
  
-   El archivo backupscript.cmd llevará a cabo la copia de instantáneas datos p: y p: en la unidad de copia de seguridad.  
  
Puede escribir estos comandos manualmente o escribirlas en script:  
  
```  
#diskshadow script file  
set context persistent nowriters  
set metadata c:\diskshadowdata\example.cab  
set verbose on  
begin backup  
add volume c: alias Systemvolumeshadow  
add volume d: alias Datavolumeshadow  
  
create  
  
expose %Systemvolumeshadow% p:  
expose %Datavolumeshadow% q:  
exec c:\diskshadowdata\backupscript.cmd  
end backup  
#End of script  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

