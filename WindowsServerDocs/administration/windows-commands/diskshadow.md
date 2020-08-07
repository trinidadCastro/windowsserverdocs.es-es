---
title: Diskshadow
description: Artículo de referencia para el comando DiskShadow, que es una herramienta que expone la funcionalidad que ofrece el servicio de instantáneas de volumen (VSS).
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3170cde50208eb54d1657ceee0c409d76ed3b806
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890803"
---
# <a name="diskshadow"></a>Diskshadow

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Diskshadow.exe es una herramienta que expone la funcionalidad que ofrece el servicio de instantáneas de volumen (VSS). De forma predeterminada, DiskShadow usa un intérprete de comandos interactivo similar al de Diskraid o Diskpart. DiskShadow también incluye un modo que admite scripts.

> [!NOTE]
> La pertenencia al grupo local Administradores, o equivalente, es lo mínimo necesario para ejecutar DiskShadow.

## <a name="syntax"></a>Sintaxis

En el modo interactivo, escriba lo siguiente en el símbolo del sistema para iniciar el intérprete de comandos de DiskShadow:

```
diskshadow
```

En modo de script, escriba lo siguiente, donde *script.txt* es un archivo de script que contiene comandos de DiskShadow:

```
diskshadow -s script.txt
```

### <a name="parameters"></a>Parámetros

Puede ejecutar los siguientes comandos en el intérprete de comandos de DiskShadow o a través de un archivo de script. Como mínimo, solo se necesitan **Agregar** y **crear** para crear una instantánea. Sin embargo, esto perderá la configuración de contexto y de opciones, será una copia de seguridad de copia y creará una instantánea sin ningún script de ejecución de copia de seguridad.

| Get-Help | Descripción |
| --------- | ----------- |
| [Set (comando)](set_2.md) | Establece el contexto, las opciones, el modo detallado y el archivo de metadatos para crear instantáneas. |
| [comando cargar metadatos](load-metadata.md) | Carga un archivo Metadata. cab antes de importar una instantánea transportable o carga los metadatos del escritor en el caso de una restauración. |
| [escritor (comando)](writer.md) | comprueba que un escritor o componente está incluido o excluye un escritor o componente del procedimiento de copia de seguridad o restauración. |
| [Agregar comando](add.md) | Agrega volúmenes al conjunto de volúmenes de los que se van a realizar instantáneas o agrega alias al entorno de alias. |
| [crear comando](create.md) | Inicia el proceso de creación de instantáneas con la configuración de contexto y opciones actual. |
| [exec (comando)](exec.md) | Ejecuta un archivo en el equipo local. |
| [comando iniciar copia de seguridad](begin-backup.md) | Inicia una sesión de copia de seguridad completa. |
| [comando finalizar copia de seguridad](end-backup.md) | Finaliza una sesión de copia de seguridad completa y emite un evento **backupcomplete** con el estado de escritor adecuado, si es necesario. |
| [comando Begin restore](begin-restore.md) | Inicia una sesión de restauración y emite un evento de **prerestauración** a escritores implicados. |
| [end restore (comando)](end-restore.md) | Finaliza una sesión de restauración y emite un evento **postrestore** a escritores implicados. |
| [restablecer comando](reset.md) | Restablece DiskShadow al estado predeterminado. |
| [List (comando)](list.md) | Enumera escritores, instantáneas o proveedores de instantáneas registrados actualmente en el sistema. |
| [eliminar Shadows (comando)](delete-shadows.md) | Elimina las instantáneas. |
| [comando importar](import.md) | Importa una instantánea transportable de un archivo de metadatos cargado en el sistema. |
| [Mask (comando)](mask.md) | Quita las instantáneas de hardware que se importaron mediante el comando de **importación** . |
| [exponer (comando)](expose.md) | Expone una instantánea persistente como letra de unidad, recurso compartido o punto de montaje. |
| [unexpote (comando)](unexpose.md) | Anula la exposición de una instantánea que se expuso mediante el comando de **exposición** . |
| [Break (comando)](break_2.md) | Desasocia un volumen de instantáneas de VSS. |
| [revert (comando)](revert.md) | Revierte un volumen a una instantánea especificada. |
| [Exit (comando)](exit.md) | Sale del intérprete de comandos o el script. |

## <a name="examples"></a>Ejemplos

Se trata de una secuencia de comandos de ejemplo que creará una instantánea para la copia de seguridad. Se puede guardar en el archivo como script. DSH y ejecutarse con `diskshadow /s script.dsh` .

Supongamos lo siguiente:

- Tiene un directorio existente denominado c: \\ diskshadowdata.

- El volumen del sistema es C: y el volumen de datos es D:.

- Tiene un archivo backupscript. cmd en c: \\ diskshadowdata.

- El archivo backupscript. cmd realizará la copia de los datos de instantáneas p: y q: en la unidad de copia de seguridad.

Puede escribir estos comandos manualmente o incluirlos en el script:

```
#Diskshadow script file
set context persistent nowriters
set metadata c:\diskshadowdata\example.cab
set verbose on
begin backup
add volume c: alias systemvolumeshadow
add volume d: alias datavolumeshadow

create

expose %systemvolumeshadow% p:
expose %datavolumeshadow% q:
exec c:\diskshadowdata\backupscript.cmd
end backup
#End of script
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
