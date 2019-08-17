---
title: Comandos Diskpart
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7155dbf34f9986b3ebdd8b549b6a861cf7fcfe3a
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560437"
---
# <a name="diskpart-commands"></a>Comandos Diskpart

Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2, Windows Server 2008

Los comandos de Diskpart le ayudan a administrar las unidades del equipo (discos, particiones, volúmenes o discos duros virtuales). Antes de poder usar comandos de DiskPart, primero debe enumerar y, a continuación, seleccionar un objeto para darle el foco. Cuando un objeto tiene el foco, los comandos de Diskpart que escriba actuarán en ese objeto.

Puede enumerar los objetos disponibles y determinar el número o la letra de la unidad de un objeto mediante los comandos **List Disk, List Volume, List Partition**y **List vDisk** . Los comandos **List Disk, List vDisk** y **List Volume** muestran todos los discos y volúmenes del equipo. Sin embargo, el comando **List Partition** solo muestra las particiones en el disco que tiene el foco. Cuando se usan los comandos de **lista** , aparece un\*asterisco () junto al objeto que tiene el foco.

Al seleccionar un objeto, el foco permanece en ese objeto hasta que se selecciona otro objeto. Por ejemplo, si el foco se establece en el disco 0 y selecciona volumen 8 en el disco 2, el foco se desplaza desde el disco 0 al disco 2, volumen 8. Algunos comandos cambian automáticamente el foco. Por ejemplo, al crear una nueva partición, el foco cambia automáticamente a la nueva partición.

Solo puede dar el foco a una partición en el disco seleccionado. Cuando una partición tiene el foco, el volumen relacionado (si existe) también tiene el foco. Cuando un volumen tiene el foco, el disco y la partición relacionados también tienen el foco si el volumen se asigna a una única partición específica. Si no es así, se perderá el foco en el disco y la partición.

## <a name="diskpart-commands"></a>Comandos Diskpart

Para iniciar el intérprete de comandos Diskpart, en el símbolo del sistema, escriba:

`diskpart`

> [!IMPORTANT]
> La pertenencia al grupo local **administradores** , o equivalente, es lo mínimo necesario para ejecutar Diskpart. 

Puede ejecutar los siguientes comandos en el intérprete de comandos Diskpart:

  - [Active](active.md)  
      
  - [Agregar](add.md)  
      
  - [Quitar](assign.md)  
      
  - [Attach vDisk](attach-vdisk.md)  
      
  - [Sus](attributes.md)  
      
  - [Montaje automático](automount.md)  
      
  - [Eliminar](break.md)  
      
  - [Principio](clean.md)  
      
  - [Compact vDisk](compact-vdisk.md)  
      
  - [Verso](convert.md)  
      
  - [A](create.md)  
      
  - [Eliminar](delete.md)  
      
  - [Detach vDisk](detach-vdisk.md)  
      
  - [Detalle](detail.md)  
      
  - [Salir](exit.md)  
      
  - [Expandir vDisk](expand-vdisk.md)  
      
  - [Allá](extend.md)  
      
  - [Sistemas](filesystems.md)  
      
  - [Format](format.md)  
      
  - [GPT](gpt.md)  
      
  - [Ayuda](help.md)  
      
  - [Importaciónación](import.md)  
      
  - [Inactiva](inactive.md)  
      
  - [Lista](list.md)  
      
  - [Merge vDisk](merge-vdisk.md)  
      
  - [N](offline.md)  
      
  - [Pantalla](online.md)  
      
  - [Corregir](recover.md)  
      
  - [Real](rem.md)  
      
  - [Quitar](remove.md)  
      
  - [Resolver](repair.md)  
      
  - [Volver a examinar](rescan.md)  
      
  - [Tain](retain.md)  
      
  - [Red](san.md)  
      
  - [No](select.md)  
      
  - [Identificador de conjunto](set-id.md)  
      
  - [Prime](shrink.md)  
      
  - [UniqueID](uniqueid.md)  
      

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Cmdlets de almacenamiento en Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
