---
title: Convertir un registro de arranque maestro (MBR) en un disco de tabla de particiones GUID (GPT)
description: "Describe cómo convertir un registro de arranque maestro (MBR) en un disco de tabla de particiones GUID (GPT)"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f28d151d3b1d6b19b9ca330c5ff8576a53acbfa5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-master-boot-record-mbr-disk-into-a-guid-partition-table-gpt-disk"></a>Convertir un disco de registro de arranque maestro (MBR) en un disco de tabla de particiones GUID (GPT)

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los discos de registro de arranque maestro (MBR) usan la tabla de particiones BIOS estándar. Los discos de tabla de particiones GUID (GPT) usan Unified Extensible Firmware Interface (UEFI). Una ventaja de los discos GPT es que puedes tener más de cuatro particiones en cada disco. GPT también se requiere para discos de más de dos terabytes (TB).

Puedes cambiar un disco de MBR al estilo de partición GPT siempre que el disco no incluya particiones o volúmenes.
No puedes usar el estilo de partición GPT en un medio extraíble o en discos en clúster que estén conectados a buses compartidos SCSI o Fibre Channel que use el servicio de clúster.

> [!NOTE]
> Antes de convertir los discos, cierra todos los programas que se estén ejecutando en dichos discos.

## <a name="changing-an-mbr-disk-into-a-gpt-disk"></a>Convertir un disco MBR en un disco GPT

-   [Usar la interfaz de Windows](#BKMK_WINUI)
-   [Usar una línea de comandos](#BKMK_CMD)

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

<a id="BKMK_WINUI"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-the-windows-interface"></a>Para convertir un disco MBR en un disco GPT con la interfaz de Windows:

1.  Realiza una copia de seguridad o mueve los datos del disco MBR básico que quieras convertir en un disco GPT.

2.  Si el disco incluye particiones o volúmenes, haz clic con el botón derecho en cada uno de ellos y luego haz clic en **Eliminar partición** o **Eliminar volumen**.

3.  Haz clic con el botón derecho en el disco MBR que quieres convertir en un disco GPT y luego haz clic en **Convertir en disco GPT**.

<a id="BKMK_CMD"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-a-command-line"></a>Para convertir un disco MBR en un disco GPT con una línea de comandos

1.  Realiza una copia de seguridad o mueve los datos del disco MBR básico que quieras convertir en un disco GPT.

2.  Abre un símbolo del sistema con privilegios elevados haciendo clic con el botón derecho en **Símbolo del sistema** y luego elige **Ejecutar como administrador**.

3. Escribe `diskpart`. Si el disco no incluye particiones ni volúmenes, continúa al paso 6.

4.  En el símbolo del sistema **DISKPART**, escribe `list disk`. Anota el número de disco que quieras convertir.

5.  En el símbolo del sistema **DISKPART**, escribe `select disk <disknumber>`.

6.  En el símbolo del sistema **DISKPART**, escribe `clean`.

> [!NOTE]
> Ejecutar el comando **clean** eliminará todas las particiones o volúmenes en el disco.

7.  En el símbolo del sistema **DISKPART**, escribe `convert gpt`.

<br />

| Valor  | Descripción  |
| ----- | ----|
| <p>**list disk**</p> | <p>Muestra una lista de discos e información sobre ellos, como su tamaño, la cantidad de espacio libre disponible, si se trata de un disco básico o dinámico y si el disco utiliza el estilo de partición de Registro de arranque maestro (MBR) o Tabla de particiones GUID (GPT). El disco marcado con un asterisco (*) tiene el foco.</p> |
| <p>**select disk** <em>disknumber</em></p> | <p>Selecciona el disco especificado, donde <em>disknumber</em> es el número de disco y el que recibe el foco.</p> |
| <p>**clean**</p> | <p>Quita todas las particiones o volúmenes del disco con el foco.</p>  |
| <p>**convert gpt**</p>| <p>Convierte un disco básico vacío con el estilo de partición de registro de arranque maestro (MBR) en un disco básico con el estilo de partición de tabla de particiones GUID (GPT).</p> |

## <a name="see-also"></a>Consulta también

-   [Notación de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


