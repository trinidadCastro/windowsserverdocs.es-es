---
title: DiskPart
description: Tema de comandos de Windows para DiskPart, que le ayuda a administrar las unidades del equipo.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 45fe66e4843b96db8e4593c0e963e4a80dbd22c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845478"
---
# <a name="diskpart"></a>DiskPart

>Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2, Windows Server 2008

Los comandos de Diskpart le ayudan a administrar las unidades del equipo (discos, particiones, volúmenes o discos duros virtuales). 

Antes de poder usar comandos de DiskPart, primero debe enumerar y, a continuación, seleccionar un objeto para darle el foco. Cuando un objeto tiene el foco, los comandos de Diskpart que escriba actuarán en ese objeto.

## <a name="list-the-available-objects"></a>Enumerar los objetos disponibles

Puede enumerar los objetos disponibles y determinar el número o la letra de unidad de un objeto mediante:

- `list disk`: muestra todos los discos del equipo.

- `list volume`: muestra todos los volúmenes del equipo.

- `list partition`: muestra las particiones en el disco que tiene el foco en el equipo.

- `list vdisk`: muestra todos los discos virtuales del equipo.

Cuando use los comandos de **lista** , aparecerá un asterisco (\*) junto al objeto que tiene el foco.

## <a name="determine-focus"></a>Determinar el foco

Al seleccionar un objeto, éste conserva el foco hasta que se selecciona otro objeto. Por ejemplo, si el foco se establece en el disco 0 y selecciona volumen 8 en el disco 2, el foco se desplaza desde el disco 0 al disco 2, volumen 8.

Algunos comandos cambian automáticamente el foco. Por ejemplo, al crear una nueva partición, el foco cambia automáticamente a la nueva partición.

Solo puede dar el foco a una partición en el disco seleccionado. Cuando una partición tiene el foco, el volumen relacionado (si lo hay) también lo tiene. Cuando un volumen tiene el foco, el disco y la partición relacionados también lo tienen si el volumen se asigna a una única partición específica. Si no es así, se perderá el foco en el disco y la partición.

## <a name="diskpart-commands"></a>Comandos Diskpart

Para iniciar el intérprete de comandos Diskpart, en el símbolo del sistema, escriba:

```
diskpart
```

> [!IMPORTANT]
> Debe estar en el grupo de **administradores** locales, o un grupo con permisos similares, para ejecutar Diskpart. 

Puede ejecutar los siguientes comandos en el intérprete de comandos Diskpart:

| Comando | Descripción |
| ------- | ----------- |
| [Active](active.md) | Marca la partición del disco con el foco, como activa. |
| [Agregar](add.md) | Refleja el volumen simple que tiene el foco en el disco especificado. |
| [Quitar](assign.md) | Asigna una letra de unidad o un punto de montaje al volumen que tiene el foco. |
| [Attach vDisk](attach-vdisk.md) | Adjunta (a veces denominados montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local. |
| [Sus](attributes.md) | Muestra, establece o borra los atributos de un disco o volumen. |
| [Montaje automático](automount.md) | Habilita o deshabilita la característica de montaje automático. | 
| [Eliminar](break.md) | Divide el volumen reflejado que tiene el foco en dos volúmenes simples. |
| [Principio](clean.md) | Quita todo el formato de particiones o volúmenes del disco que tiene el foco. |
| [Compact vDisk](compact-vdisk.md) | Reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica. |
| [Verso](convert.md) | Convierte los volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS, lo que deja intactos los archivos y directorios existentes. |
| [A](create.md) | Crea una partición en un disco, un volumen en uno o varios discos o un disco duro virtual (VHD). |
| [Eliminar](delete.md) | Elimina una partición o un volumen. |
| [Detach vDisk](detach-vdisk.md) | Detiene el disco duro virtual (VHD) seleccionado para que no aparezca como una unidad de disco duro local en el equipo host. |
| [Detalle](detail.md) | Muestra información sobre el disco, la partición, el volumen o el disco duro virtual (VHD) seleccionado. |
| [Salir](exit.md) | Sale del intérprete de comandos de DiskPart. |
| [Expandir vDisk](expand-vdisk.md) | 
| [Allá](extend.md) | 
| [Sistemas](filesystems.md) | 
| [Aplique](format.md) | 
| [GPT](gpt.md) | 
| [Ayuda](help.md) | 
| [Importar](import.md) | 
| [Inactiva](inactive.md) | 
| [Lista](list.md) | 
| [Merge vDisk](merge-vdisk.md) | 
| [N](offline.md) | 
| [Pantalla](online.md) | 
| [Corregir](recover.md) | 
| [Real](rem.md) | 
| [Quitar](remove.md) | 
| [Resolver](repair.md) | 
| [Volver a examinar](rescan.md) | 
| [Tain](retain.md) | 
| [Red](san.md) | 
| [No](select.md) | 
| [Identificador de conjunto](set-id.md) | 
| [Prime](shrink.md) | 
| [UniqueID](uniqueid.md) | 

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos] (command-line-syntax-key.md

- [Información general de administración de discos](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Cmdlets de almacenamiento en Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
