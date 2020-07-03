---
title: diskpart
description: Artículo de referencia para el intérprete de comandos Diskpart, que le ayuda a administrar las unidades del equipo.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 87fc3a2e91b2f5ac22e87485d9258ef369ff0da0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929300"
---
# <a name="diskpart"></a>diskpart

> Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2, Windows Server 2008

El intérprete de comandos Diskpart le ayuda a administrar las unidades del equipo (discos, particiones, volúmenes o discos duros virtuales).

Antes de poder usar comandos de **DiskPart** , primero debe enumerar y, a continuación, seleccionar un objeto para darle el foco. Después de que un objeto tenga el foco, los comandos de Diskpart que escriba actuarán en ese objeto.

## <a name="list-available-objects"></a>Enumerar objetos disponibles

Puede enumerar los objetos disponibles y determinar el número o la letra de unidad de un objeto mediante:

- `list disk`-Muestra todos los discos del equipo.

- `list volume`-Muestra todos los volúmenes del equipo.

- `list partition`-Muestra las particiones en el disco que tiene el foco en el equipo.

- `list vdisk`-Muestra todos los discos virtuales del equipo.

Después de ejecutar los comandos de **lista** , aparecerá un asterisco (*) junto al objeto con el foco.

## <a name="determine-focus"></a>Determinar el foco

Al seleccionar un objeto, éste conserva el foco hasta que se selecciona otro objeto. Por ejemplo, si el foco se establece en el disco 0 y selecciona volumen 8 en el disco 2, el foco se desplaza desde el disco 0 al disco 2, volumen 8.

Algunos comandos cambian automáticamente el foco. Por ejemplo, al crear una nueva partición, el foco cambia automáticamente a la nueva partición.

Solo puede dar el foco a una partición en el disco seleccionado. Después de que una partición tenga el foco, el volumen relacionado (si existe) también tiene el foco. Una vez que un volumen tiene el foco, el disco y la partición relacionados también tienen el foco si el volumen se asigna a una única partición específica. Si no es así, se perderá el foco en el disco y la partición.

## <a name="syntax"></a>Sintaxis

Para iniciar el intérprete de comandos Diskpart, en el símbolo del sistema, escriba:

```
diskpart <parameter>
```

> [!IMPORTANT]
> Debe estar en el grupo de **administradores** locales, o un grupo con permisos similares, para ejecutar Diskpart.

### <a name="parameters"></a>Parámetros

Puede ejecutar los siguientes comandos desde el intérprete de comandos de Diskpart:

| Comando | Descripción |
| ------- | ----------- |
| [active](active.md) | Marca la partición del disco con el foco, como activa. |
| [add](add.md) | Refleja el volumen simple que tiene el foco en el disco especificado. |
| [assign](assign.md) | Asigna una letra de unidad o un punto de montaje al volumen que tiene el foco. |
| [attach vdisk](attach-vdisk.md) | Adjunta (a veces denominados montajes o superficies) un disco duro virtual (VHD) para que aparezca en el equipo host como una unidad de disco duro local. |
| [attributes](attributes.md) | Muestra, establece o borra los atributos de un disco o volumen. |
| [automount](automount.md) | Habilita o deshabilita la característica de montaje automático. |
| [break](break.md) | Divide el volumen reflejado que tiene el foco en dos volúmenes simples. |
| [clean](clean.md) | Quita todo el formato de particiones o volúmenes del disco que tiene el foco. |
| [compact vdisk](compact-vdisk.md) | Reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica. |
| [convert](convert.md) | Convierte los volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS, lo que deja intactos los archivos y directorios existentes. |
| [create](create.md) | Crea una partición en un disco, un volumen en uno o varios discos o un disco duro virtual (VHD). |
| [delete](delete.md) | Elimina una partición o un volumen. |
| [detach vdisk](detach-vdisk.md) | Detiene el disco duro virtual (VHD) seleccionado para que no aparezca como una unidad de disco duro local en el equipo host. |
| [detail](detail.md) | Muestra información sobre el disco, la partición, el volumen o el disco duro virtual (VHD) seleccionado. |
| [exit](exit.md) | Sale del intérprete de comandos Diskpart. |
| [expand vdisk](expand-vdisk.md) | Expande un disco duro virtual (VHD) al tamaño que especifique. |
| [extend](extend.md) | Extiende el volumen o la partición que tiene el foco, junto con su sistema de archivos, en un espacio libre (sin asignar) en un disco. |
| [filesystems](filesystems.md) | Muestra información sobre el sistema de archivos actual del volumen que tiene el foco y enumera los sistemas de archivos que se admiten para formatear el volumen. |
| [format](format.md) | Formatea un disco para aceptar archivos de Windows. |
| [gpt](gpt.md) | Asigna los atributos GPT a la partición que tiene el foco en discos básicos de tabla de particiones GUID (GPT). |
| [help](help.md) | Muestra una lista de los comandos disponibles o información de ayuda detallada sobre un comando especificado. |
| [import](import.md) | Importa un grupo de discos externos en el grupo de discos del equipo local. |
| [inactive](inactive.md) | Marca la partición del sistema o la partición de arranque con el foco como inactivo en discos básicos de registro de arranque maestro (MBR). |
| [list](list.md) | Muestra una lista de discos, de particiones en un disco, de volúmenes de un disco o de discos duros virtuales (VHD). |
| [merge vdisk](merge-vdisk.md) | Combina un disco duro virtual (VHD) de diferenciación con su VHD primario correspondiente. |
| [offline](offline.md) | Toma un disco o volumen en línea en el estado sin conexión. |
| [online](online.md) | Toma un disco o volumen sin conexión al estado en línea. |
| [recover](recover.md) | Actualiza el estado de todos los discos de un grupo de discos, intenta recuperar discos en un grupo de discos no válido y vuelve a sincronizar los volúmenes reflejados y los volúmenes RAID-5 que tienen datos obsoletos. |
| [rem](rem.md) | Proporciona una forma de agregar comentarios a un script. |
| [remove](remove.md) | Quita una letra de unidad o un punto de montaje de un volumen. |
| [repair](repair.md) | Repara el volumen RAID-5 que tiene el foco reemplazando la región del disco con el disco dinámico especificado. |
| [rescan](rescan.md) | Busca nuevos discos que pueden haberse agregado al equipo. |
| [retain](retain.md) | Prepara un volumen dinámico simple existente para usarlo como volumen de arranque o del sistema. |
| [san](san.md) | Muestra o establece la Directiva de red de área de almacenamiento (San) para el sistema operativo. |
| [select](select.md) | Desplaza el foco a un disco, partición, volumen o disco duro virtual (VHD). |
| [set id](set-id.md) | Cambia el campo de tipo de partición para la partición que tiene el foco. |
| [shrink](shrink.md) | Reduce el tamaño del volumen seleccionado en la cantidad especificada. |
| [uniqueid](uniqueid.md) | Muestra o establece el identificador de la tabla de particiones GUID (GPT) o la firma del registro de arranque maestro (MBR) del disco que tiene el foco. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Introducción a la administración de discos](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Cmdlets de almacenamiento en Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
