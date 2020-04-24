---
title: Asignar una ruta de acceso de carpeta de punto de montaje a una unidad
description: En este artículo se describe cómo asignar una ruta de acceso de carpeta de punto de montaje (en lugar de una letra de unidad) a una unidad.
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b2fda216b57fbf036ce20c40b4c8b38d44404f3c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80815538"
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>Asignar una ruta de acceso de carpeta de punto de montaje a una unidad

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Puedes usar la Administración de discos para asignar una ruta de acceso de carpeta de punto de montaje (en lugar de una letra de unidad) a la unidad. Las rutas de acceso de carpeta de punto de montaje solo están disponibles en carpetas vacías en volúmenes NTFS básicos o dinámicos.

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>Asignar una ruta de acceso de carpeta de punto de montaje a una unidad

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-by-using-the-windows-interface"></a>Para asignar una ruta de acceso de carpeta de punto de montaje a una unidad mediante la interfaz de Windows

1.  En el Administrador de discos, haz clic con el botón derecho en la partición o volumen donde quieras asignar la ruta de acceso de carpeta de punto de montaje. 
2. Haz clic en **Cambiar la letra y rutas de acceso de unidad** y luego haz clic en **Agregar**. 
3. Haz clic en **Montar en la siguiente carpeta NTFS vacía**.
4. Escribe la ruta de acceso a una carpeta vacía en un volumen NTFS o haz clic en **Examinar** para buscarla.

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>Para asignar una ruta de acceso de carpeta de punto de montaje a una unidad con una línea de comandos

1.  Abra un símbolo del sistema y escriba `diskpart`.

2.  En el símbolo de sistema **DISKPART**, escribe `list volume`, tomando nota del número de volumen al que quieres asignar la ruta de acceso.

3.  En el símbolo del sistema **DISKPART**, escribe `select volume <volumenumber>`. 

4. Selecciona el volumen simple *volumenumber* al que quieres asignar la ruta de acceso.

5.  En el símbolo del sistema **DISKPART**, escribe `assign [mount=<path>]`.

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>Para quitar una ruta de acceso de carpeta de punto de montaje a una unidad

-   Para quitar la ruta de acceso de carpeta de punto de montaje, haz clic en ella y luego haz clic en **Quitar**.

| Value | Descripción |
| --- | --- |
| **list volume** | Muestra una lista de volúmenes básicos y dinámicos en todos los discos. |
| **select volume**        | Selecciona el volumen especificado, donde <em>volumenumber</em> es el número de volumen y el que recibe el foco. Si no se especifica ningún volumen, el comando **select** muestra el volumen actual con el foco. Puedes especificar el volumen por número, letra de unidad o ruta de acceso de carpeta de punto de montaje. En un disco básico, si seleccionas un volumen, este también recibe el foco de partición correspondiente.|
| **assign** | <ul><li> Asigna una letra de unidad o una ruta de acceso de carpeta de punto de montaje al volumen con foco. Si no se especifica ninguna ruta de acceso de carpeta de punto de montaje o letra de unidad, se le asigna la siguiente letra de unidad disponible. Si la ruta de acceso de carpeta de punto de montaje o letra de unidad ya está en uso, se genera un error.</li>  <li>Con el comando **assign**, puedes cambiar la letra de unidad asociada a una unidad extraíble.</li> <li> No puedes asignar letras de unidad a volúmenes de arranque ni a volúmenes que incluyan el archivo de paginación. Además, no puedes asignar una letra de unidad a una partición del fabricante de equipos originales (OEM), partición de sistema EFI o una partición GPT que no sea una partición de datos básica.</li></ul> |
| **mount=** <em>ruta de acceso</em> | Especifica una carpeta NTFS vacía y existente donde residirá la unidad montada.  |

## <a name="additional-considerations"></a>Consideraciones adicionales

-   Si administras un equipo local o remoto, puedes buscar carpetas NTFS en dicho equipo.
-   Las rutas de acceso de carpeta de punto de montaje solo están disponibles en carpetas vacías en volúmenes NTFS básicos o dinámicos.
-   Para modificar una ruta de acceso de carpeta de punto de montaje, quítala y luego crea una nueva ruta de acceso de carpeta en la nueva ubicación. No se puede modificar la ruta de acceso de carpeta de punto de montaje directamente.
-   Al asignar una ruta de acceso de carpeta de punto de montaje a una unidad, usa el **Visor de eventos** para comprobar el registro del sistema en busca de errores o advertencias del servicio de clúster que indiquen errores en la ruta de acceso de carpeta de punto de montaje. Estos errores aparecerían como **ClusSvc** en la columna **Origen** y **Recurso de disco físico** en la columna **Categoría**.
-   También puedes crear una unidad montada usando el comando [mountvol](https://go.microsoft.com/fwlink/?linkid=64111).

## <a name="see-also"></a>Consulta también
-   [Notación de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


