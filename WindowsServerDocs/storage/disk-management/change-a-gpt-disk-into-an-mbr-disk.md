---
title: Cambiar un disco de tabla de particiones GUID (GPT) por un disco de registro de arranque maestro (MBR)
description: Describe cómo cambiar un disco de tabla de particiones GUID (GPT) por un disco de estilo de partición de registro de arranque maestro (MBR).
ms.date: 06/19/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ae755b9c41d66ce5f907f600be17547398acc1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839586"
---
# <a name="convert-a-gpt-disk-into-an-mbr-disk"></a>Convertir un disco GPT en un disco MBR

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los discos de registro de arranque maestro (MBR) usan la tabla de particiones BIOS estándar. Los discos de tabla de particiones GUID (GPT) usan Unified Extensible Firmware Interface (UEFI). Los discos MBR no admiten más de cuatro particiones en cada disco. El método de partición MBR no se recomienda para los discos de más de dos terabytes (TB).

Puedes cambiar un disco de un estilo de partición GPT a un estilo de partición MBR siempre que el disco esté vacío y no incluya volúmenes.

> [!NOTE]
> Antes de convertir un disco, todos los datos de copia de seguridad y cierre todos los programas que se accede al disco.

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

<a id="BKMK_WINUI"></a>

## <a name="converting-using-the-windows-interface"></a>Conversión mediante la interfaz de Windows

1.  Realiza una copia de seguridad o mueve todos los volúmenes del disco GPT básico que quieras convertir en un disco MBR.

2.  Si el disco incluye particiones o volúmenes, haz clic con el botón derecho en cada uno de ellos y luego haz clic en **Eliminar volumen**.

3.  Haz clic con el botón derecho en el disco GPT que quieres convertir en un disco MBR y luego haz clic en **Convertir en disco MBR**.

<a id="BKMK_CMD"></a>

## <a name="converting-using-a-command-line"></a>Convertir la línea de comandos

1.  Realiza una copia de seguridad o mueve todos los volúmenes del disco GPT básico que quieras convertir en un disco MBR.

2.  Abre un símbolo del sistema con privilegios elevados haciendo clic con el botón derecho en **Símbolo del sistema** y luego elige **Ejecutar como administrador**.

3. Escriba `diskpart`. Si el disco no incluye particiones ni volúmenes, continúa al paso 6.

4.  En el símbolo del sistema **DISKPART**, escribe `list disk`. Anota el número de disco que quieres eliminar.

5.  En el símbolo del sistema **DISKPART**, escribe `select disk <disknumber>`.

6.  En el símbolo del sistema **DISKPART**, escribe `clean`.

    > [!IMPORTANT]
    > Ejecutar el comando **clean** eliminará todas las particiones o volúmenes en el disco.

7.  En el símbolo del sistema **DISKPART**, escribe `convert mbr`.

<br />

| Valor | Descripción |
| --- | --- |
| <p>**disco de la lista**</p> | <p>Muestra una lista de discos e información sobre ellos, como su tamaño, la cantidad de espacio libre disponible, si se trata de un disco básico o dinámico y si el disco utiliza el estilo de partición de Registro de arranque maestro (MBR) o Tabla de particiones GUID (GPT). El disco marcado con un asterisco (*) tiene el foco.</p> |
| <p>**Seleccione el disco**</p> | <p>Selecciona el disco especificado, donde <em>disknumber</em> es el número de disco y el que recibe el foco.</p> | <p>**clean**</p> | <p>Quita todas las particiones o volúmenes del disco con el foco.</p> |
| <p>**convert mbr**</p> | <p>Convierte un disco básico vacío con el estilo de partición de la tabla de particiones GUID (GPT) en un disco básico con el estilo de partición de registro de arranque maestro (MBR).</p>

## <a name="see-also"></a>Vea también

-   [Notación de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


