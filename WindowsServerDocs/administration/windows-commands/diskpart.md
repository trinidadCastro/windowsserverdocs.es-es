---
Title: Comandos de DiskPart
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 54b34b6d8849caecae2123ddab91a658a4692ba3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849346"
---
# <a name="diskpart-commands"></a>Comandos de DiskPart


Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2, Windows Server 2008

Comandos de DiskPart ayudan a administrar las unidades de su PC (discos, particiones, volúmenes o discos duros virtuales). Para poder usar comandos de DiskPart, debe enumerar primero y, a continuación, seleccione un objeto para transferirle el foco. Cuando un objeto tiene el foco, los comandos de DiskPart que se escriben actuará en ese objeto.

Puede enumerar los objetos disponibles y determinar la letra de unidad o el número de un objeto mediante el **disco de la lista, el volumen de lista, la partición de la lista**, y **lista vdisk** comandos. El **disco de la lista, lista vdisk** y **lista volumen** comandos mostrarán todos los discos y volúmenes en el equipo. Sin embargo, el **lista partición** comando sólo muestra las particiones del disco que tiene el foco. Cuando se usa el **lista** comandos, un asterisco (\*) aparece junto al objeto con el foco.

Cuando se selecciona un objeto, el foco permanece en ese objeto hasta que haya seleccionado un objeto diferente. Por ejemplo, si se establece el foco en el disco 0 y selecciona el volumen 8 del disco 2, el foco cambiará del disco 0 a 2, volumen 8 del disco. Algunos comandos cambian automáticamente el foco. Por ejemplo, cuando se crea una nueva partición, el foco cambia automáticamente a la nueva partición.

Solo puede dar el foco a una partición en el disco seleccionado. Cuando una partición tiene el foco, el volumen relacionado (si existe) también tiene el foco. Cuando un volumen tiene el foco, el disco asociado y la partición también tiene el foco si el volumen se asigna a una única partición específica. Si esto no es así, se centran en el disco y partición se pierde.

## <a name="diskpart-commands"></a>Comandos de DiskPart

Para iniciar el intérprete de comandos de DiskPart, en el símbolo del sistema, escriba:

`diskpart`


> [!IMPORTANT]
> Pertenecer al grupo local <STRONG>administradores</STRONG> , o equivalente, es lo mínimo necesario para ejecutar DiskPart. 
<br>


Puede ejecutar los comandos siguientes en el intérprete de comandos de Diskpart:

  - [Active](active.md)  
      
  - [Agregar](add.md)  
      
  - [Asignar](assign.md)  
      
  - [Attach vdisk](attach-vdisk.md)  
      
  - [Atributos](attributes.md)  
      
  - [Montaje automático](automount.md)  
      
  - [salto](break.md)  
      
  - [Clean](clean.md)  
      
  - [Compactar vdisk](compact-vdisk.md)  
      
  - [Convertir](convert.md)  
      
  - [Crear](create.md)  
      
  - [Eliminar](delete.md)  
      
  - [Desasociar vdisk](detach-vdisk.md)  
      
  - [Detalle](detail.md)  
      
  - [Salir](exit.md)  
      
  - [Expandir vdisk](expand-vdisk.md)  
      
  - [Ampliar](extend.md)  
      
  - [Filesystems](filesystems.md)  
      
  - [Format](format.md)  
      
  - [GPT](gpt.md)  
      
  - [Ayuda](help.md)  
      
  - [Importar](import.md)  
      
  - [inactivo](inactive.md)  
      
  - [Lista](list.md)  
      
  - [Combinar vdisk](merge-vdisk.md)  
      
  - [Sin conexión](offline.md)  
      
  - [Online](online.md)  
      
  - [Recover](recover.md)  
      
  - [Rem](rem.md)  
      
  - [Quitar](remove.md)  
      
  - [Reparación](repair.md)  
      
  - [Rescan](rescan.md)  
      
  - [Retain](retain.md)  
      
  - [San](san.md)  
      
  - [Select](select.md)  
      
  - [Id. de conjunto](set-id.md)  
      
  - [Shrink](shrink.md)  
      
  - [Uniqueid](uniqueid.md)  
      

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Cmdlets de almacenamiento en Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/storage/)

