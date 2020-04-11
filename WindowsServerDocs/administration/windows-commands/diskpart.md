---
title: DiskPart
description: Tema de comandos de Windows para **DiskPart**, que le ayuda a administrar las unidades del equipo.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: eb2921d8da4a4a29c4f700107ef5b6d7bfb41481
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122535"
---
# <a name="diskpart"></a>DiskPart

>Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2, Windows Server 2008

Los comandos de Diskpart le ayudan a administrar las unidades del equipo (discos, particiones, volúmenes o discos duros virtuales).

Antes de poder usar comandos de DiskPart, primero debe enumerar y, a continuación, seleccionar un objeto para darle el foco. Cuando un objeto tiene el foco, los comandos de Diskpart que escriba actuarán en ese objeto.

## <a name="list-available-objects"></a>Enumerar objetos disponibles

Puede enumerar los objetos disponibles y determinar el número o la letra de unidad de un objeto mediante:

- `list disk`: muestra todos los discos del equipo.

- `list volume`: muestra todos los volúmenes del equipo.

- `list partition`: muestra las particiones en el disco que tiene el foco en el equipo.

- `list vdisk`: muestra todos los discos virtuales del equipo.

Cuando use los comandos de **lista** , aparecerá un asterisco (*) junto al objeto que tiene el foco.

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

Puede ejecutar los siguientes comandos desde el intérprete de comandos de Diskpart:

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
| [Expandir vDisk](expand-vdisk.md) | expande un disco duro virtual (VHD) al tamaño que especifique. |
| [Allá](extend.md) | Extiende el volumen o la partición que tiene el foco, junto con su sistema de archivos, en un espacio libre (sin asignar) en un disco. |
| [Sistemas](filesystems.md) | Muestra información sobre el sistema de archivos actual del volumen que tiene el foco y enumera los sistemas de archivos que se admiten para formatear el volumen. |
| [Aplique](format.md) | Formatea un disco para aceptar archivos de Windows. |
| [GPT](gpt.md) | Asigna los atributos GPT a la partición que tiene el foco en discos básicos de tabla de particiones GUID (GPT). |
| [Ayuda](help.md) | Muestra una lista de los comandos disponibles o información de ayuda detallada sobre un comando especificado. |
| [Importar](import.md) | Importa un grupo de discos externos en el grupo de discos del equipo local. |
| [Inactiva](inactive.md) | Marca la partición del sistema o la partición de arranque con el foco como inactivo en discos básicos de registro de arranque maestro (MBR). |
| [Lista](list.md) | Muestra una lista de discos, de particiones en un disco, de volúmenes de un disco o de discos duros virtuales (VHD). |
| [Merge vDisk](merge-vdisk.md) | Combina un disco duro virtual (VHD) de diferenciación con su VHD primario correspondiente. |
| [N](offline.md) | Toma un disco o volumen en línea en el estado sin conexión. |
| [Pantalla](online.md) | Toma un disco o volumen sin conexión al estado en línea. |
| [Corregir](recover.md) | Actualiza el estado de todos los discos de un grupo de discos, intenta recuperar discos en un grupo de discos no válido y vuelve a sincronizar los volúmenes reflejados y los volúmenes RAID-5 que tienen datos obsoletos. |
| [Real](rem.md) | Proporciona una forma de agregar comentarios a un script. |
| [Quitar](remove.md) | Quita una letra de unidad o un punto de montaje de un volumen. |
| [Resolver](repair.md) | Repara el volumen RAID-5 que tiene el foco reemplazando la región del disco con el disco dinámico especificado. |
| [Volver a examinar](rescan.md) | Busca nuevos discos que pueden haberse agregado al equipo. |
| [Tain](retain.md) | Prepara un volumen dinámico simple existente para usarlo como volumen de arranque o del sistema. |
| [Red](san.md) | Muestra o establece la Directiva de red de área de almacenamiento (San) para el sistema operativo. |
| [No](select.md) | Desplaza el foco a un disco, partición, volumen o disco duro virtual (VHD). |
| [Identificador de conjunto](set-id.md) | Cambia el campo de tipo de partición para la partición que tiene el foco. |
| [Prime](shrink.md) | Reduce el tamaño del volumen seleccionado en la cantidad especificada. |
| [UniqueID](uniqueid.md) | Muestra o establece el identificador de la tabla de particiones GUID (GPT) o la firma del registro de arranque maestro (MBR) del disco que tiene el foco. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Información general de administración de discos](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Cmdlets de almacenamiento en Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
